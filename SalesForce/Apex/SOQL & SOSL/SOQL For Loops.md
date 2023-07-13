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