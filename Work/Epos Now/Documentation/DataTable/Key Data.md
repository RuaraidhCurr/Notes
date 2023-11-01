## Account
Represents an account record for a customer, partner or reseller. A Salesforce account is linked to a Backoffice via a unique **GUID**
## Asset
Represents a specific stock item with a serial number. It tracks the dispatch provider and associated customer account
## Case
Customer support tickets containing information regarding the customers issue and related details in solving the problem
## Commission 
Individual commission record attributing to the amount to be paid to a rep or manager including both upfront SPIFFs and contributions toward the monthly commission threshold
## Contact
Represents an individual person associated with an account and their contact information for sending SMS and emails
## Customer Satisfaction 
Feedback and reviews from our customers, Scores are used in calculating customers **NPS** and satisfaction **Star Rating**
## Event
A calendar booking with a Start and End time. Used for booking such as Demo's and training Calls
## Lead
Inbound unqualified leads from our website and partners. Once qualified can be converted to an Account, Contact & Opportunity
## Vonage Call Summery
Part of the Vonage (NVM) Package. represents a call made through Salesforce and the summary breakdown of that call.
## Opportunity 
Tracks and manages potential deals. Opportunity records track details about deals, including basket products and the amount of the potential sales. An opportunity is created when a lead is converted or created to upsell to a customer. If sold the stage will be 'Closed Won' or if lost the stage will be 'Closed Lost'
## App/Partner Referral
Represents a specific product line attached to a Payment Transaction (Links a subscription to a payment transaction)
## Payment transaction 
An invoice for an individual billed transaction on a specific date. Stores information regarding the success of that payment. These records are generated in advance and trigger payments to be taken on specified Due Date
## Price Book Entry
Individual template pricing products that are available for sale with separate entries for each currency and pricebook. Entries from multiple pricebooks are combined to generate the final price.
## Product 
Represents a specific product/SKU for a Hardware Item, Subscription or App. Contains any settings or configuration for the product setup.
## Subscription
Access to a specific product(service, feature or application), as well as laying out the billing setup for this purchase. A subscription is required for any purchase. 
## SMS History
Sent, Inbound and Outbound SMS message history records containing the content of the SMS or relevant user
## Usage 
Individual account usage such as Backoffice Tenders (including how many transactions & volumes from last 28days)