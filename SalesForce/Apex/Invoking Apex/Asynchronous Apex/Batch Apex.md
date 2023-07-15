Batch Apex is used to run, long-running processes that run thousands of records on the Lightning Platform. Batch apex runs small batches of records breaking down an entire dataset into manageable junks as to not hit [[Governor Limits]] or any other Org specific limits.

Batch apex jobs are invoked at run time using Apex. 

You can only have 5 batch jobs queued at one time. You can evaluate your current count by viewing the Scheduled Jobs page in Salesforce.

Batch jobs can also be programmatically scheduled to run at specific times using [[Apex Scheduler]]. 

### Batch Job Statuses 
The following table lists all possible statuses for a batch job along with a description of each.

|Status|Description|
|---|---|
|Holding|Job has been submitted and is held in the Apex flex queue until system resources become available to queue the job for processing.|
|Queued|Job is awaiting execution.|
|Preparing|The start method of the job has been invoked. This status can last a few minutes depending on the size of the batch of records.|
|Processing|Job is being processed.|
|Aborted|Job aborted by a user.|
|Completed|Job completed with or without failures.|
|Failed|Job experienced a system failure.|

# Using Batch Apex
To use Batch Apex write a Class that implements `Database.Batchable` and then invoke the class programmatically. 

## Implementing the Database.Batchable Interface

The Database.Batchable interface contains three methods that must be implemented.
1. `start` method:
```apex
public (Database.QueryLocator | Iterable<sObject>) start(Database.BatchableContext bc) {}
```
The `start` method Used to collect the records or objects to pass through to the execute method. 
>[!Important]
>When using [[SOQL & SOSL Queries|SOQL]] statements to generate data, use the `Database.QueryLocator` object to bypass any SOQL query limits. For example, a batch Apex job for the Account object can return a QueryLocator for all account records (up to 50 million records) in an org.

2. `execute` method:
```apex
public void execute(Database.BatchableContext BC, list<P>){}
```
The `execute` method is used too do the processing of each chunk of data. 
This method takes the following:
	\- A reference of the Database.BatchableContext object.
	\- A list of [[SObjects & Objects|sObjects]], such as List\<sObject\>, or a list of parameterized types. 
Batchable records execute in the order in which they are received. 

3. `finish` method:
```apex
public void finish(Database.BatchableContext BC){}
```
The `finish` method is used to send confirmation emails r to run post-processing operations. 

## Using Database.BatchableContext & Database.QueryLocator to define Scope

The following example uses the `Database.BatchableContext` to query the `AsyncApexJob` associated with the batch job and uses a Database.QueryLocator:
```apex
public class SearchAndReplace implements Database.Batchable<sObject>{

   public final String Query;
   public final String Entity;
   public final String Field;
   public final String Value;

   public SearchAndReplace(String q, String e, String f, String v){
      Query=q; Entity=e; Field=f;Value=v;
   }

   public Database.QueryLocator start(Database.BatchableContext BC){
      return Database.getQueryLocator(query);
   }

   public void execute(Database.BatchableContext BC, List<sObject> scope){
     for(sobject s : scope){
     s.put(Field,Value); 
     }
     update scope;
    }

   public void finish(Database.BatchableContext BC){
   AsyncApexJob a = [SELECT Id, Status, NumberOfErrors, JobItemsProcessed,
      TotalJobItems, CreatedBy.Email
      FROM AsyncApexJob WHERE Id =
      :BC.getJobId()];
   // Send an email to the Apex job's submitter notifying of job completion.
   Messaging.SingleEmailMessage mail = new Messaging.SingleEmailMessage();
   String[] toAddresses = new String[] {a.CreatedBy.Email};
   mail.setToAddresses(toAddresses);
   mail.setSubject('Apex Sharing Recalculation ' + a.Status);
   mail.setPlainTextBody
   ('The batch Apex job processed ' + a.TotalJobItems +
   ' batches with '+ a.NumberOfErrors + ' failures.');
   Messaging.sendEmail(new Messaging.SingleEmailMessage[] { mail });
   }
}
```

## Using an Iterable in Batch Apex to Define Scope 

