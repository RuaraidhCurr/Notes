To define a method, specify the following:
- Optional: Modifiers, such as public or protected.
- Required: The data type of the value returned by the method, such as String or Integer. Use void if the method doesn’t return a value.
- Required: A list of input parameters for the method, separated by commas, each preceded by its data type, and enclosed in parentheses (). If there are no parameters, use a set of empty parentheses. A method can only have 32 input parameters.
- Required: The body of the method, enclosed in braces {}. All the code for the method, including any local variable declarations, is contained here.

Use the following syntax when defining a method:

```apex
[public | private | protected | global] [override] [static] data_type method_name 
(input parameters) 
{
// The body of the method
}
```
You can use override to override methods only in classes that have been defined as virtual or abstract.

For example:
```apex
public static Integer getInt() { 
     return MY_INT; 
```

## Passing Method Arguments by Value 
In Apex, all [[Primitive Data types|primitive data]] types arguments, such as Integer or String, are passed into methods by value. This mean that changes to the arguments exist only within the scope of the method. When the method returns the arguments are lost.
Non-[[Primitive Data types|primitive data]] type arguments, such as [[SObjects & Objects|sObjects]], are passed into methods by reference. Therefore, when the method returns, the passed-in argument still references the same object as before the method call. Within the method, the reference can't be changed to another object but the value of the object's field can be changed.

**Example: Passing Primitive Data Type Arguments**
```apex
public class PassPrimitiveTypeExample {
    public static void debugStatusMessage() {
        String msg = 'Original value';
        processString(msg);
        // The value of the msg variable didn't
        // change; it is still the old value.
        System.assertEquals(msg, 'Original value');
    }
    
    public static void processString(String s) {
        s = 'Modified value';
    }
}
```

**Example: Passing Non-Primitive Data Type Arguments**
```apex
public class PassNonPrimitiveTypeExample {
    
    public static void createTemperatureHistory() {
        List<Integer> fillMe = new List<Integer>();        
        reference(fillMe);
        // The list is modified and contains five items
        // as expected.
        System.assertEquals(fillMe.size(),5);        
        
        List<Integer> createMe = new List<Integer>();
        referenceNew(createMe);
        // The list is not modified because it still points
        // to the original list, not the new list 
        // that the method created.
        System.assertEquals(createMe.size(),0);     
    }
            
    public static void reference(List<Integer> m) {
        // Add rounded temperatures for the last five days.
        m.add(70);
        m.add(68);
        m.add(75);
        m.add(80);
        m.add(82);
    }    
        
    public static void referenceNew(List<Integer> m) {
        // Assign argument to a new List of
        // five temperature values.
        m = new List<Integer>{55, 59, 62, 60, 63};
    }    
}
```