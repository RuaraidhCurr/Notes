A Future Method runs in the background [[Asynchronous Apex|asynchronously]]. Call future methods for executing long-running operations, such as callouts to external web services or operations you'd like to run in it's own thread, on it's own time.

Future Methods are queued and executed when system resources become available. A benefit of using Future Methods is that some [[Governor Limits]] are higher, such as [[SOQL & SOSL Queries|SOQL query]] limits and heap size limits.

To define a future method, simply annotate it with the future annotation, as follows.
```apex
global class FutureClass
{
    @future
    public static void myFutureMethod()
    {   
         // Perform some operations
    }
}
```
Methods with the future annotation must be static methods, and can only return a void type and method parameters must be [[Primitive Data types]], [[Sets|sets/arrays]] of [[Primitive Data types]] or collections of [[Primitive Data types]]. The reason why [[SObjects & Objects|sObjects]] can’t be passed as arguments to future methods is because the [[SObjects & Objects|sObjects]] can change between the time you call the method and the time it executes.

The following example shows how to do so with a [[Lists|list]] of IDs.
```apex
global class FutureMethodRecordProcessing
{
    @future
    public static void processRecords(List<ID> recordIds)
    {   
         // Get those records based on the IDs
         List<Account> accts = [SELECT Name FROM Account WHERE Id IN :recordIds];
         // Process records
    }
}
```

The following is a skeletal example of a future method that makes a callout to an external service. Notice that the annotation takes an extra parameter `(callout=true)` to indicate that callouts are allowed.
```apex
global class FutureMethodExample
{
    @future(callout=true)
    public static void getStockQuotes(String acctName)
    {   
         // Perform a callout to an external service
    }

}
```

### Future Method Limits
- No more than 