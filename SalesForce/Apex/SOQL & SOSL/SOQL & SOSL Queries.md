You can run Salesforce Object Query Language (SOQL) or Salesforce Object Search Language (SOSL) statements in Apex by surrounding the statement in square brackets. 

## SOQL Statements


SOQL statements returns a list of [[sObjects]](*records*), a single [[SObjects|sObject]](*record*), or and Integer for [[SOQL & SOSL Queries#^1cbe1d|count]] method queries.  
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

The count method can be used to return the number of rows returned by a query: ^1cbe1d
```
Integer i = [SELECT COUNT() FROM Contact WHERE LastName = 'Weissman'];

// Expand to

Integer j = 5 * [SELECT COUNT() FROM Account];
```

## SOSL Statements

SOSL statements evaluate to a list of sObjects, where each list contains the search results for a particular sObject type. 

For example, you can return a list of accounts, contacts, opportunities, and leads that begin with the phrase map:
```
List<List<SObject>> searchList = [FIND 'map*' IN ALL FIELDS RETURNING Account (Id, Name), Contact, Opportunity, Lead];
```
From `searchList`, you can create arrays for each object returned:
```
Account [] accounts = ((List<Account>)searchList[0]);
Contact [] contacts = ((List<Contact>)searchList[1]);
Opportunity [] opportunities = ((List<Opportunity>)searchList[2]);
Lead [] leads = ((List<Lead>)searchList[3]);
```