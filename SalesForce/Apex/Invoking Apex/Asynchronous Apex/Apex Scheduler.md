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

## Testing Apex Scheduler
When testing scheduled apex, you must ensure that the scheduled job is finished before testing against the results. to do this you should use `startTest` & `stopTest` for checking you results with `System.assertEquals`. e.g.
```apex
@istest
class TestClass {

   static testmethod void test() {
   Test.startTest();

      Account a = new Account();
      a.Name = 'testScheduledApexFromTestMethod';
      insert a;

      // Schedule the test job

      String jobId = System.schedule('testBasicScheduledApex',
      TestScheduledApexFromTestMethod.CRON_EXP, 
         new TestScheduledApexFromTestMethod());

      // Get the information from the CronTrigger API object
      CronTrigger ct = [SELECT Id, CronExpression, TimesTriggered, 
         NextFireTime
         FROM CronTrigger WHERE id = :jobId];

      // Verify the expressions are the same
      System.assertEquals(TestScheduledApexFromTestMethod.CRON_EXP, 
         ct.CronExpression);

      // Verify the job has not run
      System.assertEquals(0, ct.TimesTriggered);

      // Verify the next time the job will run
      System.assertEquals('2042-09-03 00:00:00', 
         String.valueOf(ct.NextFireTime));
      System.assertNotEquals('testScheduledApexFromTestMethodUpdated',
         [SELECT id, name FROM account WHERE id = :a.id].name);

   Test.stopTest();

   System.assertEquals('testScheduledApexFromTestMethodUpdated',
   [SELECT Id, Name FROM Account WHERE Id = :a.Id].Name);

   }
}
```

## Using the System.schedule Method
After definiens a class with `Schedulable` interface, use the `System.schedule` method to execute it.  `System.schedule` takes three arguments: 
1. a name for the job 
2. an expression used to represent the time and date the job is scheduled to run
3. the name of the class.
e.g.
```apex
Seconds Minutes Hours Day_of_month Month Day_of_week Optional_year
```

The following are the values for the expression:

|Name|Values|Special Characters|
|---|---|---|
|Seconds|0–59|None|
|Minutes|0–59|None|
|Hours|0–23|, - * /|
|Day_of_month|1–31|, - * ? / L W|
|Month|1–12 or the following:<br>- JAN<br>- FEB<br>- MAR<br>- APR<br>- MAY<br>- JUN<br>- JUL<br>- AUG<br>- SEP<br>- OCT<br>- NOV<br>- DEC|, - * /|
|Day_of_week|1–7 or the following:<br>- SUN<br>- MON<br>- TUE<br>- WED<br>- THU<br>- FRI<br>- SAT|, - * ? / L #|
|optional_year|null or 1970–2099|, - * /|

## [[Governor Limits|Apex Scheduler Limits]]

You can only have 100 scheduled Apex job at any one time. The maximum number of scheduled apex executions per 24hr period is 250,000, or the number of users you have on the org multiplied by 200.