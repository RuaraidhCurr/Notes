---
tags: Apex, Asynchronous Apex, Queueable Apex, Transaction Finalizers, Finalizer Methods, Finalizer, Chaining jobs
---
## Transaction Finalizers
Transaction finalizers enables you to attach actions, with the `System.Finalizer` interface, to apex jobs using the [[Queueable Apex|queueable framework]] and are designed to specify specific actions when a queueable job succeeds or fails. e.g.
- Poll the status of `AsyncApexJob` using a [[SOQL & SOSL Queries|SOQL query]] and re-enqueue the job if it fails
- Fire `BatchApexErrorEvents` when a [[batch Apex]] method encounters an unhandled exception

A [[Queueable Apex|Queueable job]] that failed due to an unhandled exception can be successively re-enqueued five times by a transaction finalizer.

The `System.Finalizer` interface includes the execute method:
```apex
global void execute(System.FinalizerContext ctx) {}
```

The `System.FinalizerContext` Interface contains four methods
- `getAsyncApexJobId` method:
``` apex
global Id getAsyncApexJobId {}
```
Returns the ID of the [[Queueable Apex|Queueable job]]
- `getRequestId` method:
```apex
global String getRequestId {}
```
Returns the request ID, the unique string that identifies the request and can be used with the Event Monitoring logs
- `getResult` Method:
```apex
global System.ParentJobResult getResult {}
```
Returns the parent [[Asynchronous Apex|asynchronous apex]] queueable job which the finalizer is attached. Values: `SUCCESS`, `UNHANDLED_EXCEPTION`. 
- `getException` method:
```apex
global System.Exception getException {}
```
Returns exceptions when the job fails. e.g. when `getResults` is `UNHANDLED_EXCEPTION`.

Attach the finalizer to your Queueable jobs using the System.attachFinalizer method.
1. Define a class that implements the System.Finalizer interface.
2. Attach a finalizer within a Queueable job’s execute method. To attach the finalizer, invoke the System.attachFinalizer method, using as argument the instantiated class that implements the System.Finalizer interface.
    ```apex
    global void attachFinalizer(Finalizer finalizer) {}
    ```

### Examples

#### Logging Finalizer Example
```apex
public class LoggingFinalizer implements Finalizer, Queueable {

  // Queueable implementation
  // A queueable job that uses LoggingFinalizer to buffer the log
  // and commit upon exit, even if the queueable execution fails

    public void execute(QueueableContext ctx) {
        String jobId = '' + ctx.getJobId();
        System.debug('Begin: executing queueable job: ' + jobId);
        try {
            // Create an instance of LoggingFinalizer and attach it
            LoggingFinalizer f = new LoggingFinalizer();
            System.attachFinalizer(f);

            // While executing the job, log using LoggingFinalizer.addLog()
            // Note that addlog() modifies the Finalizer's state after it is attached 
            DateTime start = DateTime.now();
            f.addLog('About to do some work...', jobId);

            while (true) {
              // Results in limit error
            }
        } catch (Exception e) {
            System.debug('Error executing the job [' + jobId + ']: ' + e.getMessage());
        } finally {
            System.debug('Completed: execution of queueable job: ' + jobId);
        }
    }

  // Finalizer implementation
  // Logging finalizer provides a public method addLog(message,source) that allows buffering log lines from the Queueable job.
  // When the Queueable job completes, regardless of success or failure, the LoggingFinalizer instance commits this buffered log.
  // Custom object LogMessage__c has four custom fields-see addLog() method.

    // internal log buffer
    private List<LogMessage__c> logRecords = new List<LogMessage__c>();

    public void execute(FinalizerContext ctx) {
        String parentJobId = '' + ctx.getAsyncApexJobId();
        System.debug('Begin: executing finalizer attached to queueable job: ' + parentJobId);

        // Update the log records with the parent queueable job id
        System.Debug('Updating job id on ' + logRecords.size() + ' log records');
        for (LogMessage__c log : logRecords) {
            log.Request__c = parentJobId; // or could be ctx.getRequestId()
        }
        // Commit the buffer
        System.Debug('committing log records to database');
        Database.insert(logRecords, false);

        if (ctx.getResult() == ParentJobResult.SUCCESS) {
            System.debug('Parent queueable job [' + parentJobId + '] completed successfully.');
        } else {
            System.debug('Parent queueable job [' + parentJobId + '] failed due to unhandled exception: ' + ctx.getException().getMessage());
            System.debug('Enqueueing another instance of the queueable...');
        }
        System.debug('Completed: execution of finalizer attached to queueable job: ' + parentJobId);
    }

    public void addLog(String message, String source) {
        // append the log message to the buffer
        logRecords.add(new LogMessage__c(
            DateTime__c = DateTime.now(),
            Message__c = message,
            Request__c = 'setbeforecommit',
            Source__c = source
        ));
    }
}
```

#### Retry Queueable Example
```apex
public class RetryLimitDemo implements Finalizer, Queueable {

  // Queueable implementation
  public void execute(QueueableContext ctx) {
    String jobId = '' + ctx.getJobId();
    System.debug('Begin: executing queueable job: ' + jobId);
    try {
        Finalizer finalizer = new RetryLimitDemo();
        System.attachFinalizer(finalizer);
        System.debug('Attached finalizer');
        Integer accountNumber = 1;
        while (true) { // results in limit error
          Account a = new Account();
          a.Name = 'Account-Number-' + accountNumber;
          insert a;
          accountNumber++;
        }
    } catch (Exception e) {
        System.debug('Error executing the job [' + jobId + ']: ' + e.getMessage());
    } finally {
        System.debug('Completed: execution of queueable job: ' + jobId);
    }
  }

  // Finalizer implementation
  public void execute(FinalizerContext ctx) {
    String parentJobId = '' + ctx.getAsyncApexJobId();
    System.debug('Begin: executing finalizer attached to queueable job: ' + parentJobId);
    if (ctx.getResult() == ParentJobResult.SUCCESS) {
        System.debug('Parent queueable job [' + parentJobId + '] completed successfully.');
    } else {
        System.debug('Parent queueable job [' + parentJobId + '] failed due to unhandled exception: ' + ctx.getException().getMessage());
        System.debug('Enqueueing another instance of the queueable...');
        String newJobId = '' + System.enqueueJob(new RetryLimitDemo()); // This call fails after 5 times when it hits the chaining limit
        System.debug('Enqueued new job: ' + newJobId);
    }
    System.debug('Completed: execution of finalizer attached to queueable job: ' + parentJobId);
  }
}
```
