Apex enables you to insert, update, delete or restore data in the database through using DML operations either one record at a time or in bulk.
performing bulk operations on multiple [[SObjects & Objects]] as appose to multiple single records is the recommended way to avoid hitting governor limits, such as the 150 statements per Apex transaction.

The following code snippet is an example of using DML calls inefficiently as if the list contains more than 150 items it will strop and return exceptions at the 151st item:
```apex
List<Contact> conList = [Select Department , Description from Contact];
for(Contact badCon : conList) {
    if (badCon.Department == 'Finance') {
        badCon.Description__c = 'New description';
    }
    // Not a good practice since governor limits might be hit.
    update badCon;
}
```
Instead (for the same example) we should use the following code to perform the same operations as 1 DML statement: 
```apex
// List to hold the new contacts to update.
List<Contact> updatedList = new List<Contact>();
List<Contact> conList = [Select Department , Description from Contact];
for(Contact con : conList) {
    if (con.Department == 'Finance') {
        con.Description = 'New description';
        // Add updated contact sObject to the list.
        updatedList.add(con);
    }
}

// Call update on the list of contacts.
// This results in one DML call for the entire list.
update updatedList;
```
