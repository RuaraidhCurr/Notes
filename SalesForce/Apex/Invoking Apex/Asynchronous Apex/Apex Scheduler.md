---
tags: Apex, Asynchronous Apex, Queueable Apex, Schedule, Scheduled Apex, Apex Scheduler, Scheduled Jobs, Jobs, Schedulable
---

To invoke apex classes to run at specific times, firstly the class must be `Schedulable`, then  you can specify by using the schedule apex page in the Salesfroce UI or the `System.schedule` method. 

> [!Imporatnt]
> Execution time can be delayed based on service availability.
> 
> Governor Limits only allow for 100 scheduled jobs at one time. 

## Implementing the Schedulable Interface

The Schedulable interface contains one method that must be implemented, execute.
```apex
global void execute(SchedulableContext sc){}
```

The following example implements the Schedulable interface for a class called MergeNumbers:
```apex
global class ScheduledMerge implements Schedulable {
   global void execute(SchedulableContext SC) {
      MergeNumbers M = new MergeNumbers(); 
   }
}
// To implement the class, execute this example in the Developer Console.

ScheduledMerge m = new ScheduledMerge();
String sch = '20 30 8 10 2 ?';
String jobID = System.schedule('Merge Job', sch, m);
```

To use `Schedulable` interface with [[Batch Apex|batch classes]] use the following `Batchable` interface
```apex
global class ScheduledBatchable implements Schedulable {
   global void execute(SchedulableContext sc) {
      Batchable b = new Batchable(); 
      Database.executeBatch(b);
   }
}
```
*\*An easier way to schedule a batch job is to call the [[Batch Apex^Using the System.scheduleBatch Method|System.scheduleBatch]] "HTML (New Window)") method without having to implement the Schedulable interface.

## Tracking Progress of Scheduled Jobs
you can track the progress of scheduled jobs running [[SOQL & SOSL Queries|SOQL queries]] on `CornTrigger`.   
```apex
CronTrigger ct = 
    [SELECT TimesTriggered, NextFireTime
    FROM CronTrigger WHERE Id = :jobID];
```
If the ID is not known use `getTriggerId()`
```apex
CronTrigger ct = 
    [SELECT TimesTriggered, NextFireTime
    FROM CronTrigger WHERE Id = :sc.getTriggerId()];
```
You can also get the job’s name and the job’s type from the CronJobDetail record. To do so, use the `CronJobDetail` relationship when performing a query on CronTrigger.
```apex
CronTrigger job = 
    [SELECT Id, CronJobDetail.Id, CronJobDetail.Name, CronJobDetail.JobType 
    FROM CronTrigger ORDER BY CreatedDate DESC LIMIT 1];
```
Alternatively, you can query CronJobDetail directly to get the job’s name and type.
```apex
CronJobDetail ctd = 
    [SELECT Id, Name, JobType 
    FROM CronJobDetail WHERE Id = :job.CronJobDetail.Id];
```
