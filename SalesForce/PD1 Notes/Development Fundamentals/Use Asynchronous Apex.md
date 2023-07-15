An asynchronous process is a process or function that executes a task "in the background" without the user having to wait for the task to finish.

## When to Go Asynchronous
The following are three of the main reason behind using asynchronous programming
1. Processing a very large number of records. 
2. Making callouts to external web services.
3. Creating a better and faster user experience

## [[Future Methods]] 
In situations where you need to make a callout top a web service or want to offload processing to an asynchronous task, creating a [[Future Methods|future method]] could be the way to go. 

Channing a method from synchronous to asynchronous processing is amazingly easy. Essentially, you just add the `@future` annotation to your static void method. e.g.
```apex
public class MyFutureClass {
    @future(callout=true)    
    static void myFutureMethod(Set<Id> ids) {
        List<Contact> contacts = [SELECT Id, LastName, FirstName, Email
            FROM Contact WHERE Id IN :ids];
        for (Contact con: contacts) {
            String response = anotherClass.calloutMethod(con.Id,
                con.FirstName,
                con.LastName,
                con.Email);
        }
    }
}
```

### [[Future Methods#^4bd109|Future Limitations]] 
- You can't track execution because no apex job ID is retuned 
- Params must be given in primitive data types 
- You can't chain future methods and have one call another

## [[Batch Apex|Batch or Scheduled Apex]]
When you want to process a large number of records e.g. something up to 50mil records, batchable apex is your answer.

To use it you class implements the `Database.Batchable` interface and is required to define the `start()`, `execute()`, and `finish()` methods. 

you invoke a Batch Class using the `Database.executeBatch` method. 

``` apex
global class MyBatchableClass implements Database.Batchable<sObject>, Database.Stateful {  
    // Used to record the total number of Accounts processed
    global Integer numOfRecs = 0;
    // Used to gather the records that will be passed to the interface method
    // This method will only be called once and will return either a
    // Database.QueryLocator object or an Iterable that contains the records
    // or objects passed to the job.            
    global Database.QueryLocator start(Database.BatchableContext bc) {
        return Database.getQueryLocator('SELECT Id, Name FROM Account');                
    }
    // This is where the actual processing occurs as data is chunked into
    // batches and the default batch size is 200.
    global void execute(Database.BatchableContext bc, List<Account> scope) {
        for (Account acc : scope) {
            // Do some processing here
            // and then increment the counter variable
            numOfRecs = numOfRecs + 1;
        }     
    }
    // Used to execute any post-processing that may need to happen. This
    // is called only once and after all the batches have finished.
    global void finish(Database.BatchableContext bc) {
        EmailManager.sendMail('someAddress@somewhere.com',
                              numOfRecs + ' Accounts were processed!',
                              'Meet me at the bar for drinks to celebrate');            
    }
}
```
You could then invoke the batch class using anonymous code such as this:
```apex
MyBatchableClass myBatchObject = new MyBatchableClass();
Database.executeBatch(myBatchObject);
```

### [[Batch Apex#^2dbcf1|batchable Limitations]] 
The bathcable interface is great, but consider its limitations.
- Troubleshooting can be troublesome. 
- Jobs are queued and subject to server availability, which can take longer than anticipated.
-  