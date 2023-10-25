Invoices **(Invoice__c)** are generated upon first purchase and is more a formality object containing one or more payment transactions **(Payment_Trasaction__c)** or subscriptions  **(Subscriptions__c)**. Both the payment transactions and subscriptions use "Invoice Lines/Products" **(Payment_Transaction_Line_Item__c)** as the items in their "Basket". 

To Process a payment a Payment Transaction needs to be created. This is collected on the payment Transactions Due date where *Asperato* will update it as either Paid or Failed. 

Both subsc