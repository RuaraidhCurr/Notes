---
tags: Apex, Asynchronous Apex, Queueable Apex, Schedule, Scheduled Apex, Apex Scheduler, Scheduled Jobs, Jobs, Schedulable
---

To invoke apex classes to run at specific times, firstly the class must be `Schedulable`, then  you can specify by using the schedule apex page in the Salesfroce UI or the `System.schedule` method. 

> [!Imporatnt]
> Execution time can be delayed based on service availability.
> 
> Governor Limits only allow for 100 scheduled jobs at one time. 

## Im