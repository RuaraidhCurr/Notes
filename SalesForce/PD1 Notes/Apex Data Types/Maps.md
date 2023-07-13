A map is a collection of key value pairs. the key value pairs can be any combination primitive data type. Key value must be unique.

Example 

| Country (Key) | Currency (Value) | 
|--| --| 
| United States| Dollar | 
| Japan | yen |
| France | Euro |
| England | Pound |
| India | Roupee | 

To declare a Map use `Map` followed by the two data types within `<>` e.g.
```
Map<String, String> country_currencies = new Map<String, String>(); Map<ID, Set<String>> m = new Map<ID, Set<String>>();
```
To access elements in a map, use the map method in Apex. e.g.
```
Map<Integer, String> m = new Map<Integer, String>(); // Define a new map 
m.put(1, 'First entry'); // Insert a new key-value pair in the map 
m.put(2, 'Second entry'); // Insert a new key-value pair in the map System.assert(m.containsKey(1)); // Assert that the map contains a key 
String value = m.get(2); // Retrieve a value, given a particular key 
System.assertEquals('Second entry', value); 
Set<Integer> s = m.keySet(); // Return a set that contains all of the keys in the map
```
Could have a map that contains ID values for the key and then mapped to an sObject) e.g.
```
Map<Id, Account> accountMap = new Map<Id, Account>([SELECT Id, Name FROM Account]);
```
