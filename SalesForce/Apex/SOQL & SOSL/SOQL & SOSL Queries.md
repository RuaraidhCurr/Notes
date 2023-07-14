---
tags: Apex, SOQL, For Loops, For-Loops
---

You can run Salesforce Object Query Language (SOQL) or Salesforce Object Search Language (SOSL) statements in Apex by surrounding the statement in square brackets. 

## SOQL Statements

^869117


SOQL statements returns a list of [[SObjects & Objects]](*records*), a single [[SObjects & Objects|sObject]](*record*), or and Integer for [[SOQL & SOSL Queries#^1cbe1d|count]] method queries.  
Example below:
```apex
List<Account> aa = [SELECT Id, Name FROM Account WHERE Name = 'Acme'];

// From this list, you can access individual elements:

if (!aa.isEmpty()) {
   // Execute commands
}
```
You can also create new objects from SOQL queries on existing ones:
```apex
Contact c = new Contact(Account = [SELECT Name FROM Account 
    WHERE NumberOfEmployees > 10 LIMIT 1]);
c.FirstName = 'James';
c.LastName = 'Yoyce';
```

The count method can be used to return the number of rows returned by a query: ^1cbe1d
```apex
Integer i = [SELECT COUNT() FROM Contact WHERE LastName = 'Weissman'];

// Expand to

Integer j = 5 * [SELECT COUNT() FROM Account];
```

### Aggregate Functions
Aggregate functions such as `SUM()`, `MAX()` and `AVG()` allow you to roll up and summarize data in a query. 

For example, you could use the AVG() aggregate function to find the average Amount for all your opportunities:
```apex
AggregateResult[] groupedResults = [SELECT 
AVG(Amount) 
FROM Opportunity];
Object avgAmount = groupedResults[0].get('aver');
```

Aggregate functions become a more powerful tool to generate reports when you use them with a GROUP BY clause e.g.
```apex
AggregateResult[] groupedResults
  = [SELECT CampaignId, AVG(Amount)
      FROM Opportunity
      GROUP BY CampaignId];
for (AggregateResult ar : groupedResults)  {
    System.debug('Campaign ID' + ar.get('CampaignId'));
    System.debug('Average amount' + ar.get('expr0'));
}
```
### Polymorphic Relationships
A polymorphic relationship is a relationship between objects where a referenced object can be one of several different types. For example, the `Who` relationship field of a `Task` can be a `Contact` or a `Lead`.

This example queries Events that are related to an Account or Opportunity via the What field.
```apex
List<Event> events = [SELECT Description FROM Event WHERE What.Type IN ('Account', 'Opportunity')];
```
If you need to access the referenced object in a polymorphic relationship, you can use the [instanceof](https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_classes_keywords_instanceof.htm) keyword to determine the object type.
```apex
Event myEvent = eventFromQuery;
if (myEvent.What instanceof Account) {
    // myEvent.What references an Account, so process accordingly
} else if (myEvent.What instanceof Opportunity) {
    // myEvent.What references an Opportunity, so process accordingly
}
```
## SOSL Statements

^1c251f

SOSL statements evaluate to a list of sObjects, where each list contains the search results for a particular sObject type. 

For example, you can return a list of accounts, contacts, opportunities, and leads that begin with the phrase map:
```apex
List<List<SObject>> searchList = [FIND 'map*' IN ALL FIELDS RETURNING Account (Id, Name), Contact, Opportunity, Lead];
```
From `searchList`, you can create arrays for each object returned:
```apex
Account [] accounts = ((List<Account>)searchList[0]);
Contact [] contacts = ((List<Contact>)searchList[1]);
Opportunity [] opportunities = ((List<Opportunity>)searchList[2]);
Lead [] leads = ((List<Lead>)searchList[3]);
```

## Additional Information

### Using Apex Variables in SOQL and SOSL Queries
SOQL and SOSL statements in Apex can reference Apex code variables and expressions if they’re preceded by a colon (:)
- The search string in FIND clauses.
- The filter literals in WHERE clauses.
- The value of the IN or NOT IN operator in WHERE clauses, allowing filtering on a dynamic set of values. Note that this is of particular use with a list of IDs or Strings, though it works with lists of any type.
- The division names in WITH DIVISION clauses.
- The numeric value in LIMIT clauses.
- The numeric value in OFFSET clauses.

### Accessing [[SObjects & Objects|sObject]] fields through Relationships
You can access connected an sObject's related sObjects through SOQL & SOSL statements using the related sObjects type field on the sObject. For example, the Contact sObject has both an `AccountId` field of type `ID`, and an `Account` field of type `Account` that points to the associated sObject record itself.

For example, the following Apex code shows how an account and a contact can be associated with one another, and then how the contact can be used to modify a field on the account:
```apex
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
```apex
Account a : [SELECT Id, Name, Contact, Contact.LastName, Contact.FirstName FROM Account WHERE Name = 'Acme];
contact c = a.Contact;
```

### Improve Performance by Avoiding Null Values
In your SOQL and SOSL queries, explicitly filtering out null values in the WHERE clause allows Salesforce to improve query performance. In the following example, any records where the Thread__c value is null are eliminated from the search: 
```apex
[SELECT Name FROM CSO_CaseThread_Tag__c WHERE Thread__c = :ID AND Thread__c != null]
```

### Some pointers
- If you try to access a field that was not selected in the SOQL or SOSL query (other than ID), you receive a runtime error. 
- If only one sObject field is selected, a SOQL or SOSL query always returns data as complete records. Consequently, you must dereference the field in order to access it.