The `start` method can return either a `Database.QuieryLocator` Object that contains the records to be used or an iterable. Use an iterable to step through the returned items more easily.
```apex
public class batchClass implements Database.batchable{ 
   public Iterable start(Database.BatchableContext info){ 
       return new CustomAccountIterable(); 
   }     
   public void execute(Database.BatchableContext info, List<Account> scope){
       List<Account> accsToUpdate = new List<Account>();
       for(Account a : scope){ 
           a.Name = 'true'; 
           a.NumberOfEmployees = 70; 
           accsToUpdate.add(a); 
       } 
       update accsToUpdate; 
   }     
   public void finish(Database.BatchableContext info){     
   } 
}
```

## Using the Database.executeBatch Method to Submit Batch Jobs

You can use the `Database.executeBatch` method to programmatically begin a batch job.
>[!Important]
>
When you call Database.executeBatch, Salesforce adds the process to the queue. Actual execution can be delayed based on service availability.

The `Database.executeBatch` takes two parameters :
1. An instance of a class that implements the `Database.Batchable` interface.
2. A parameter specifies the number of records to pass into the `execute` method.

The `Database.executeBatch` method returns the ID of the AsyncApexJob object, which you can use to track the progress of the job. For example:
```apex
ID batchprocessid = Database.executeBatch(reassign);

AsyncApexJob aaj = [SELECT Id, Status, JobItemsProcessed, TotalJobItems, NumberOfErrors 
                    FROM AsyncApexJob WHERE ID =: batchprocessid ];
```


## Using the System.scheduleBatch Method

^ffc109
You can use the `System.scheduleBatch` method to schedule batch's to run at a time in the future. 
The `System.scheduleBatch` takes the following parameters:
1. An instance of the class that implements the `Database.Batchable` interface
2. The job name
3. The time internal, in minutes, after which the job starts executing
4. An optional scope value. The number of records to pass into the `execute` method.
e.g. the example passes the method an instance of a batch class `reassign`,  job name `job example`, time interval of `60` minutes.
```apex
String cronID = System.scheduleBatch(reassign, 'job example', 60);

CronTrigger ct = [SELECT Id, TimesTriggered, NextFireTime
                FROM CronTrigger WHERE Id = :cronID];

// TimesTriggered should be 0 because the job hasn't started yet.
System.assertEquals(0, ct.TimesTriggered);
System.debug('Next fire time: ' + ct.NextFireTime); 
// For example:
// Next fire time: 2013-06-03 13:31:23
```
=
## Batch Apex Examples

The following example uses a Database.QueryLocator:
```apex
public class UpdateAccountFields implements Database.Batchable<sObject>{
   public final String Query;
   public final String Entity;
   public final String Field;
   public final String Value;

   public UpdateAccountFields(String q, String e, String f, String v){
             Query=q; Entity=e; Field=f;Value=v;
   }

   public Database.QueryLocator start(Database.BatchableContext BC){
      return Database.getQueryLocator(query);
   }

   public void execute(Database.BatchableContext BC, 
                       List<sObject> scope){
      for(Sobject s : scope){s.put(Field,Value); 
      }      update scope;
   }

   public void finish(Database.BatchableContext BC){
   }
}
```
You can use the following code to call the previous class.
```apex
// Query for 10 accounts
String q = 'SELECT Industry FROM Account LIMIT 10';
String e = 'Account';
String f = 'Industry';
String v = 'Consulting';
Id batchInstanceId = Database.executeBatch(new UpdateAccountFields(q,e,f,v), 5);
```

The following is an example of a batch Apex class for deleting records.
```apex
public class BatchDelete implements Database.Batchable<sObject> {
   public String query;

   public Database.QueryLocator start(Database.BatchableContext BC){
      return Database.getQueryLocator(query);
   }

   public void execute(Database.BatchableContext BC, List<sObject> scope){
      delete scope;
      DataBase.emptyRecycleBin(scope);
   }

   public void finish(Database.BatchableContext BC){
   }
}
```
This code calls the BatchDelete batch Apex class to delete old documents. The query selects all documents that are in a specified folder and that are older than a specified date to be deleted. 
```apex
BatchDelete BDel = new BatchDelete();
Datetime d = Datetime.now();
d = d.addDays(-1);
// Replace this value with the folder ID that contains
// the documents to delete.
String folderId = '00lD000000116lD';
// Query for selecting the documents to delete
BDel.query = 'SELECT Id FROM Document WHERE FolderId=\'' + folderId + 
    '\' AND CreatedDate < '+d.format('yyyy-MM-dd')+'T'+
    d.format('HH:mm')+':00.000Z';
// Invoke the batch job.
ID batchprocessid = Database.executeBatch(BDel);
System.debug('Returned batch process ID: ' + batchProcessId);
```

