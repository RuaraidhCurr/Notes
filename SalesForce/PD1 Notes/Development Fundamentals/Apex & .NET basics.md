
## Map .NET Concepts to the Lightning Platform

Salesforce is a metadata driven platform. Everything from code, configurations, apps, flows are all specified as metadata. Lightning platform is tightly integrated with the database, meaning certain things are integrated into the system, eg reporting, GUIs, security etc and you can build on top of & customize your database very quickly.

## Apex
Basic syntax for defining a class:
	```
	private | public | global
	[virtual | abstract | with sharing | without sharing]
	class ClassName [implements InterfaceNameList] [extends ClassName]
	{ 
		// The body of the class 
	})
	```
### Class Modifiers:
- **Private** - `private` class is only known locally. (inner classes are defaulted to private)
- **Public** - `public` classes are visible in your application namespace
- **Global** - `global` classes are visible everywhere, all apex classes have access to them. examples would be HttpEndpoints, batch classes, schedulable apex
- **With Sharing** - `with sharing` declares that a class runs in the current users sharing rules
- **Without Sharing** - `without sharing` declares that the current users sharing rules are no enforce

### Apex Data Types:
- **[[Primitive Data types]]** - `Integer`, `Double`, `Date`, `Long`, `String`, `ID`, `Boolean`, `Datetime`
- **sObject** - `sObject` is a generic object that you use when you're not sure what the object type is going to be.
- **collections**:
	- **[[Lists]]**  - `List` of items, can be strings, objects, integers, sObjects
	- **[[Sets]]** - `Sets` An unordered collection of elements(any primitive data type) that does no contain duplicates. 
	- **[[Maps]]** - `Maps` List of key value pairs e.g. **Key** | United States || **Value** | Dollar

### [[Visualforce]]
Visualforce is a framework that renders HTML pages. 