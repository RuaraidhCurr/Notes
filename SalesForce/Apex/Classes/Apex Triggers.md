Apex can be invoked by using `triggers`. Apex triggers enable you to perform actions before or after changes to Salesforce records, such as insertions, updates, or deletions.

Triggers can execute **before** and **after** the following types f operations:
- insert
- update
- delete 
- merge *(before and after both delete for losing records and update for the winning record)*
- upsert *(before and after both insert and update triggers -as appropriate-)*
- undelete
-------------------------------------------------------------------------------
- Before triggers are used to validate records before values are saved to the database
- After triggers are used to access field values and check for changes on fields which can be used to invoke other apex

## Bulk Triggers
All triggers are Bulk Triggers by default, as they can process multiple records at a time. 

## Trigger Syntax 

To define a trigger, use the following syntax:

```apex
	trigger TriggerName on ObjectName (trigger_events) {
		 code_block
	}
	
	// E.g.
	
	trigger myAccountTrigger on Account (before insert, before update) {
	    // Your code here
	}
```

- before insert
- before update
- before delete
- after insert
- after update
- after delete
- after undelete

## Trigger Context Variables
All triggers define implicit variables that allow developers to access run-time context. These variables are contained in the System.Trigger class.

|Variable|Usage|
|---|---|
|isExecuting|Returns true if the current context for the Apex code is a trigger, not a <br/>Visualforce page, a Web service, or an executeanonymous() API call.|
|isInsert|Returns true if this trigger was fired due to an insert operation, from the <br/>Salesforce user interface, Apex, or the API.|
|isUpdate|Returns true if this trigger was fired due to an update operation, from the <br/>Salesforce user interface, Apex, or the API.|
|isDelete|Returns true if this trigger was fired due to a delete operation, from the <br/>Salesforce user interface, Apex, or the API.|
|isBefore|Returns true if this trigger was fired before any record was saved.|
|isAfter|Returns true if this trigger was fired after all records were saved.|
|isUndelete|Returns true if this trigger was fired after a record is recovered from the Recycle <br/>Bin. This recovery can occur after an undelete operation from the<br/> Salesforce user interface, Apex, or the API.|
|new|Returns a list of the new versions of the sObject records.<br><br>This sObject list is only available in insert, update, and undelete triggers, and <br/>the records can only be modified in before triggers.|
|newMap|A map of IDs to the new versions of the sObject records.<br><br>This map is only available in before update, after insert, after update, <br/>and after undelete triggers.|
|old|Returns a list of the old versions of the sObject records.<br><br>This sObject list is only available in update and delete triggers.|
|oldMap|A map of IDs to the old versions of the sObject records.<br><br>This map is only available in update and delete triggers.|
|operationType|Returns an enum of type System.TriggerOperation corresponding to the current operation.<br><br>Possible values of the System.TriggerOperation enum are: BEFORE_INSERT, BEFORE_UPDATE, BEFORE_DELETE, AFTER_INSERT, <br/>AFTER_UPDATE, AFTER_DELETE, and AFTER_UNDELETE. <br/>If you vary your programming logic based on different trigger types, consider using the switch statement with different permutations of unique trigger execution enum states.|
|size|The total number of records in a trigger invocation, both old and new.|

Note

The record firing a trigger can include an invalid field value, such as a formula that divides by zero. In this case, the field value is set to null in these variables:
- new
- newMap
- old
- oldMap