Apex can be invoked by using `triggers`. Apex triggers enable you to perform actions before or after changes to Salesforce records, such as insertions, updates, or deletions.

Triggers can execute **before** and **after** the following types f operations:
- insert
- update
- delete 
- merge *(before and after both delete for losing records and update for the winning record)*
- upsert *(before and after both insert and update triggers -as appropriate-)*
- undelete

	Before triggers are used to validate records before values are saved to the database 
	After triggers are used to access field values and check for changes on fields which can be used to invoke other apex