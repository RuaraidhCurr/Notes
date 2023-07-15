Apex allows you to set class access modifiers to `private`, `protected`, `public` and `global`. 

while [[Apex Triggers|riggers]] and [[Anonymous Block|anonymous blocks]] can also use these access modifiers, they aren't useful in smaller portions of apex. e.g. declaring a global methods in an anonymous block doesn't enable you to call it from outside that code. 

By default, a method or variable is only visible in the apex file it's written in. We need to explicitly specify a method to be