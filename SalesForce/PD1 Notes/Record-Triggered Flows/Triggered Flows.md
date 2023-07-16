
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

## Trigger Explorer
You can change the order in which triggered flows run. perhaps you need one flow to edit a field before another flow uses it, or you want the related records that each flow creates to appear in a certain order. with Flow Trigger Explorer, you can change the order that flows run in without having to open each flow and change their settings. 

To change the order that the flows run:

1. Click and hold <img src="https://res.cloudinary.com/hy4kyit2a/f_auto,fl_lossy,q_70/learn/modules/record-triggered-flows/meet-flow-trigger-explorer/images/5c901af897273ca5705d926c2eeebf02_70730-dc-9-8636-48-bf-9-e-6-b-cd-4-f-754-d-6-f-57.png"> next to the flow you want to reorder. Drag it to the place in the flow order that you want this flow to run.
2. Click the **Edit Order** button in the section you want to reorder.
3. Repeat these steps for any other flows you want to reorder in this section. Flow Trigger Explorer highlights each flow you move, so you know what’s going to be changed.
<img src="https://res.cloudinary.com/hy4kyit2a/f_auto,fl_lossy,q_70/learn/modules/record-triggered-flows/meet-flow-trigger-explorer/images/140a2cdc125eb3ff1a5625abcf6ff458_b-5-d-32-ea-4-bc-69-4585-8624-b-82011-cd-9-ea-4.png" />

## Monitoring Record-Triggered Flows
to monitor your Record-Triggered-Flows you use the Time-Based Workflow page.

The Time-Based Workflow shows you individual instances of the flow so that you can monitor pending automation.

To access the Time-Based Workflow page follow these steps:
1. From Setup, in the Quick Find box, enter `Time` and then select **Time-Based Workflow**.
2. Click **Search** to view all pending actions for any active flows.
3. To filter the list of pending actions, define filter criteria (using filter type, operator, and value) and then click **Search**.

To cancel pending actions:
1. Select the pending actions that you want to cancel.
2. Click **Delete**.

