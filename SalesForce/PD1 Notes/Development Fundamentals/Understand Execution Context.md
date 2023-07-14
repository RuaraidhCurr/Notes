---
tags: Apex, Lightning Platform, Execution Context, Invoking Apex, Triggers, Trigger, Governor Limits
---
### What is Execution Context?
In the Lightning Platform world, code executes within an execution context. *IN A NUT SHELL* This context **represents the time between when the code is executed and when it ends**. 
*The apex code that you write is not always the only code that is executing*

### Methods of Invoking Apex
To understand how this works, you need to know all the ways Apex can be executed. 

| Method            | Description                                                  |
| ----------------- | ------------------------------------------------------------ |
| Database Trigger  | Invoked for a specific event on a custom or standard object. |
| Anonymous Apex    | Code snippets executed on the fly in Dev Console & other tools. |
| Asynchronous Apex |  Occurs when executing a future or queueable Apex, running a batch job, or scheduling Apex to run at a specified interval.                                |
| Web Services      |             Code that is exposed via SOAP or REST web services.                                                 | 
| Email Services | Code that is set up to process inbound email.| 
|           Visualforce or Lightning Pages                                                   |Visualforce controllers and Lightning components can execute Apex code automatically or when a user initiates an action, such as clicking a button. Lightning components can also be executed by Lightning processes and flows. | 

By default, Apex executes in system context. Apex code has access to all objects and fields. Object permissions, field-level security, and [[Apex & .NET basics#^c54648|Sharing]] rules aren’t applied for the current user. You can use the with [[Apex & .NET basics#^c54648|Sharing]] keyword to specify that the sharing rules for the current user be taken into account for a class.

### [[Apex Triggers|Trigger Essentials]]
Similar to triggers on SQL Server, Apex DB triggers execute programming logic before or after events to records in salesforce. [[Apex Triggers#^fcefe2|Trigger Types]]:
- before insert
- before update
- before delete
- after insert
- after update
- after delete
- after undelete
The [[Apex Triggers#^e0a31a|basic trigger syntax]] for a trigger looks like the following:

```apex
trigger TriggerName on ObjectName (trigger_events) {
   // code_block
}
```

## Working with Limits 
The two limits you will probably be the most concerned with involve the number of SOQL queries or DML statements. This leads to developers needing to go back and "bulkify" the code.
### Common [[Governor Limits]] ran into
- [[Apex Triggers]] can receive [[Governor Limits#^a78715|up to 200 records]] at once. 
- [[SOQL & SOSL Queries|SOQL Queries]] is [[Governor Limits#^a78715|100]]
- [[Adding and Retrieving Data with DML|DML]] statements are [[Governor Limits#^a78715|150]]