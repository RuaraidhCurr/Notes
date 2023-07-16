We will talk about tools and recommendations for various triggered automation tools and why to choose them. We will also talk about how Flow automatically handles bulkification and recursion control, as well as some points on performance and automation design. 

The table below shows the most common trigger use case, and the tolls we believe are well-suited for each. 

|Low Code|-------| -------  |Pro Code|
|---|---|---|---|
||Before-Save Flow Trigger|After-Save Flow Trigger|After-Save Flow Trigger + Apex|Apex Triggers|
|Same-Record Field Updates|Available|Not Ideal|Not Ideal|Available|
|High-Performance Batch Processing|Not Ideal|Not Ideal|Not Ideal|Available|
|Cross-Object CRUD|Not Available|Available|Available|Available|
|Asynchronous Processing|Not Available|Available|Available|Available|
|Complex List Processing|Not Available|Not Ideal|Available|Available|
|Custom Validation Errors|Not Available|Not Available|Not Available|Available|
- **Available** = should work fine, with basic considerations.
- **Not Ideal** = possible, but with important and potentially limiting considerations.
- **Not Available** = no plans to support in any capacity in the next twelve months.

If your team already has Apex developers that have an established CI/CD pipeline for creating & handling triggers than it is probably best to keep to that rather than re-organizing your operations to adapt for flow development. On the other hand if you don't have many Apex developers or a low to no culture of code quality than your org might be best served switching over to flows. 

## Process Builder & Workflow Rules
The retirement of the Process Builder & Workflow Rules is recommended so that you can start implementing low-code automation going forward. Flows are better designed to meet the increasing functionality and extensibility requirements of salesforce customers today. 

