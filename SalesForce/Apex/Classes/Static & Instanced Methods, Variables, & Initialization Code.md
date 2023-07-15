In Apex, you can have `static` methods, variables, and initialization code. However, Apex can't be static. You can also have instanced methods, member variables, and initialization code, which have no modifier, and local variables. 

### Characteristics 

Static methods, variables and initialization code have these characteristics 
- They're associated with a class
- They're allowed only in outer classes.
- They're initialized only when a class is loaded.
- They aren't transmitted as part of the view state for Visualforce pages

Instance methods, member variables, and initialization code have these characteristics 
- They're associated with a particular object.
- They have no definition modifier.
- They're created with every object instantiated from the class in which they're declared. 

Local variables have these characteristics
- They're associated with the block of code in which they're declared
- They must be initialized before they're used

The following example shows a local variable whose scope is the duration of the if code block.
```apex
Boolean myCondition = true;
if (myCondition) {
    integer localVariable = 10;
}
```

## Static Methods and Variables 
A static method is used as a utility method, and it never depends on the value of an instance member variable. Because a static method is only associated with a class, it can't access the instance member variable value of its class.  

A static variable is static only within the scope of the Apex transaction. It's not static across the server or the organization.

To store information that is shared across instances of a class, use a static variable. All instances of the same class share a single copy of the static variable. For example, all triggers that a single transaction spawns can communicate with each other by viewing and updating static variables in a related class.

Suppose that you had the following class.

```apex
public class P { 
   public static boolean firstRun = true; 
}
```
A trigger that uses this class could then selectively fail the first run of the trigger.
```apex
trigger T1 on Account (before delete, after delete, after undelete) { 
       if(Trigger.isBefore){
          if(Trigger.isDelete){
             if(p.firstRun){
                 Trigger.old[0].addError('Before Account Delete Error');
                  p.firstRun=false;
              } 
           }
        }
}
```
