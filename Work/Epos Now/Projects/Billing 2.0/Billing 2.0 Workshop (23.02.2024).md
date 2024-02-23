### General Meeting Notes
- refactor the code so that it does the recalculations in the background 
- Move the delivery charge calculation to the basket checkout button rather than on the plan selection process?  
- Basket scrollable with a fixed height
- We need prices on the Plans and Addons during the selection process.
- On the confirm details screen include the billing and shipping details pre-populated if possible
- Have the plan & addon selection process on the account page? (make it load a pre-existing opp or create a new one if one isn't already linked)
- Create a new plan object. Used to rollup subs. Primary sub linked to the plan ? 
- Plan record that contains every sub under that plan, and is the rollup summary of the said subs (by default the min plan with always be one location 1 device, If I buy another device)
### Plan Object Notes
- Salesforce account can only have one active plan, it will be used to roll up all 
### Checkout Notes 
Upset items as they are selected (add them to a queue so that it reduces on loading screen), then on the config once details are submitted this will then add the updated OppLineItem to another queue to then update the OppLineItemwith the chosen configurations.