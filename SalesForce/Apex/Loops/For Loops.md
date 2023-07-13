The `for` loop supports three variations:

##### Traditional `for` loop
```
for (init_stmt; exit_condition; increment_stmt) {
    code_block
}
```
##### List or set iteration `for` loop
```
for (variable : list_or_set) {
    code_block
}
```
##### SOQL `for` loop
```
for (variable : [soql_query]) {
    code_block
}
or
for (variable_list : [soql_query]) {
    code_block
}
```
Both `variable` and `variable_list` must be of the same `sObject` type as is returned by the `soql_query`.
