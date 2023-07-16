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

## Search Within a Single Object