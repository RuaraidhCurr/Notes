The Custom Object `Async_Request__c` is used to run tasks asynchronously and is handled mainly through the [[TriggerOnAsyncRequest||`TriggerOnAsyncRequest.trigger`]] & a platform event called [[TriggerOnAsyncAttempt||`TriggerOnAsyncAttempt.trigger`]] 

*The below is an initial understanding of the Async custom object. It may require further updating.*

We use this object in a way to run processes asynchronously. How it works is that a record is created it will have a identifier (like subject or name or something) that will be picked up in the Async trigger handler class. Depending on the identifier it will then run a specific platform event which carries out specified process. 

## Important fields
- **`Processing_Delay__c`**: This formula calculates the time difference between "Latest_Attempt__c" and either "Resumed_Datetime__c" or "CreatedDate", and then converts it into minutes.
- **`Type__c`** This specifies the type of asyncrequest it is and depending on the type will run different actions. These actions can be found in `ProcessAsyncRequest.cls` class.
- **`Latest_Attempt__c`**: Latest attempt is a date/time field that identifies the time of the first attempt then any following attempts to run the process. This is done on the platform event trigger called `TriggerOnAsyncAttempt`. 