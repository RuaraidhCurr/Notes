As in java, you can create classes in apex. A class is a template or blueprint from which objects are created. An `Object` is an instance of a class. 

A Class can contain variables and methods. Variables are used to specify the state of an object, such as Object's `Name` or `Type`. Since these variables are associated with a class and are members of it, they are commonly referred to as "Member Variables". 

A Class can contain other classes, exception types, and initialization code. 

In Addition to classes, Apex provides [[Apex Triggers|Triggers]], a trigger is Apex code that executes before or after database operations. 

### Example
The `PurchaseOrder` class describes an entire purchase order, and everything that you can do with a purchase order. An instance of the `PurchaseOrder` class is a specific purchase order that you send or receive. 
The state of a `PurchaseOrder` object-*what it knows*-includes the user who sent it, the date and time it was created, and whether it was flagged as important. 

## Apex Class Definition
You can define top-level classes (also called "outer classes") as well as inner classes, that is, a class defined within another class. You can only have inner classes one level deep. 
```apex
public class myOuterClass {
   // Additional myOuterClass code here
   class myInnerClass {
     // myInnerClass code here
   }
}
```

Specify the following to define a class: 
1. Access modifiers 
	1. such as `public` `global`
	2. you don't need access modifiers for inner classes 
2. *Optional *