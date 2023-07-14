---
tags: Apex, Asynchronous Apex, Queueable Apex, Invoking Apex, Running Apex, Scheduled Apex, Batch Apex, Future Methods, Batch, Future
---

Apex offers multiple ways for you to run your code asynchronously. Below lists the different wants you can run apex asynchronously and when you should use them.

| Asynchronous Apex Feature                                                                                                                                                                                                                                                                                                                                                                    | When to Use                                                                                                                                                                                                      |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [[Queueable Apex]] | - To start a long-running operation and get an ID for it<br>- To pass complex types to a job<br>- To chain jobs                                                                                                  |
| [[Scheduled Apex]] | - To schedule an Apex class to run on a specific schedule                                                                                                                                                        |
| [[Batch Apex]]  | - For long-running jobs with large data volumes that need to be performed in batches, such as database maintenance jobs<br>- For jobs that need larger query results than regular transactions allow             |
| [[Future Methods]] | - When you have a long-running method and need to prevent delaying an Apex transaction<br>- When you make callouts to external Web services<br>- To segregate DML operations and bypass the mixed save DML error |

