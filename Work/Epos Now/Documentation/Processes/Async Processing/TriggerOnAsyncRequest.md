Triggered when an async request record is inserted or updated with "Retry" ticked. If the 
## `TriggerOnAsyncRequest.trigger`
Trigger that runs before and after insertion or update of an `Async_Request__c` record
1. Firstly checks to see if the trigger is running **BEFORE** async record insertion
2. It quireis the most recent record from the `Async_Request__c` object that has a non-null `Lastest_Attempt__c` field, less than 3 retry attempts, & not the type of "GenerateSubscriptions". It retrieves the processing delay, batch size and calculates the wait time
3. It determines if the load is high based on the delay, batch size, and wait time and stores `true/false` result in `highLoad` variable. 
4. If the trigger runs before the insertion of records, it queries paused requests with less than 20 retry attempts and where the `retry__c` field is not checked (set to "`False`")