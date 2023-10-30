Triggered when an async request record is inserted or updated with "Retry" ticked. If the request is below 20 retires then a [[TriggerOnAsyncAttempt|Async_Attempt__e]] platform event is submitted placing it in the platform event queue. ([[TriggerOnAsyncAttempt||TriggerOnAsyncAttempt]] is then fired from the platform event which runs the related action detailed in ProcessAsyncRequest) 
## `TriggerOnAsyncRequest.trigger`
Trigger that runs before and after insertion or update of an `Async_Request__c` record
1. Firstly checks to see if the trigger is running **BEFORE** async record insertion
2. It quireis the most recent record from the `Async_Request__c` object that has a non-null `Lastest_Attempt__c` field, less than 3 retry attempts, & not the type of "GenerateSubscriptions". It retrieves the processing delay, batch size and calculates the wait time
3. It determines if the load is high based on the delay, batch size, and wait time and stores `true/false` result in `highLoad` variable. 
4. If the trigger runs before the insertion of records, it queries paused requests with less than 20 retry attempts and where the `retry__c` field is not checked (set to "`False`"). it counts the number of paused requests
5. if the load (`hightLoad`) is not high (`true`), it resumes some of the paused requests by updating their `Paused__c` field and setting `Resumed_Datetime` to NOW()
6. For each record being inserted or updated, it checks if the `System.Label.ProcessAsyncRequests` is set to "PlatformEvents" or if the code is running in a test context and if the trigger is running before insertion of records (`beforeInsert = true`)
	1. If the load is high and the request's priority is not 'P1' and the `System.Label.PauseAsyncs` is set to `true`, it marks the request as paused and sets the `Paused_Count__c` field to the count of paused requests.
7. If the request is marked to use platform events and the trigger is running after insertion or update of records:
	1. If the trigger is running after an update and the `Paused__c` field has changed from true to false, and the `Rety_attempts__c` field is null or less than or equal to 1. It sets the `Retry__c` field to true