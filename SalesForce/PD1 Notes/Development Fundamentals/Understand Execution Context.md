### What is Execution Context?
In the Lightning Platform world, code executes within an execution context. *IN A NUT SHELL* This context **represents the time between when the code is executed and when it ends**. 
*The apex code that you write is not always the only code that is executing*

### Methods of Invoking Apex
To understand how this works, you need to know all the ways Apex can be executed. 

| Method            | Description                                                  |
| ----------------- | ------------------------------------------------------------ |
| Database Trigger  | Invoked for a specific event on a custom or standard object. |
| Anonymous Apex    | Code snippets executed on the fly in Dev Console & other tools. |
| Asynchronous Apex |  Occurs when executing a future or queueable Apex, running a batch job, or scheduling Apex to run at a specified interval.                                |
| Web Services      |             Code that is exposed via SOAP or REST web services.                                                 | 
| Email Services | Code that is set up to process inbound email.| 
|           Visualforce or Lightning Pages                                                   |Visualforce controllers and Lightning components can execute Apex code automatically or when a user initiates an action, such as clicking a button. Lightning components can also be executed by Lightning processes and flows. | 

