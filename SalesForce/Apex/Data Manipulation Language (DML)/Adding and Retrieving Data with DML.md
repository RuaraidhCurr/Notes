---
tags: Apex, Data retrieval, Data manipulation, Data insert, Data Manipulation Language, DML
---

Records in the database can be inserted and manipulated through apex directly using simple statements. These Statements are the Data Manipulation Language (DML). In contrast to the SOQL which is used for read operations, DML is used for write operations. 
when inserting or manipulating records, the data bust first be created in memory as an [[SObjects & Objects|sObject]] such as `Account`, `Contact`, `MyCustomObject__c` but when you don't know the type object in advance you can work with generic [[SObjects & Objects|sObject]] data type instead.

You use DML statements to persist sObjects to the database, here is an example:
```
Account a = new Account(Name='Account Example');
insert a;
```
you can use DML to modify records that have already been inserted or already exist on the database 
``` apex
// Query existing account.
Account a = [SELECT Name,Industry 
               FROM Account 
               WHERE Name='Account Example' LIMIT 1];

// Write the old values the debug log before updating them.
System.debug('Account Name before update: ' + a.Name); // Name is Account Example
System.debug('Account Industry before update: ' + a.Industry);// Industry is not set

// Modify the two fields on the sObject.
a.Name = 'Account of the Day';
a.Industry = 'Technology';

// Persist the changes.
update a;

// Get a new copy of the account from the database with the two fields.
Account a = [SELECT Name,Industry 
             FROM Account 
             WHERE Name='Account of the Day' LIMIT 1];

// Verify that updated field values were persisted.
System.assertEquals('Account of the Day', a.Name);
System.assertEquals('Technology', a.Industry);
```
