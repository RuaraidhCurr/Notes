The Custom Object `Async_Request__c` is used to run tasks asynchronously and is handled mainly through the `TriggerOnAsyncRequest.trigger` & a platform event called `TriggerOnAsyncAttempt.trigger`
## Important fields
- **`Processing_Delay__c`**: This formula calculates the time difference between "Latest_Attempt__c" and either "Resumed_Datetime__c" or "CreatedDate", and then converts it into minutes.
- **`Type__c`** This specifies the type of asyncrequest it is and depending on the type will run different actions. These actions can be found in `ProcessAsyncRequest.cls` class.
- **`Latest_Attempt__c`**: Latest attempt is a date/time field that identifies the time of the first attempt then any following attempts to run the process. This is done on the platform event trigger called `TriggerOnAsyncAttempt`. 

## TriggerOnAsyncRequest.trigger
Trigger that runs before and after insertion or update of an `Async_Request__c` record
1. Firstly checks to see if the trigger is running **BEFORE** async record insertion
2. It quireis the most recent record from the `Async_Request__c` object that has a non-null `Lastest_Attempt__c` field, less than 3 retry attempts, & not the type ofg