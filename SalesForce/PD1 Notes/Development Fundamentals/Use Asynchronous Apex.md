An asynchronous process is a process or function that executes a task "in the background" without the user having to wait for the task to finish.

## When to Go Asynchronous
The following are three of the main reason behind using asynchronous programming
1. Processing a very large number of records. 
2. Making callouts to external web services.
3. Creating a better and faster user experience

## Future Methods 
In situations where you need to make a callout top a web service or want to offload processing to an asynchronous task, creating a [[Future Methods|future method]] could be the way to go. 

Channing a method from synchronous to asynchronous processing is amazingly easy. Essentially, you just add the `@future` annotation to your static void method. e.g.
```apex
public class MyFutureClass {
    @future(callout=true)    
    static void myFutureMethod(Set<Id> ids) {
        List<Contact> contacts = [SELECT Id, LastName, FirstName, Email
            FROM Contact WHERE Id IN :ids];
        for (Contact con: contacts) {
            String response = anotherClass.calloutMethod(con.Id,
                con.FirstName,
                con.LastName,
                con.Email);
        }
    }
}
```

### [[Future Methods#^4bd109|Future Limitations]] 
- You can't track exection becuase n 