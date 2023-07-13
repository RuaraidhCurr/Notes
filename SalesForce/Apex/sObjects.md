SObject is any object that can be stored in the lightning platform database. An object can be thought of as an ***Rows & Columns in an excel spread sheet*** with the ***object (record)*** being the ***name*** of the sheet, or what the sheets data represents, the ***field names*** can be thought of as the ***column headers*** & the individual record IDs data can be thought of as the ***row headers*** & the data can be thought of as the  ***individual cells*** within the ***rows*** of data. Example below

| **sObject Name**        |      |     |         |
| ------------ | ------- | ------- | ------- |
| **ID**           | **Field 1** | **Field 2** | **Field 3** |
| #1           | data1   | data2   | data3   |
| #2           | data1   | data2   | data3   |
| #3           | data1   | data2   | data3   |

### SObject Types

Instantiating a new sObject in Apex is simple: Example below
```
Account a = new Account();
MyCustomObject__c co = new MyCustomObject__c();

or 

Object obj = s;
// and
a = (Account)obj;

```


we can specify initial field values when instantiating a new sObject e.g.
```
Account a = new Account(name = 'Acme', billingcity = 'San Francisco');
```
### Lists of sObjects
Lists can contain sObjects amongst other element types. You can use lists to store sObjects which is useful for bulk processing without hitting governor limits. 

To declare a list of sObjects, use the `List` keyword followed by the sObject type within `<>` characters. For example:
```apex
// Create an empty list of Accounts
List<Account> myList = new List<Account>();

// auto populate this list with accounts
myList = [SELECT Id, Name FROM Account LIMIT 1000];
```

### SObject Fields

As in Java, sObject fields can be accessed or changed with simple dot notation. For example:
```
Account a = new Account();
a.Name = 'Acme';    // Access the account name field and assign it 'Acme'

or 

Account a = new Account(Name = 'Acme', BillingCity = 'San Francisco');
```

System generated fields such as `ID`, `Created date`, `last modified by`, etc cannot be modified. 

If you use the generic sObject type instead of a specific object (such as `Account` or `MyCustomerObject__c`) you can only retrieve the `ID` field using dot notation. 
```
Account a = new Account(Name = 'Acme', BillingCity = 'San Francisco');
insert a;
sObject s = [SELECT Id, Name FROM Account WHERE Name = 'Acme' LIMIT 1];
// This is allowed
ID id = s.Id;
// The following line results in an error when you try to save
String x = s.Name;
// This line results in an error when you try to save using API version 26.0 or earlier
s.Id = [SELECT Id FROM Account WHERE Name = 'Acme' LIMIT 1].Id;
```
If you want to edit an SObject, you should first convert it into a specific object. For example:
```
Account a = new Account(Name = 'Acme', BillingCity = 'San Francisco');
insert a;
sObject s = [SELECT Id, Name FROM Account WHERE Name = 'Acme' LIMIT 1];
ID id = s.ID;
Account convertedAccount = (Account)s;
convertedAccount.name = 'Acme2';
update convertedAccount;
Contact sal = new Contact(FirstName = 'Sal', Account = convertedAccount);
```
