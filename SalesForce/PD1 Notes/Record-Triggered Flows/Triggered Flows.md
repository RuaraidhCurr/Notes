
Flow Types

|Flow Type|Launched By|Description|
|---|---|---|
|Screen Flow|- Quick action<br>- Lightning page<br>- Experience Cloud site, and more|Screen Flows provide a UI that guides users through a business process.|
|Autolaunched Flow|- Another flow<br>- Apex code<br>- REST API|Autolaunched Flows automate business processes that have no UI. They have no trigger and they run in the background.|
|Triggered Flow|- Time<br>- Data change<br>- Platform event|Triggered Flows are autolaunched by a trigger you specify. They run in the background.|

Flow Builder provides debugging, testing, and integration with functions across the salesforce platform. It combines capabilities from Workflow Rules & Process Builder with the power of Flow for a single no-code automation home. 

## Trigger Flows
Trigger flows consists of a **trigger**, at least on **criterion**, and at least one **action** 
- Trigger identifies what launches the flow
- Criteria define the specifics of the trigger
- Action determines what the flow does 

## Trigger Types

There are three types of triggers.

| Trigger Type   | When It Runs                                         | How to Use It                              |
| -------------- | ---------------------------------------------------- | ------------------------------------------ |
| Schedule       | At a time and frequency you specify                  | Running nightly batch jobs                 |
| Platform Event | When a particular platform event message is received | Subscribing to events                      |
| Record         | When a record is created, updated, or deleted        | Updating records and sending notifications |

## Record-Triggered Flows
A record-triggered flow is the most commonly used automation. It's the best way to interact with records in you org.

For record-triggered flows, the trigger determines which object the Flow acts on and when it runs.
- When a record is created
- When a record is updated
- When a record is created or updated
- When a record is deleted 

Because the flow is triggered when a record has been changed, that change is already on its way to the database. This is often called a transaction, and is referred to as the initial triggering transaction.

|Option|When It Runs|How to Use It|
|---|---|---|
|Fast Field Update|During the record update that triggered the flow and before that update is saved.|Updating the record that triggered the transaction<br><br>Benefit: Optimal performance because it’s limited to updating the triggering record|
|Related Records and Actions|During the record update that triggered the flow and after that update is saved.|1. Creating, updating, or deleting other records<br>2. Calling subflows<br>3. Calling actions, such as send email alert or post to Chatter<br><br>  <br>Benefit: Automating common processes triggered by record changes|
|Run Asynchronously|Immediately after the record update that triggered the flow is complete.|Executing more advanced scenarios like sending requests to external systems or performing other longer running processes<br><br>Benefit: Avoids slowing down or blocking the record update that triggered the flow|
|Scheduled Paths|In the future, after the trigger has fired, based on dates and times.|Scheduling reminders or follow-ups based on dates in the record that triggered the flow, such as Close Date<br><br>Benefit: Waits a specified amount of time between the trigger firing and the automation running|


### Record triggered flow
