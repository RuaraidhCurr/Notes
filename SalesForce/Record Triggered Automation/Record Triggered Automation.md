*More Notes can be found [here](https://architect.salesforce.com/decision-guides/trigger-automation)*

We will talk about tools and recommendations for various triggered automation tools and why to choose them. We will also talk about how Flow automatically handles bulkification and recursion control, as well as some points on performance and automation design. 

The table below shows the most common trigger use case, and the tolls we believe are well-suited for each. 

| |Low Code|-------| -------  | Pro Code|
|---|---|---|---| --- |
||Before-Save Flow Trigger|After-Save Flow Trigger|After-Save Flow Trigger + Apex| [[Apex Triggers]] |
|[[Record Triggered Automation#^f4713e\|Same-Record Field Updates]] |Available|Not Ideal|Not Ideal|Available|
|[[Record Triggered Automation#^7bdea8\|High-Performance Batch Processing]]|Not Ideal|Not Ideal|Not Ideal|Available|
|[[Record Triggered Automation#^56710b\|Cross-Object CRUD]] |Not Available|Available|Available|Available|
|[[Record Triggered Automation#^bbbe3b\|Asynchronous Processing]] |Not Available|Available|Available|Available|
|[[Record Triggered Automation#^fcaf8a\|Complex List Processing]]|Not Available|Not Ideal|Available|Available|
|[[Record Triggered Automation#^008713\|Custom Validation Errors]]|Not Available|Not Available|Not Available|Available|
- **Available** = should work fine, with basic considerations.
- **Not Ideal** = possible, but with important and potentially limiting considerations.
- **Not Available** = no plans to support in any capacity in the next twelve months.

If your team already has Apex developers that have an established CI/CD pipeline for creating & handling triggers than it is probably best to keep to that rather than re-organizing your operations to adapt for flow development. On the other hand if you don't have many Apex developers or a low to no culture of code quality than your org might be best served switching over to flows. 

## Process Builder & Workflow Rules
The retirement of the Process Builder & Workflow Rules is recommended so that you can start implementing low-code automation going forward. Flows are better designed to meet the increasing functionality and extensibility requirements of salesforce customers today. 

### Workflows
The vast majority of workflows are used for same-record field updates. While Workflow Rules have a reputation for being fast, they nevertheless cause a recursive save, and will always be considerably slower and more resource-hungry than a single, functionally equivalent before-save flow trigger.

### Process Builder
The Process Builder will always be less performant and harder to debug than Flow

## Use Case Considerations

### Same-Record Field Updates

^f4713e

| |Record-Changed Flow: Before Save|Record-Changed Flow: After Save|Record-Changed Flow: After Save + Apex|Apex Triggers|
|---|---|---|---|---|
|Same-Record Updates|Available|Not Ideal|Not Ideal|Available|


It is recommended to reduce / minimize the number of same-record field updates that occur after the save. Instead, do start implementing same-record field update actions in before-save flow triggers or before-save Apex triggers. Before-save same-record field updates are faster, by design for these reasons:
1. The record's field values are already loaded into memory
2. The update is performed by changing the values of the record in memory 

In our experiments, bulk same-record updated performed anywhere between 10-20x faster when doing before-save triggers. 

The limitations for before-save flow triggers are that they are functionally spares: you can query records, loop. evaluate formulas, assign variables, perform decisions for logic and can make updates to the underlying record. 

We know that same-record field updates account for the lion’s share of Workflow Rule actions executed site-wide, and are also a large contributor to problematic Process Builder execution performance.

### High-Performance Batch Processing

^7bdea8

| |Record-Changed Flow: Before Save|Record-Changed Flow: After Save|Record-Changed Flow: After Save + Apex|Apex Triggers|
|---|---|---|---|---|
|High-Performance Batch Processing|Not Ideal|Not Ideal|Not Ideal|Available|

If you need highly performant evaluation of complex logic in batch scenarios, then Apex and its rich debug and tooling capabilities are for you. Reasons:
1. The ability to define and evaluating complicated logic expressions or formulas
2. Ability for complex list processing, loading and transforming data from large numbers of records loops over loops of loops.
3. Working with anything that requiring [[Maps|Map]]-like or [[Sets|Set]]-like functionalities.
4. If you need Transaction savepoints. 

Before-save Apex triggers are faster than before-save Flow triggers, and before-save Flow triggers are 10x faster than Workflow Rules.

While Flows have some capabilities they continue to be constrained and less feature-rich than Apex. Scheduled flows can currently do batch operations of up to 250,000 records per day.

### Cross-Object CRUD

^56710b

| |Record-Changed Flow: Before Save|Record-Changed Flow: After Save|Record-Changed Flow: After Save + Apex|Apex Triggers|
|---|---|---|---|---|
|Cross-Object CRUD|Not Available|Available|Available|Available|

Creating, Reading, Updated, or Deleting (CRUD), requires database operations, no matter what tool you use. 

Currently, Apex is faster than Flows in raw database operation speed. 

The most inefficient user implementations tend to issue multiple [[Adding and Retrieving Data with DML|DML]] statements where fewer would suffice. For example, here is an implementation of a flow trigger that updates two fields on a case's parent account record with two update record elements. 
<img src="https://architect.salesforce.com/1/asset/immutable/s/0672dcd/assets/images/automation-1.png"  width="500px">

This is suboptimal and could be done with just one [[Adding and Retrieving Data with DML|DML]] operation. 

Workflow Rules have gained rep for being highly performant. Part of this is attributed to how workflow rules constrain the amount of [[Adding and Retrieving Data with DML|DML]] performed.

### Complex List Processing

^fcaf8a

| |Record-Changed Flow: Before Save|Record-Changed Flow: After Save|Record-Changed Flow: After Save + Apex|Apex Triggers|
|---|---|---|---|---|
|Complex List Processing|Not Available|Not Ideal|Available|Available|

There are a few major list processing limitations in Flow today
1. Flows offers a limited set of basic list processing operations out of the box
2. There's no way to reference an item in a Flow collection, either by index or by using Flow's Loop functionality.
3. Loops are executed serially during runtime, even during batch processing. For this reason, any [[SOQL & SOSL Queries|SOQL]] or [[Adding and Retrieving Data with DML|DML]] operations that are enclosed within a loop are not bulkified. 

The combination of these limitations makes some common processing tasks overly cumbersome to achieve in Flow while being much more straightforward in Apex. 

This is where extending Flows with [[Invokable Apex]] can really shine. Since these methods are declared as invokable methods, they are automatically made available to Flow users.

### [[Asynchronous Apex|Asynchronous Processing]]

^bbbe3b

| |Record-Changed Flow: Before Save|Record-Changed Flow: After Save|Record-Changed Flow: After Save + Apex|Apex Triggers|
|---|---|---|---|---|
|Fire & Forget Asynchronous Processing|Not Available|Available|Available|Available|
|Other Asynchronous Processing|Not Available|Available|Available|Available|


_Asynchronous processing_ has many meanings in the world of programming, but when it comes to record-triggers, there are a couple topics that generally arise. It's often requested in opposition to the default option, which is to make changes synchronously during the [[Order of Execution]].

**Benefits of synchronous Processing**:
- **Minimal Database Transactions**: Record-change triggers are generally configured to run during the initial transaction in order to optimize database transactions.
- **Consistent Rollbacks**: for updates to other records, compiling changes into the initial transaction means that the overall change to the database will be atomic from a data integrity standpoint, and rollbacks can be handled together

**Downside of Synchronous Processing**:
- **Time Window**: The record-trigger starts with an open transaction to the database that cannot be committed until all the steps in the trigger order of execution have happened.
- **[[Governor Limits]]**
- **Support for External Objects and Callouts**: In general, any access to an external system that needs to wait for a response will be too long to do within the original open transaction.
- **Mixed DML: Occasionally**, you may want to do cross-object CRUD on objects in Setup and non-Setup, like updating a user and an associated contact after a particular change.

With these considerations in mind, for Apex, we recommend implementing asynchronous processing inside a Queueable Apex class. For Flow, we recommend using the Run Asynchronously path in after-save flows to achieve a similar result in a low-code manner.

When testing it's important to think about what happens if a particular step has an error, timeout or sends back malformed data. In general asynchronous processing has more power, but requires the designer to be more thoughtful about edge cases. 

When it comes to asynchronous processing, it may take additional care and consideration to design what your record-triggered automation, particularly if you require callouts to external systems or need to perpetuate state between processes. The Run Asynchronously path in Flow should meet many of your low-code needs, but complex ones around custom errors or configurable retries will require Apex instead. 

### Custom Validation Errors

^008713

|                          | Record-Changed Flow: Before Save | Record-Changed Flow: After Save | Record-Changed Flow: After Save + Apex | Apex Triggers |
| ------------------------ | -------------------------------- | ------------------------------- | -------------------------------------- | ------------- |
| Custom Validation Errors | Not Available                    | Not Available                   | Not Available                          | Available     |

*At the time*, Flow provides no way to either prevent DML operations from committing, or to throw custom errors the `addError()` apex method is not supported when executed from Flow via Apex invokable method. 


## Designing Your Record-Triggered Automation 

### What Problems Are You Trying to Solve?

#### Performance
Making your record-triggered automation performant is a multidimensional problem, and no design rule will encompass all the factors for Flow, there are two important points to remember when it comes to your design:
1. Consolidating you automation into a single flow will not have major impact on performance completed to splitting it into multiple flows
2. Entry conditions can be lead into significant performance improvements for your record-triggered automation if they are used to exclude changes that do not impact a specific use case. 

#### Troubleshooting
As architects, we would love to never have to troubleshoot automation, but we do from time to time. While having your automation spread out among multiple tools can work during initial development, it often causes more headaches over time as changes are made in different places. This is where the advice to consolidate your automation on a single Object into either Apex or Flow comes from.

Given that it may be prudent to consolidate an object's automation into a single tool when maintenance, debugging, or conflicts are likely to be a concern.

#### Ordering 
In the past the need for ordering has led to recommendations for consolidating all automation into a single flow. With flow trigger ordering, there is now no need to do that. 

#### Organisational Issues 
Ultimately, the best approach is one that works well with your business and org. If you feel abut lost, the Trailblazer Community is full of advice on how to manage a complex organization, so dig in and ask questions as you can learn how to better match you unique business and admin setup with the product.