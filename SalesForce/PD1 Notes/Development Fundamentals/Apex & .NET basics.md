---
tags: Apex, Lightning Platform, Data types, database, DB, Development Tools, .NET, Visualforce, MCV
---
## Map .NET Concepts to the Lightning Platform

Salesforce is a metadata driven platform. Everything from code, configurations, apps, flows are all specified as metadata. Lightning platform is tightly integrated with the database, meaning certain things are integrated into the system, eg reporting, GUIs, security etc and you can build on top of & customize your database very quickly.

## Apex
Basic syntax for defining a class:
	``` apex
private | public | global
[virtual | abstract | with sharing | without sharing]
class ClassName [implements InterfaceNameList] [extends ClassName]
{
    // The body of the class
}
	```
### [[Access Modifiers|Class Modifiers]]:

^c54648

- **[[Access Modifiers#^5d2efd|Private]]** - `private` class is only known locally. (inner classes are defaulted to private)
- **[[Access Modifiers#^4b5b07|Public]]** - `public` classes are visible in your application namespace
- **[[Access Modifiers#^88dd84|Global]]** - `global` classes are visible everywhere, all apex classes have access to them. examples would be HttpEndpoints, batch classes, schedulable apex
- **With Sharing** - `with sharing` declares that a class runs in the current users sharing/access rules
- **Without Sharing** - `without sharing` declares that the current users sharing/access rules are not enforce

### Data Types:
- **[[Primitive Data types]]** - `Integer`, `Double`, `Date`, `Long`, `String`, `ID`, `Boolean`, `Datetime`
- **[[SObjects & Objects|sObjects]]** - `sObject` is a generic object that you use when you're not sure what the object type is going to be.
- **collections**:
	- **[[Lists]]**  - `List` of items, can be strings, objects, integers, [[SObjects & Objects|sObjects]]
	- **[[Sets]]** - `Sets` An unordered collection of elements(any primitive data type) that does no contain duplicates. 
	- **[[Maps]]** - `Maps` List of key value pairs e.g. **Key** | United States || **Value** | Dollar

### [[Visualforce]]
Visualforce is a tag-based mark-up language framework for rendering HTML pages using [[Model View Controller (MVC)|MVC]] paradigm. Visualforce pages use standard controllers and extension controllers. standard controllers is system generated code that allows you the quickly incorporate basic Create, Read, Update, Delete (**CRUD**) functionality. extension controllers are ones you create yourself and let you give added functionality to a page. 

### Apex & the Database(DB)
Apex and the lightning platform are tightly coupled to the point they are sometimes indistinguishable. each standard and custom object in the DB has their own Apex class that provides standard functionality to make for quick interaction. Whenever you create a field on an object a class member is automatically surfaced to reference the values in the DB. it's also impossible to add a reference to a field on an object in apex without that field exist on that object. The platform works hard to make sure that the database schema and your code doesn't come out of sync.

### Unit tests are Required
Salesforce requires above 75% of apex code test coverage on any org. 

### No Solution, Project, or Config Files
the lightning platform has no such thins as a soliton or project file. You can create an application, but it's not the same as creating a .NET application or assembly. An App on the lightning platform is a collection of tabs, reports, dashboards and pages (several of which come built into your salesforce org). Apps can be purchased on the AppExchange. 

All your code resides and is executed in the cloud automatically. 

### Development Tools
- Developer Console Application
- VS Code
- Salesforce DX (SFDX)


### Handling Security
no Authentication, passwords and database connection strings. ID is handled by the platform and access can be controlled from different levels, including object level, record level, field level. 

