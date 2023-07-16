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

efore-save Apex triggers are faster than before-save Flow triggers, and before-save Flow triggers are 10x faster than Workflow Rules.



### Cross-Object CRUD

^56710b



### Complex List Processing

^fcaf8a



### [[Asynchronous Apex|Asynchronous Processing]]

^bbbe3b



### Custom Validation Errors

^008713

