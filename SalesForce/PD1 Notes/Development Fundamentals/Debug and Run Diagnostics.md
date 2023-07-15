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

## Log Inspector
The Log Inspector is the section at the bottom of the developer console. (good for viewing test coverage). You can also view `System.debug`'s in there as well by following the next steps. 
1. From Setup, select **Your Name > Developer Console** to open Developer Console.
2. Select **Debug > Change Log Levels**.
3. Click the **Add/Change** link in General Trace Setting for You.
4. Select **INFO** as the debug level for all columns.
5. Click **Done**.
6. Click **Done**.
7. Select **Debug > Perspective Manager**.
8. Select **All (Predefined)** and click **Set Default**.
9. Click **Yes** to change this to your default perspective.
10. *Run some code to debug*
14. Select **Debug > Switch Perspective > All (Predefined)**.
15. Examine the results in the Timeline and Executed Units tabs.
16. Under Execution Log, select the **Filter** option, and enter **FINE**. Because we set the debug level to INFO for Apex Code, no results appear.
17. Select **Debug > Change Log Levels**.
18. Click the **Add/Change** link in General Trace Setting for You.
19. Change the DebugLevel for ApexCode and Profiling to **FINEST**.
20. Click **Done**.
21. Click **Done**.

## Checkpoints
Checkpoints are similar to breakpoints in that they reveal detailed execution information about a line of code. They **DON'T** stop the execution of that line. 

