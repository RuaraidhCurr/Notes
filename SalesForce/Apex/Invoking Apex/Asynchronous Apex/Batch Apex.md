Batch Apex is used to run, long-running processes that run thousands of records on the Lightning Platform. Batch apex runs small batches of records breaking down an entire dataset into manageable junks as to not hit [[Governor Limits]] or any other Org specific limits.

Batch apex jobs are invoked at run time using Apex. 

You can only have 5 batch jobs queued at one time. You can evaluate your current count by viewing the Scheduled Jobs page in Salesforce.

Batch jobs can also be programmatically scheduled to run at specific times using [[Apex Scheduler]]. 

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
	\- A list of sObjects, such as List\<sObject\>, or a list of parameterized types. 
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



## Using the System.scheduleBatch Method

^ffc109
