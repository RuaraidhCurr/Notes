Lower rates might require Manager Approval which can be specified on the product records. These approvals will go to **customer success below 1%** and the **managers between 1-1.24%** who handle Payments approval. This team might come back with an alternate rate, the response can be entered on the **Reject** comments box, where any required documentation can be attached to the related record. 

On **Opportunity Product** (``OpportunityLineItem``) insert, the systems copies the ``Require_Manager_Approval__c`` checkbox from the product and adds it onto the **Approval Required?** (``Approval_Required__c``) field on the new Opportunity Product record. This is the rolled up onto the opportunity record via the "**Product Approvals**" (``Product_Approvals__c``) rollup field.  **ApprovalClass.cls** is called on save, to send the opportunity for approval with the following comments "Manager Approval required to sell XXX". This then starts the "**Product Approval**" approval process that sends this to the manager of the opp owner. The result from the approval is written to the "**Approval Action**" field that triggers the "**Opportunity Product - Approval Action**" flow cia the "**OpporunityHandler**" trigger. If approced, the "**Apporval Required?**" field is unticked on the related products and if rejected, those products are instead removed from the opportunity.  

## Notes
These bundle products contain **Payment Integration** line so activates the proposition pricing the same existing payment integration providers. In addition these bundles will have a dispatch weight for the hardware component requiring Standard Shipping to be paid when sold.

# Product Required
[Epos Now Payments](https://eposnow--test.my.salesforce.com/01t3X00000FycmP) **application** - free application. Not available for sale. (Live App Id: 1230)  
[p400](https://eposnow--test.my.salesforce.com/01t5E000006OYsQ) - hardware. Not available for sale on its own. P400 SKU = UK-EN-2504
[V400m](https://eposnow--test.my.salesforce.com/01t5E000006OYt4) - hardware. Not available for sale on its own. V400m SKU = UK-EN-2505  
[Saturn S1F2](https://eposnow.my.salesforce.com/01t3X00000J24yr) - hardware. Not available for sale on its own. SKU = UK-EN-2539  

[Epos Now Payments p400](https://eposnow--test.my.salesforce.com/01t5E000006OGZP)  - £15 per month (Non-primary Subscription Bundle Product)  
- Bundle of Epos Now Payments App (Free), Payment Integration (to trigger proposition)  
#### **3 Years Minimum Term**  
[Epos Now Payments V400m](https://eposnow--test.my.salesforce.com/01t5E000006OYwN?srPos=0&srKp=01t)  - £15 per month (Non-primary Subscription Bundle Product)  
- Bundle of Epos Now Payments App (Free), Payment Integration (to trigger proposition)  
#### **3 Years Minimum Term**  
- Child products for selection of specific rates & ranges (0-3%)  
[Epos Now Payments p400 (0-3%)](https://eposnow--test.my.salesforce.com/01t5E000006OGZj), [Epos Now Payments p400 (3-4%)](https://eposnow--test.my.salesforce.com/01t5E000006OGZt) etc...  

**When invoice is created Epos Now Payments is applied as Bespoke pricing to £0 the Invoice Line to prevent taking payment straight away, Authorisation is ticked to ensure payment details are collected for this £0 charge. Customer will not be charged until KYC is complete.**