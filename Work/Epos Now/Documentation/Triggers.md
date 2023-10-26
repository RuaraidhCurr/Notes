The handling of triggers are not in the actual trigger class. Each trigger class simply runs a handler e.g. "TriggerOnAccount.trigger" simply contains:
``` java
trigger TriggerOnAccount on Account (before insert, after insert, before update, after update) {
    AccountHandler.handleTrigger();
}
```
The handler class is called in the trigger in this case "AccountHandler.cls" in the handler class it identifies the type of trigger is being performed. E.g ``before insert``, ``after insert``, ``after update``. ``before update``, etc... Once the type of trigger has been identified it run the associated method e.g. ``handleBeforeUpdate``. In this method it will check the record against a set of arguments using the a method called ``checkDisabled``, where it will figure out which function is meant to run.

The ``checkDisabled`` method takes a string and returns a boolean value (*``True`` or ``False``*) dependant if the string argument is present or not inside the ``System.Label.Disabled_Methods`` label.  

Example below: 
``` java
public static void handleBeforeUpdate( List<Account> accounts )
{
	for(Account a : accounts)
	{
		if(checkDisabled('FreeTrialAccount')) FreeTrialAccount(a);
	}
}
```
