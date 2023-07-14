Queueable apex is apex that is added to the queueable interface. the queueable interface allows you to add jobs to a queue and monitor them. it is an enhanced way of running apex compared to using [[future methods]]. 
Typical processes that run for a long time e.g. extensive database operations or external web call outs can be run asynchronously by using the Queueable interface. meaning these jobs can run in the background on its own thread and not delay any execution of your main apex processes. 
Reason of using Queueable apex is because [[Governor Limits]] are higher on Asynchronous apex than on synchronous apex, such as [[Governor Limits#^a78715|heap size limits]].

Queueable jobs are similar to future methods in the same way that they both queue for execution but Queueable jobs have these added benefits:
- When you invoke your job using the `System.enqueueJob` it returns a job ID. you can use this ID to  monitor your job through the *salesforce jobs page*, or by querying your record on the AsyncApexJob. 
- Your queueable class can contain member variables of non-primitive data types, such as [[SObjects & Objects|sObjects]] or custom Apex types.
- You can chain one job to another job by starting a second job from a running job.

### Example
This example implements the `Queueable` interface. The `execute` method in this example inserts a new account. The `System.enqueueJob(queueable)` method is used to add the job to the queue.
```apex
public class AsyncExecutionExample implements Queueable {
    public void execute(QueueableContext context) {
        Account a = new Account(Name='Acme',Phone='(415) 555-1212');
        insert a;        
    }
}
```
To add this class as a job on the queue, call this method:
```apex
ID jobID = System.enqueueJob(new AsyncExecutionExample());
```
To query information about your submitted job, perform a SOQL query on `AsyncApexJob` by filtering on the job ID that the `System.enqueueJob` method returns. Example:
```apex
AsyncApexJob jobInfo = [SELECT Status,NumberOfErrors FROM AsyncApexJob WHERE Id=:jobID];
```
Use the `System.enqueueJob(queueable, delay)` method to add queueable jobs to the asynchronous execution queue with a specified minimum delay (0–10 minutes). The delay is ignored during Apex testing.
``` apex
Integer delayInMinutes = 5;
ID jobID = System.enqueueJob(new MyQueueableClass(), delayInMinutes);
```
