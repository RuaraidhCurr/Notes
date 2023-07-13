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

### SObject Fields

As in Java, sObject fields can be accessed or changed with simple dot notation. For example:
```
Account a = new Account();
a.Name = 'Acme';    // Access the account name field and assign it 'Acme'

or 

Account a = new Account(Name = 'Acme', BillingCity = 'San Francisco');
```

System generated fields such as `ID`, `Created date`, `last modified by`, etc cannot be modified. 

