
---
tags: Apex, Governor Limits, Limits, Asynchronous Limits, Synchronous Limits, Apex limits, Email Limits, Email
---

Because Apex runs in a multitenant environment, the apex runtime engine strictly enforces limits so that runaway Apex code or processes don't monopolize shared resources. If code exceeds the limits the associated governor issues a runtime exception that can't be handled. 

## Per-Transaction Apex Limits

^a78715

| Description                                                                                                            | Synchronous Limit   | Asynchronous Limit                                      |
| ---------------------------------------------------------------------------------------------------------------------- | ------------------- | ------------------------------------------------------- |
| Total number of SOQL queries issued                                                                                   | 100                 | 200                                                     |
| Total number of records retrieved by SOQL queries                                                                      | 50,000              | 50,000                                                  |
| Total number of records retrieved by Database.getQueryLocator                                                          | 10,000              | 10,000                                                  |
| Total number of SOSL queries issued                                                                                    | 20                  | 20                                                      |
| Total number of records retrieved by a single SOSL query                                                               | 2,000               | 2,000                                                   |
| Total number of DML statements issued                                                                                 | 150                 | 150                                                     |
| Total number of records processed as a result of DML statements, Approval.process, or database.emptyRecycleBin         | 10,000              | 10,000                                                  |
| Total stack depth for any Apex invocation that recursively fires triggers due to insert, update, or delete statements | 16                  | 16                                                      |
| Total number of callouts (HTTP requests or web services calls) in a transaction                                        | 100                 | 100                                                     |
| Maximum cumulative timeout for all callouts (HTTP requests or Web services calls) in a transaction                     | 120 seconds         | 120 seconds                                             |
| Maximum number of methods with the future annotation allowed per Apex invocation                                       | 50                  | 0 in batch and future contexts; 50 in queueable context |
| Maximum number of Apex jobs added to the queue with System.enqueueJob                                                  | 50                  | 1                                                       |
| Total number of sendEmail methods allowed                                                                              | 10                  | 10                                                      |
| Total heap size                                                                                                       | 6 MB                | 12 MB                                                   |
| Maximum CPU time on the Salesforce servers                                                                            | 10,000 milliseconds | 60,000 milliseconds                                     |
| Maximum execution time for each Apex transaction                                                                       | 10 minutes          | 10 minutes                                              |
| Maximum number of push notification method calls allowed per Apex transaction                                          | 10                  | 10                                                      |
| Maximum number of push notifications that can be sent in each push notification method call                            | 2,000               | 2,000                                                   |
| Maximum number of EventBus.publish calls for platform events configured to publish immediately                         | 150                 | 150                                                     |

## Per-Transaction Certified Managed Package Limits

This table lists the cumulative cross-namespace limits.

|Description|Cumulative Cross-Namespace Limit|
|---|---|
|Total number of SOQL queries issued|1,100|
|Total number of records retrieved by Database.getQueryLocator|110,000|
|Total number of SOSL queries issued|220|
|Total number of DML statements issued|1,650|
|Total number of callouts (HTTP requests or web services calls) in a transaction|1,100|
|Total number of sendEmail methods allowed|110|

All per-transaction limits count separately for certified managed packages except for:

- The total heap size
- The maximum CPU time
- The maximum transaction execution time
- The maximum number of unique namespaces

## Lightning Platform Apex Limits

The limits in this table aren't specific to an Apex transaction; Lightning Platform enforces these limits.

| Description                                                                                                                                        | Limit                                                                                      |
| -------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------ |
| The maximum number of asynchronous Apex method executions (batch Apex, [[Furture Methods|future methods]], Queueable Apex, and [[Apex Scheduler|scheduled Apex]]) per a 24-hour period1,6 | 250,000 or the number of user licenses in your org multiplied by 200, whichever is greater |
| Number of synchronous concurrent transactions for long-running transactions that last longer than 5 seconds for each org.2                         | 10                                                                                         |
| Maximum number of Apex classes scheduled concurrently                                                                                              | 100. In Developer Edition orgs, the limit is 5.                                            |
| Maximum number of batch Apex jobs in the Apex flex queue that are in Holding status                                                                | 100                                                                                        |
| Maximum number of batch Apex jobs queued or active concurrently3                                                                                   | 5                                                                                          |
| Maximum number of batch Apex job start method concurrent executions4                                                                               | 1                                                                                          |
| Maximum number of batch jobs that can be submitted in a running test                                                                               | 5                                                                                          |
| Maximum number of test classes that can be queued per 24-hour period (production orgs other than Developer Edition)5,6                             | The greater of 500 or 10 multiplied by the number of test classes in the org               |
| Maximum number of test classes that can be queued per 24-hour period (sandbox and Developer Edition orgs)5,6                                       | The greater of 500 or 20 multiplied by the number of test classes in the org               |

