# Repeat billing System

Invoices **(Invoice__c)** are generated upon first purchase and is more a formality object containing one or more payment transactions **(Payment_Trasaction__c)** or subscriptions  **(Subscriptions__c)**. Both the payment transactions and subscriptions use "Invoice Lines/Products" **(Payment_Transaction_Line_Item__c)** as the items in their "Basket". 

To Process a payment a Payment Transaction needs to be created. This is collected on the payment Transactions Due date where *Asperato* will update it as either Paid or Failed. 

Both subscriptions and Payment transactions need to be Authorized. We don't store any PCI (Payment card industry) card details within salesforce. We instead use "Repeat Billing Authority" **(Continous_Authority__c)** and we cross reference repeat order IDs to collect future payments via *Asperato*. 
Diagram below:![[620cdf81d2478-image.png]]

# Repeat Payment Run
On the day that payments are due, we lock records for payments at 4am, *Asperato* runs at 5am and processes any Due/Failed Payments. Any payments that happen immediately are unlocked and synced in SF for 6am meaning invoices are set to "Paid" or "Closed Won". Between 6am and 11 we wait for the slower payments responses. Then at 1pm the slower payments are synced with SF and set to "Paid" or "Closed Won". 

**During this whole time period payments may be taken but not yet synced with SF**

Diagram Below![[Pasted image 20231025094810.png]]