## Using State in Batch Apex
Each execution of a Batch Apex job is considered a discrete transaction. For example a job that contains 1000 records without a scope parameter is considered 5 transactions of 200 records each.

If you Specify `Database.Stateful` in the class definition, you can maintain state across these transactions. only instanced member variables retain their values between transitions when using `Database.Stateful`. so for example it is useful for counting or summarizing records as they're being processed. 

## Testing Batch Apex
You can only test one execution of the execute method for Batch Apex. Use the *scope* parameter to make sure you do not hit [[Governor Limits]]. 

When testing Batch Apex, make sure to use the `startTest` and `stopTest` around the `executeBatch` method to insure that the finished before testing against the results.  

If you want to handle exceptions in the test method enclose the code in `try` & `catch` statements. 
``` apex
public static testMethod void testBatch() {
   user u = [SELECT ID, UserName FROM User 
             WHERE username='testuser1@acme.com'];
   user u2 = [SELECT ID, UserName FROM User 
              WHERE username='testuser2@acme.com'];
   String u2id = u2.id;

   List <Account> accns = new List<Account>();
      for(integer i = 0; i<200; i++){
         Account a = new Account(Name='testAccount'+ i, 
                     Ownerid = u.ID); 
         accns.add(a);
      }
   
   insert accns;
   
   Test.StartTest();
   OwnerReassignment reassign = new OwnerReassignment();
   reassign.query='SELECT ID, Name, Ownerid ' +
            'FROM Account ' +
            'WHERE OwnerId=\'' + u.Id + '\'' +
            ' LIMIT 200';
   reassign.email='admin@acme.com';
   reassign.fromUserId = u.Id;
   reassign.toUserId = u2.Id;
   ID batchprocessid = Database.executeBatch(reassign);
   Test.StopTest();

   System.AssertEquals(
           database.countquery('SELECT COUNT()'
              +' FROM Account WHERE OwnerId=\'' + u2.Id + '\''),
           200);  
   
   }
}
```

## Batch Apex Limitations
- Up to 5 batch jobs can be queued or active concurrently
- Up to 100 `Holding` batch jobs can be held in the Apex flex queue
- Max of 5 batch jobs in a running test
- Max number of Batch methods executions is 250,000
- A max of 50mil records can be returned in the `Databse.QueryLocator` object. 
- If `start` method returns a `QueryLocator`, the *Scope* param of `Database.executeBatch` can have a max of 200.  
- If no scope is given for the `Database.executeBatch`, Salesforce chunks the records into batches of 200 records. 
- Only 1 batch `start` method can run at a time in an org. Other jobs remain in queue until started.

# Firing Platform Events from Batch Apex
Batch Classes can fire platform events when encountering an error or exception. 

The `BatchApexErrorEvent` object represents a platform event associated with that batch class. If the `start`, `execute` or `finish` methods encounter an unhandled error or exception, a `BatchApexErrorEvent` platform event is fired. 

## Example
This example creates a trigger to determine which accounts failed in the batch transaction. 
```apex
trigger MarkDirtyIfFail on BatchApexErrorEvent (after insert) {
    Set<Id> asyncApexJobIds = new Set<Id>();
    for(BatchApexErrorEvent evt:Trigger.new){
        asyncApexJobIds.add(evt.AsyncApexJobId);
    }
    
    Map<Id,AsyncApexJob> jobs = new Map<Id,AsyncApexJob>(
        [SELECT id, ApexClass.Name FROM AsyncApexJob WHERE Id IN :asyncApexJobIds]
    );
    
    List<Account> records = new List<Account>();
    for(BatchApexErrorEvent evt:Trigger.new){
        //only handle events for the job(s) we care about
        if(jobs.get(evt.AsyncApexJobId).ApexClass.Name == 'AccountUpdaterJob'){
            for (String item : evt.JobScope.split(',')) {
                Account a = new Account(
                    Id = (Id)item,
                    ExceptionType__c = evt.ExceptionType,
                    Dirty__c = true
                );
                records.add(a);
            }
        }
    }
    update records;
}
```
