A list is a collection of any data types e.g. *primitive types*, *collections*, *[[sObjects]]*, etc...

This table is a visual representation of a list of Strings:

|Index 0|Index 1|Index 2|Index 3|Index 4|Index 5|
|---|---|---|---|---|---|
|'Red'|'Orange'|'Yellow'|'Green'|'Blue'|'Purple'|

To create a list use the `List` keyword followed by the data type within `<>` e.g.
```
// Create an empty list of String 
List<String> my_list = new List<String>(); 
// Create a nested list 
List<List<Set<Integer>>> my_list_2 = new List<List<Set<Integer>>>();
```

## Adding and Retrieving List Elements

As with lists of primitive data types, you can access and set elements of sObject lists using the List methods provided by Apex. For example:

```apex
List<Account> myList = new List<Account>(); // Define a new list
Account a = new Account(Name='Acme'); // Create the account first
myList.add(a);                    // Add the account sObject
Account a2 = myList.get(0);      // Retrieve the element at index 0
```