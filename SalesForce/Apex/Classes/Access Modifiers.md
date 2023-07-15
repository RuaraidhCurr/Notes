Apex allows you to set class access modifiers to `private`, `protected`, `public` and `global`. 

while [[Apex Triggers|riggers]] and [[Anonymous Block|anonymous blocks]] can also use these access modifiers, they aren't useful in smaller portions of apex. e.g. declaring a global methods in an anonymous block doesn't enable you to call it from outside that code. 

By default, a method or variable is only visible in the apex file it's written in. We need to explicitly specify a method to be `public` for it to be available to other classes in the same application [[namespace]].

To use the private, protected, public, or global access modifiers, use the following syntax:
```apex
[(none)|private|protected|public|global] declaration
```

### `private` 

^5d2efd

this access modifier is the default, and means that the method or variable is accessible only within the apex class in which it's defined.

### `protected` 
This means that the method or variable is visible to any inner classes in the defining Apex class, and to the classes that extend the defining Apex class. You can only use this modifier for instance methods and member variables 

### `public`

^4b5b07

This means that the method or variable is accessible by all Apex within a specific package. For accessibility by all second-generation (2GP) managed packages that share a namespace, use `public` with the `&NamespaceAccessible` annotation. 

### `global`

^88dd84

This means the method or variable can be used by any Apex code that has access to the class, not just the apex code in the same application. This access modifier must be used for any method that must be referenced outside of the application, either in SOAP API or by other Apex code. If you declare a method or variable as `global`, you must also declare the class that contains it as `global`.