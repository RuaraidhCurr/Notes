# Search Solutions

All record are stored as data fields in the org's database. when you update or create a record, the search engine comes along, makes a copy of the data, and breaks up the content into smaller pieces called tokens. We store these tokens in the search index, along with a link back to the original record.

Search index and tokens allow the search engine to apply advanced features like spell correction, nicknames, lemmatization, and synonym groups. This widens the net of results for the users search. 

## Connect to Search with APIs

Both [[SOQL & SOSL Queries|SOQL and SOSL]] format text queries in a given API. But that’s where the similarities end. A [[SOQL & SOSL Queries|SOQL]] query is the equivalent of a SELECT SQL statement and searches the org database. On the other hand, [[SOQL & SOSL Queries|SOSL]] is a programmatic way of performing a text-based search against the search index. [[SOQL & SOSL Queries|SOSL]] uses all the great, previously mentioned features of the search index.

Use [[SOQL & SOSL Queries|SOQL]] when you want to:
- Retrieve data from a single [[SObjects & Objects|object]] or from multiple objects that are related to one another
- Count the number of records that meet specified criteria
- Sort results as part of the query
- Retrieve data from number, date, or checkbox fields 

Use [[SOQL & SOSL Queries|SOSL]] when you don't know in which [[SObjects & Objects|object]] or field the data resides and you want to: 
- Retrieve data for a specific term that you know exists within a field
- Retrieve multiple objects and fields efficiently
- Retrieve data for a particular division in an organization using the divisions features

## Search Within a Single Object#

To search within a single [[SObjects & Objects|object]] using [[SOQL & SOSL Queries|SOSL]], simply specify that object in the request:
```apex
FIND {term} RETURNING ObjectTypeName
```
In the example, `term` is what the user enters. ObjectTypeName limits search results to include only the [[SObjects & Objects|sObject]] specified. So if the user wants to find the March 2016 email campaign, the request looks like:
```apex
FIND {march 2016 email} RETURNING Campaign
```

## Search Within Multiple Objects
To search within multiple [[SObjects & Objects|Objects]] just add a comma-separated list
```apex
FIND {term} RETURNING ObjectTypeName1, ObjectTypeName2, ObjectTypeNameYouGetTheIdea
```
e.g.
```apex
FIND {recycled materials} RETURNING Product2, ContentVersion, FeedItem
```

## Search Within Custom Objects
To search within custom objects include the [[SObjects & Objects|sObject]] name and append a `__c` suffix. 
```apex
FIND {pink hi\-top} RETURNING Merchandise__c
```


# Optimize Search Results

## Create Efficient Text Searches
The more data you’re searching through and the more results you’re returning, the more you can slow down the entire operation.

How do you combat sluggish searches? With two basic strategies.
- Limit which data you’re searching through
- Limit which data you’re returning

To limit which data is searched, use `IN` SearchGroup. You can search for name, email, phone, sidebar, or all fields. For example, if you want to search only for an email, you search through only email fields.
```apex
FIND {jsmith@cloudkicks.com} IN EMAIL FIELDS RETURNING Contact
```

In SOSL, you can use `RETURNING FieldSpec` to specify which data is returned. More advanced elements it includes.
- `ObjectTypeName`—Specifies the object to return.
- `FieldList`—Specifies the fields to return.
- `ORDER By`—Specifies which fields to order the results by. You can also specify ascending or descending order.
- `LIMIT n`—Sets the maximum number of records returned for the given object.
- `OFFSET n`—Sets the starting row offset into the result set returned by your query.

|Step|Goal|Example|
|---|---|---|
|1|Specify the object to return.|```FIND {Cloud Kicks} RETURNING Account```<br>|
|2|Specify the field to return.|```FIND {Cloud Kicks} RETURNING Account(Name, Industry)```|
|3|Order the results by field in ascending order, which is the default.|```FIND {Cloud Kicks} RETURNING Account (Name, Industry ORDER BY Name)```|
|4|Set the max number of records returned|```FIND {Cloud Kicks} RETURNING Account (Name, Industry ORDER BY Name LIMIT 10)```|
|5|Set the starting row offset into the results.|```FIND {Cloud Kicks} RETURNING Account (Name, Industry ORDER BY Name LIMIT 10 OFFSET 25)```|

`WITH` filters allow you to reduce the return from a SOSL query meaning fewer results and improved performance. Types of `WITH` filters: 

| Resource           | Example                                                                                                                                                                      |
| ------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| WITH DIVISION      | ```FIND {Cloud Kicks} RETURNING Account (Name, Industry) WITH DIVISION = 'Global'```                                                                                  |
| WITH DATA CATEGORY | ```FIND {race} RETURNING KnowledgeArticleVersion (Id, Title WHERE PublishStatus='online' and language='en_US')    WITH DATA CATEGORY Location__c AT America__c``` |
| WITH NETWORK       | ```FIND {first place} RETURNING User (Id, Name), FeedItem (id, ParentId WHERE CreatedDate = THIS_YEAR Order by CreatedDate DESC) WITH NETWORK = '00000000000001'```    |
| WITH PRICEBOOK     | ```Find {shoe} RETURNING Product2 WITH PricebookId = '01sxx0000002MffAAE'```                                                                                                 |

## Display Suggested Results
To take advantage of this feature, here are your go-to [[REST]] resources. Each resouce has similar syntax and parameters, but use the one that best fits your use case. 
- Search Suggested records - Returns a list of suggested records who's names match the user's search string.
- Search Suggested Artic