## Static Apex Limits

^afd973

| Description                                                                        | Limit                                                    |
| ---------------------------------------------------------------------------------- | -------------------------------------------------------- |
| Default timeout of callouts (HTTP requests or Web services calls) in a transaction | 10 seconds                                               |
| Maximum size of callout request or response (HTTP request or Web services call)1   | 6 MB for synchronous Apex or 12 MB for asynchronous Apex |
| Maximum SOQL query run time before Salesforce cancels the transaction              | 120 seconds                                              |
| Maximum number of class and trigger code units in a deployment of Apex             | 7500                                                     |
| Apex trigger batch size2                                                           | 200                                                      |
| For loop list batch size                                                           | 200                                                      |
| Maximum number of records returned for a Batch Apex query in Database.QueryLocator | 50 million                                               |

## Size-Specific Apex Limits

|Description|Limit|
|---|---|
|Maximum number of characters for a class|1 million|
|Maximum number of characters for a trigger|1 million|
|Maximum amount of code used by all Apex code in an org1|6 MB|
|Method size limit2|65,535 bytecode instructions in compiled form|

## Miscellaneous Apex Limits

### Connect in Apex

For classes in the ConnectApi namespace, every write operation costs one DML statement against the Apex governor limit. ConnectApi method calls are also subject to rate limiting. ConnectApi rate limits match Connect REST API rate limits. Both have a per user, per namespace, per hour rate limit. When you exceed the rate limit, a ConnectApi.RateLimitException is thrown. Your Apex code must catch and handle this exception.

### Data.com Clean

If you use the Data.com Clean product and its automated jobs, consider how you use Apex triggers. If you have Apex triggers on account, contact, or lead records that run SOQL queries, the SOQL queries can interfere with Clean jobs for those objects. Your Apex triggers (combined) must not exceed 200 SOQL queries per batch. If they do, your Clean job for that object fails. In addition, if your triggers call future methods, they’re subject to a limit of 10 future calls per batch.

### Event Reports

The maximum number of records that an event report returns for a user who isn’t a system administrator is 20,000; for system administrators, 100,000.

### MAX_DML_ROWS limit in Apex testing

The maximum number of rows that can be inserted, updated, or deleted, in a single, synchronous Apex test execution context, is limited to 450,000. For example, an Apex class can have 45 methods that insert 10,000 rows each. If the limit is reached, you see this error: Your runallTests is consuming too many DB resources.

### SOQL Query Performance

For best performance, SOQL queries must be selective, particularly for queries inside triggers. To avoid long execution times, the system can terminate nonselective SOQL queries. Developers receive an error message when a non-selective query in a trigger executes against an object that contains more than 200,000 records. To avoid this error, ensure that the query is selective. See [More Efficient SOQL Queries](https://developer.salesforce.com/docs/atlas.en-us.244.0.apexcode.meta/apexcode/langCon_apex_SOQL_VLSQ.htm).

## Email Limits

Inbound Email Limits

|   |   |
|---|---|
|Email Services: Maximum Number of Email Messages Processed<br><br>(Includes limit for On-Demand Email-to-Case)|Number of user licenses multiplied by 1,000; maximum 1,000,000|
|Email Services: Maximum Size of Email Message (Body and Attachments)|25 MB1|
|On-Demand Email-to-Case: Maximum Email Attachment Size|25 MB|
|On-Demand Email-to-Case: Maximum Number of Email Messages Processed<br><br>(Counts toward limit for Email Services)|Number of user licenses multiplied by 1,000; maximum 1,000,000|

