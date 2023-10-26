The handling of triggers are not in the actual trigger class. Each trigger class simply runs a handler e.g. "TriggerOnAccount.trigger" simply contains:
``` java
trigger TriggerOnAccount on Account (before insert, after insert, before update, after update) {
    AccountHandler.handleTrigger();
}
```
The handler class is called in the trigger in this case "AccountHandler.cls"