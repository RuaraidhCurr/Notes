The Custom Object `Async_Request__c` is used to run tasks asynchronously and is handled mainly through the `TriggerOnAsyncRequest.trigger`. 

## Important fields
- **`Processing_Delay__c`**: This formula calculates the time difference between "Latest_Attempt__c" and either "Resumed_Datetime__c" or "CreatedDate", and then converts it into minutes.
- **`Type__c`** This specifies the type of asyncrequest it is and depending on the type will run different actions. These actions can be found in `ProcessAsyncRequest.cls` class.
- **`Latest_Attempt__c`**: Latest attempt is a date/time field that identifies the time of the first attempt to run the process

This trigger does the following:
1. Firstly checks to see if the trigger is running **BEFORE** async record insertion
2. 