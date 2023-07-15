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

## Passing Mthod At