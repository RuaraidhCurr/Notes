Apex is a programming language that uses Java-like syntax and acts like a database stored procedures. Apex enables developers to add business logic to system events, such as button click, updates of related records, and Visualforce pages

As a language, Apex is:
- Hosted - Apex is saved, compiled, and hosted on the server - the Lighting Platform
- Object orientated - Apex supports classes, interfaces, and inheritance
- Strongly typed - Apex validates references to objects at compile time
- Multitenant aware - The Lightning Platform guards against runaway code by enforcing limits, which prevent code from monopolizing shared resources
- Integrated with the database - Apex provides direct access to records and their fields, and provides statements and query languages to manipulate those records. 
- Data focused- Apex provides transaction access to the database, allowing you to roll back operations
- Easy to use - Apex is based on familiar Java idioms 
- Easy to test - Apex provides built-in support for unit test creation, execution, and code coverage. 
- Versioned - Custom Apex code can be saved against different version of the API

**Apex Language Highlights**
Language constructs that Apex support:
- [[Classes]], interfaces, properties, and collections
- [[SObjects & Objects|Object]] and array notation
- Expressions, variables and constants
- [[Conditional (If-Else) Statements|Conditional Statements]] (if-then-else) and control |flow statements ([[For Loops|for loops]] and [[While Loops|while loops]])

Unlike other object-oriented programming languages, Apex supports:
- Cloud development
- [[Apex Triggers|Triggers]]
- [[Adding and Retrieving Data with DML|Database statements]] that allow you to make direct database calls and query languages to query and search data 
- The [[Access Modifiers#^88dd84|global]] [[Access Modifiers|access modifier]], which is more permissive than the [[Access Modifiers#^4b5b07|public]] modifier and allows access across all [[Namespaces|namespaces]] and applications. 
- Versioning of custom code

## Data Types Overview
Apex supports various data types, including a data type specific to Salesforce—the sObject data type.

Apex supports the following data types.
- A [[Primitive Data types|primitive]], such as an Integer, Double, Long, Date, Datetime, String, ID, Boolean, among others
- An [[SObjects & Objects|sObject]], either as a generic sObject or as a specific sObject, such as an Account, Contact, or MyCustomObject__c
- A collection, including:
    - A [[Lists|list]] (or array) of primitives, sObjects, user defined objects, objects created from Apex classes, or collections
    - A [[Sets|set]] of primitives, sObjects, user defined objects, objects created from Apex classes, or collections
    - A [[Maps|map]] from a primitive to a primitive, sObject, or collection
- A typed list of values, also known as an **_enum_**
- User-defined Apex classes
- System-supplied Apex classes

