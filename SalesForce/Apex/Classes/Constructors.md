A constructor is code that invoked when an object is created from the class blueprint. Not every class needs a constructor. If there are no user defined constructor than the `public` constructor is used.

The syntax for constructors are similar to a  method, but it differs from a method definition in that it never has an explicit return type.

After you write a constructor for a class you must write the `new` keyword in order to instantiate and object from that class.
```apex
public class TestObject {

   // The no argument constructor 
   public TestObject() {
      // more code here
  }
}
```

A new object of this type can be instantiated with the following code:
```apex
TestObject myTest = new TestObject();
```

if you write a constructor that takes arguments, you can then use that constructor to create an object using those arguments.

In Apex, a constructor can be overloaded, that is, there can be more than one constructor for a class, each having different parameters. The following example illustrates a class with two constructors: one with no arguments and one that takes a simple Integer argument. It also illustrates how one constructor calls another constructor using the this(...) syntax, also know as constructor chaining.

```apex
public class TestObject2 {

private static final Integer DEFAULT_SIZE = 10;

Integer size;

   //Constructor with no arguments
   public TestObject2() {
       this(DEFAULT_SIZE); // Using this(...) calls the one argument constructor    
   }

   // Constructor with one argument 
   public TestObject2(Integer ObjectSize) {
     size = ObjectSize;  
   }
}
```

New objects of this type can be instantiated with the following code:

```apex
TestObject2 myObject1 = new TestObject2(42);
  TestObject2 myObject2 = new TestObject2();
```

Every constructor that you create for a class must have a different argument list. In the following example, all of the constructors are possible:

```apex
public class Leads {

  // First a no-argument constructor 
  public Leads () {}

  // A constructor with one argument
  public Leads (Boolean call) {}

  // A constructor with two arguments
  public Leads (String email, Boolean call) {}

  // Though this constructor has the same arguments as the 
  // one above, they are in a different order, so this is legal
  public Leads (Boolean call, String email) {}
}
```