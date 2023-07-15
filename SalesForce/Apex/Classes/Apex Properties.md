An Apex property is similar to a variable, however, you can do additional things in your code to a property value before it's accessed or returned. Properties can be used to validate data before a change is made, to prompt an action when data is changed, or to reveal data that is gathered from another source. 

property definitions include one or two code blocks, representing a `get` accessor and a `set` accessor:
- The code in the `get` accessor executes when the property is read
- The code in the `set` accessor executes when the property is assigned a new value 

If a property has only a get accessor, it’s considered read-only. If a property has only a set accessor, it’s considered write-only. A property with both accessors is considered read-write.

To declare a property, use the following syntax in the body of a class:

```apex
Public class BasicClass {

   // Property declaration
   access_modifier return_type property_name {
      get {
         //Get accessor code block
      }
      set {
         //Set accessor code block
      }
   } 
}
```
- access_modifier is the [[Access Modifiers|access modifier]] for the property. The access modifiers that can be applied to properties include: public, private, global, and protected. In addition, these definition modifiers can be applied: static and transient.
- return_type is the type of the property, such as Integer, Double, [[SObjects & Objects|sObject]], and so on.
- property_name is the name of the property

For example, the following class defines a property named prop. The property is public. The property returns an integer data type.

```apex
public class BasicProperty {
   public integer prop {
      get { return prop; }
      set { prop = value; }
   }
}
```

The following code segment calls the `BasicProperty` class, exercising the get and set accessors:

```apex
BasicProperty bp = new BasicProperty();
bp.prop = 5;                   // Calls set accessor
```


## Using Automatic Properties
Properties don't require additional code in their `get` or `set` accessor code blocks. Instead, you can leave `get` and `set` accessor code blocks empty to define an *automatic property*. Automatic properties allow you to write more compact code that is easier to debug and maintain. They can be declared as read-only, read-write, or write-only. The following example creates three automatic properties:
```apex
public class AutomaticProperty {
   public integer MyReadOnlyProp { get; }
   public double MyReadWriteProp { get; set; }
   public string MyWriteOnlyProp { set; }
}
```

