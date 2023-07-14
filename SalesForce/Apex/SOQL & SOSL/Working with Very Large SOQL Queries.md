---
tags: Apex, SOQL, Queries, SOQL Queries, SOQL Statements, Large Queries, Governor Limits 
---

SOQL queries are limited by heap size, governor limits and if exceeded and error occurs. Firstly for best performance, SOQL queries must be selective, particularly for queries inside triggers. To avoid long execution times, and [[Governor Limits]], the system can terminate nonselective SOQL queries. Developers receive an error message when a non-selective query in a trigger executes against an object that contains more than 1 million records. To avoid this error, ensure that the query is selective. selective queries are queries with filters 

An example of a query that is likely to get too big:
```apex
Account[] accts = [SELECT Id FROM Account];
```
Instead, use a SOQL query for loop as in one of the following examples:
```apex
// Use this format if you are not executing DML statements 
// within the for loop
for (Account a : [SELECT Id, Name FROM Account 
                  WHERE Name LIKE 'Acme%']) {
    // Your code without DML statements here
}

// Use this format for efficiency if you are executing DML statements 
// within the for loop
for (List<Account> accts : [SELECT Id, Name FROM Account
                            WHERE Name LIKE 'Acme%']) {
    for (Account a : accts) {
    // Your code here
    }
    update accts;
}
```


