Debugging on a multi tenant cloud environment presents unique challenges as it is not as easy as say debugging on VS Code. There are tools used in the salesforce platform that help with debugging and running diagnostics

## [[Debug Log]]
The debug Log is where you can find most of what you need to debug your code. 
To view something in the debug log simply type the following into you code and when it's run it will display in the Log:
``` apex
System.debug('My Debug Message');
```
You can also specify one of the following logging levels for the debug logs
- NONE
- ERROR
- WARN
- INFO
- DEBUG
- FINE
- FINER
- FINEST
These levels run from lowest to highest and are cumulative. 

