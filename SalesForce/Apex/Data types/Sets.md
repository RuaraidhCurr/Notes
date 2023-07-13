Sets are an unordered collection of primitive data types that does not contain any duplicates e.g. 

| |   |   |     |
|---|---|---|---|
|'San Francisco'|'New York'|'Paris'|'Tokyo'|
| | | | |
Sets are commonly used to store ID values becuase the value is always unique.

Sets can contain a collections nested within one another, e.g. *'A set of lists of of sets of Integers'*

Use the `Set` keyword followed by the primitive data type within `<>` e.g.
```
Set<String> myStringSet = new Set<String>();
// Defines a new set with two elements
Set<String> set1 = new Set<String>{'New York', 'Paris'};
```

Accessing sets may look similar to the following:
```
// Define a new set Set<Integer> 
mySet = new Set<Integer>(); 
// Add two elements to the set 
mySet.add(1); 
mySet.add(3); 
// Assert that the set contains the integer value we added 
System.assert(mySet.contains(1)); 
// Remove the integer value from the set 
mySet.remove(1);
```
