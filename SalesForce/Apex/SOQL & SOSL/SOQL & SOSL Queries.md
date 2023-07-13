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

### Aggregate Functions
Aggregate functions such as `SUM()`, `MAX()` and `AVG()` allow you to roll up and summarize data in a query. 

For example, you could use the AVG() aggregate function to find the average Amount for all your opportunities:
```
AggregateResult[] groupedResults = [SELECT 
AVG(Amount) 
FROM Opportunity];
Object avgAmount = groupedResults[0].get('aver');
```

Aggregate functions become a more powerful tool to generate reports when you use them with a GROUP BY clause e.g.
```
AggregateResult[] groupedResults
  = [SELECT CampaignId, AVG(Amount)
      FROM Opportunity
      GROUP BY CampaignId];
for (AggregateResult ar : groupedResults)  {
    System.debug('Campaign ID' + ar.get('CampaignId'));
    System.debug('Average amount' + ar.get('expr0'));
}
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

## Additional Information

### Some pointers
- If you try to access a field that was not selected in the SOQL or SOSL query (other than ID), you receive a runtime error. 
- If only one sObject field is selected, a SOQL or SOSL query always returns data as complete records. Consequently, you must dereference the field in order to access it.

### Accessing [[SObjects|sObject]] fields through Relationships
You can access connected an sObject's related sObjects through SOQL & SOSL statements using the related sObjects type field on the sObject. For example, the Contact sObject has both an `AccountId` field of type `ID`, and an `Account` field of type `Account` that points to the associated sObject record itself.

For example, the following Apex code shows how an account and a contact can be associated with one another, and then how the contact can be used to modify a field on the account:
```
Account a = new Account(Name = 'Acme');
insert a;  // Inserting the record automatically assigns a 
           // value to its ID field
Contact c = new Contact(LastName = 'Weissman');
c.AccountId = a.Id;
// The new contact now points at the new account
insert c;

// A SOQL query accesses data for the inserted contact, 
// including a populated c.account field
c = [SELECT Account.Name FROM Contact WHERE Id = :c.Id];

// Now fields in both records can be changed through the contact
c.Account.Name = 'salesforce.com';
c.LastName = 'Roth';

// To update the database, the two types of records must be 
// updated separately
update c;         // This only changes the contact's last name
update c.Account; // This updates the account name
```
In the next example we will use query the related sObjects in SOQL:
```
Account a : [SELECT Id, Name, Contact, Contact.LastName, Contact.FirstName FROM Account WHERE Name = 'Acme];
contact c = a.Contact;
```
