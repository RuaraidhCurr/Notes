Batch Apex is used to run, long-running processes that run thousands of records on the Lightning Platform. Batch apex runs small batches of records breaking down an entire dataset into manageable junks as to not hit [[Governor Limits]] or any other Org specific limits.

Batch apex jobs are invoked at run time using Apex. 

You can only have 5 batch jobs queued at one time. You can evaluate your current count by viewing the Scheduled Jobs page in Salesforce.

Batch jobs can also be programmatically scheduled to run at specific times using [[Apex Scheduler]]. 

# Using Batch Apex
To use Batch Apex write a Class that implements `data`




## Using the System.scheduleBatch Method

^ffc109
