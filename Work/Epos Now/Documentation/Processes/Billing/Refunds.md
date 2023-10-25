**Accounts** generate a **Refund** record via the "Refund" Button on a payment record. Once a refund has been processed the status will be changed to "Refunded to customer". A separate negative payment transaction record is also generated for the refund amount. 

# Creating a refund case:
1. Create a case on the Account
2. Click the button "Raise Refund"
3. Fill in any info that isn't automatically filled in on the the screen flow (Credit Due date is **TODAY()+28**)
4. On the next screen you can select one or more payment transactions **(Payment_Trasaction__c)** you want to refund and the total refund amount with  any supporting information.
5. The "Create Refund Case" doesn't actually create a new case. It just updates the case that you are on. 