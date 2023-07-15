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

Local variable names are evaluated before class names. If a local variable has the same name as a class, the local variable hides methods and variables on the class of the same name. For example, this method works if you comment out the String line. But if the String line is included the method doesn’t compile, because Salesforce reports that the method doesn’t exist or has an incorrect signature.
```apex
public static void method() {
String Database = '';
Database.insert(new Account());
}
```
An inner class behaves like a static Java inner class, but doesn’t require the static keyword. An inner class can have instance member variables like an outer class, but there’s no implicit pointer to an instance of the outer class (using the this keyword).

## Instance Methods and Variables
Instanced methods and member variables are used by an instance of a class, that is, by and object. An instance member variable is declared inside a class, but not within a method. Instance methods usually use instance member variables to affect the behaviour of the method. 

Suppose that you want to have a class that collects two-dimensional points and plots them on a graph. The following skeleton class uses member variables to hold the list of points and an inner class to manage the two-dimensional list of points.

```apex
public class Plotter {

    // This inner class manages the points
    class Point {
        Double x;
        Double y;

        Point(Double x, Double y) {
             this.x = x;
             this.y = y;
        }
        Double getXCoordinate() {
             return x;
        }

        Double getYCoordinate() {
             return y;
        }
    }

    List<Point> points = new List<Point>();

    public void plot(Double x, Double y) {
        points.add(new Point(x, y));
    }
    
    // The following method takes the list of points and does something with them
    public void render() {
    }
}
```


## Initialization Code

Instance initialization code is a block of code in the following format that is defined by a class: 
```apex
{ 
   //code body
}
```

The instance initialization code in a class is executed each time an object is instantiated from that class. These code blocks run before the constructor.