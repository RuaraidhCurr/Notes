---
tags: Apex, Asynchronous Apex, Queueable Apex, queueable time delay, time delay, stack depth, Chaining jobs
---
## Queueable Apex

Queueable apex is apex that is added to the queueable interface. the queueable interface allows you to add jobs to a queue and monitor them. it is an enhanced way of running apex compared to using [[Future Methods|future methods]]. 
Typical processes that run for a long time e.g. extensive database operations or external web call outs can be run asynchronously by using the Queueable interface. meaning these jobs can run in the background on its own thread and not delay any execution of your main apex processes. 
Reason of using Queueable apex is because [[Governor Limits]] are higher on Asynchronous apex than on synchronous apex, such as [[Governor Limits#^a78715|heap size limits]].

Queueable jobs are similar to future methods in the same way that they both queue for execution but Queueable jobs have these added benefits:
- When you invoke your job using the `System.enqueueJob` it returns a job ID. you can use this ID to  monitor your job through the *salesforce jobs page*, or by querying your record on the AsyncApexJob. 
- Your queueable class can contain member variables of non-primitive data types, such as [[SObjects & Objects|sObjects]] or custom Apex types.
- You can chain one job to another job by starting a second job from a running job.

### Examples
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

##### Queueable with time delay
Use the `System.enqueueJob(queueable, delay)` method to add queueable jobs to the asynchronous execution queue with a specified minimum delay (0–10 minutes). The delay is ignored during Apex testing.
``` apex
Integer delayInMinutes = 5;
ID jobID = System.enqueueJob(new MyQueueableClass(), delayInMinutes);
```

##### Queueable with Specific Stack Depth
Use the `System.enqueueJob(queueable, asyncOptions)` method where you can specify the maximum stack depth and the minimum queue delay in the asyncOptions parameter.

The `System.AsyncInfo` class has methods to help you determine if maximum stack depth is set in your Queueable request and to get the stack depths and queue delay for your queueables that are currently running. Use information about the current queueable execution to make decisions on adjusting delays on subsequent calls.

These are methods in the System.AsyncInfo class.
- `hasMaxStackDepth()`
- `getCurrentQueueableStackDepth()`
- `getMaximumQueueableStackDepth()`
- `getMinimumQueueableDelayInMinutes()`

``` apex
// Fibonacci
public class FibonacciDepthQueueable implements Queueable {
   
    private integer nMinus1, nMinus2;
       
    public static void calculateFibonacciTo(integer depth) {
        AsyncOptions asyncOptions = new AsyncOptions();
        asyncOptions.MaximumQueueableStackDepth = depth;
        System.enqueueJob(new FibonacciDepthQueueable(null, null), asyncOptions);
    }
       
    private FibonacciDepthQueueable(integer nMinus1param, integer nMinus2param) {
        nMinus1 = nMinus1param;
        nMinus2 = nMinus2param;
    }
   
    public void execute(QueueableContext context) {
       
        integer depth = AsyncInfo.getCurrentQueueableStackDepth();
       
        // Calculate step
        integer fibonacciSequenceStep;
        switch on (depth) {
            when 1, 2 {
                fibonacciSequenceStep = 1;
            }
            when else {
                fibonacciSequenceStep = nMinus1 + nMinus2;
            }
        }
       
        System.debug('depth: ' + depth + ' fibonacciSequenceStep: ' + fibonacciSequenceStep);
       
        if(System.AsyncInfo.hasMaxStackDepth() &&
           AsyncInfo.getCurrentQueueableStackDepth() >= 
           AsyncInfo.getMaximumQueueableStackDepth()) {
            // Reached maximum stack depth
            Fibonacci__c result = new Fibonacci__c(
                Depth__c = depth,
                Result = fibonacciSequenceStep
                );
            insert result;
        } else {
            System.enqueueJob(new FibonacciDepthQueueable(fibonacciSequenceStep, nMinus1));
        }
    }
}

```

##### Testing Queueable Jobs
To ensure that this process runs within the test method, the job is submitted to the queue between the `Test.startTest` and `Test.stopTest` block. The system executes all asynchronous processes started in a test method synchronously after the `Test.stopTest` statement. Next, the test method verifies the results of the queueable job by querying the account that the job created.
``` apex
@isTest
public class AsyncExecutionExampleTest {
    static testmethod void test1() {
        // startTest/stopTest block to force async processes to run the test
        Test.startTest();        
        System.enqueueJob(new AsyncExecutionExample());
        Test.stopTest();
        
        // Validate that the job has run by verifying that the record was created.
        Account acct = [SELECT Name,Phone FROM Account WHERE Name='Acme' LIMIT 1];
        System.assertNotEquals(null, acct);
        System.assertEquals('(415) 555-1212', acct.Phone);
    }
}
```

##### Chaining Jobs
To chain jobs together use the `execute()` method. You can only chain one job from an executing job. (one child job can exist for each parent job *-1to1 ratio-*) 
``` apex
public class AsyncExecutionExample implements Queueable {
    public void execute(QueueableContext context) {
        // Your processing logic here       

        // Chain this job to next job by submitting the next job
        System.enqueueJob(new SecondJob());
    }
}
```

### Queueable Apex Limits
The execution of one job counts against the shared limit for asynchronous Apex methods. [[Governor Limits]]*
	- You can add up to 50 jobs to the queue with `System.enqueueJob` in a single transaction. In asynchronous transactions (for example, from a batch Apex job), you can add only one job to the queue with `System.enqueueJob`.
	- Because no limit is enforced on the depth of chained jobs, you can chain one job to another. You can repeat this process with each new child job to link it to a new child job.
	- When chaining jobs with `System.enqueueJob`, you can add only one job from an executing job.