SOQL `for` loops iterate over all of the sObject records returned by a SOQL query.
The syntax of a SOQL for loop is either:
```apex
for (variable : [soql_query]) {
    code_block
}
```
or
```apex
for (variable_list : [soql_query]) {
    code_block
}
```
actual example:
```apex
String s = 'Acme';
for (Account a : [SELECT Id, Name from Account
                  where Name LIKE :(s+'%')]) {
    // Your code
}
```
The following example combines creating a list from a SOQL query, with the [[Adding and Retrieving Data with DML|DML]] update method.

```apex
// Create a list of account records from a SOQL query
List<Account> accs = [SELECT Id, Name FROM Account WHERE Name = 'Siebel']; 

// Loop through the list and update the Name field
for(Account a : accs){
   a.Name = 'Oracle';
}

// Update the database
update accs;
```