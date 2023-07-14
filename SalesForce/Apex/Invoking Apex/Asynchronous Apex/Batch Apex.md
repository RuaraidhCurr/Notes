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
1. `execute` method:
```apex
public void execute(Database.BatchableContext BC, list<P>){}
```
The `execute` method is used too do the processing of each chunk of data. 
This method takes the following:
	\- A reference of the Database.BatchableContext object.
	\- A list of sObjects, such as List<sObject
1. `finish` method:
```apex
public void finish(Database.BatchableContext BC){}
```


## Using the System.scheduleBatch Method

^ffc109
