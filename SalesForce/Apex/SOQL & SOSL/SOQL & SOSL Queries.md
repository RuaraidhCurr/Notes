You can run Salesforce Object Query Language (SOQL) or Salesforce Object Search Language (SOSL) statements in Apex by surrounding the statement in square brackets. 

## SOQL Statements

SOQL statements returns a list of [[sObjects]](*records*), a single [[SObjects|sObject]](*record*), or and Integer for [[SOQL & SOS]] method queries.  
Example below:
```
List<Account> aa = [SELECT Id, Name FROM Account WHERE Name = 'Acme'];

// From this list, you can access individual elements:

if (!aa.isEmpty()) {
   // Execute commands
}
```
You can also create new objects from SOQL queries on existing ones:
```
Contact c = new Contact(Account = [SELECT Name FROM Account 
    WHERE NumberOfEmployees > 10 LIMIT 1]);
c.FirstName = 'James';
c.LastName = 'Yoyce';
```

The count method can be used to return the number of rows returned by a query:
```
Integer i = [SELECT COUNT() FROM Contact WHERE LastName = 'Weissman'];

// Expand to

Integer j = 5 * [SELECT COUNT() FROM Account];
```