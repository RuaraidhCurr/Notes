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
```apex
Map<String, String> country_currencies = new Map<String, String>(); Map<ID, Set<String>> m = new Map<ID, Set<String>>();
```
To access elements in a map, use the map method in Apex. e.g.
```apex
Map<Integer, String> m = new Map<Integer, String>(); // Define a new map 
m.put(1, 'First entry'); // Insert a new key-value pair in the map 
m.put(2, 'Second entry'); // Insert a new key-value pair in the map System.assert(m.containsKey(1)); // Assert that the map contains a key 
String value = m.get(2); // Retrieve a value, given a particular key 
System.assertEquals('Second entry', value); 
Set<Integer> s = m.keySet(); // Return a set that contains all of the keys in the map
```
Could have a map that contains ID values for the key and then mapped to an [[SObjects & Objects|sObject]]) e.g.
```apex
Map<ID, Account> m = new Map<ID, Account>();
// or
Map<Id, Account> accountMap = new Map<Id, Account>([SELECT Id, Name FROM Account]);
```
You can populate maps key-value pairs when the map is declared by using curly brace `{}` syntax. the curly braces, specify the key firstm then specify the value for that key using =>. This example creates a map of integers to accounts lists and adds one entry using the account list created earlier.

```apex
Account[] accs = new Account[5]; // Account[] is synonymous with List<Account>
Map<Integer, List<Account>> m4 = new Map<Integer, List<Account>>{1 => accs};
```

When working with [[SOQL & SOSL Queries#^869117]] queries, maps can be populated from the results returned by the SOQL query. The map key must be declared with an ID String data type, and the map value must be declared as an sObject data type.
``` apex
// Populate map from SOQL query
Map<ID, Account> m = new Map<ID, Account>([SELECT Id, Name FROM Account LIMIT 10]);
// After populating the map, iterate through the map entries
for (ID idKey : m.keyset()) {
    Account a = m.get(idKey);
    System.debug(a);
}
```