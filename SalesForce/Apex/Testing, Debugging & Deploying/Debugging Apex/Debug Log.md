A debug log can record database operations, system processes, and errors that occur when executing a transaction or running unit tests. Debug logs can contain information about: 
- Database changes
- HTTP callouts
- Apex errors
- Resources used by Apex
- Automated workflow processes, such as:
	- Workflow rules
	- Assignment rules
	- Approval processes
	- Validation rules 
To view debug logs, go to setup and enter `Debug logs` in the find box and then select **Debug Logs**

## Debug Logs have limits 
- Each debug log must be 20 MB or smaller
- System debugs are retained for 24hrs
- If you generate more than 1,000 MB of debugs in a 15min window, your trace flags are disabled.
- When your org accumulates more than 1,000 MB of debug logs, Salesforce prevents users in the or from adding or editing trace flags. 

You can also specify one of the following logging levels for the debug logs

|                |       |
| -------------- | ----- |
| Apex Code      | DEBUG |
| Apex Profiling | INFO  |
| Callout        | INFO  |
| Database       | INFO  |
| System         | DEBUG |
| Validation     | INFO  |
| Visualforce    | INFO  |
| Workflow       | INFO  |
| ---            | ---   |
|                |       |

##### Log Lines
Log lines are included inside units of code and indicate which code or rules are being executed. Log lines can also be messages written to the debug log. For example:

![Debug Log Line Example](https://developer.salesforce.com/docs/resources/img/en-us/244.0?doc_id=help%2Fimages%2Fdebug_log_line_user_debug.png&folder=apexcode)

The log lines are a set of fields that are split by a pipe `|`. The format is:
- *timestamp*: consists of the time when an event occurred
- *event identifier*: Specifies the event that triggered the debug log entry
	- *information logged*: such as method name, line and character number where the code was executed.
