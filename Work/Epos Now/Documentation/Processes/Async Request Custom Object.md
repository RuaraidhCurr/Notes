The Custom Object `Async_Request__c` is used to run tasks asynchronously and is handled mainly through the `TriggerOnAsyncRequest.trigger`. 

This trigger does the following:
1. Firstly checks to see if the trigger is running **BEFORE** async record insertion
2. 