Payment premiums are a monthly charge that is applied if the customer does not start trading with a payment provider within the minimum time period after sale. 

There are two processes, one for **Non-ENPayments Proposition Sales** and one for **Epos Now Payments Sales** 

## Proposition Payment Premium 
If a payment integration is sold (NON-ENPayments) and a payment integration  is not setup 60days of the payment date the following will automatically be applied to the account instead
-  Apply a **Support Subscription** if not purchased
- Apply a **Care Pan Subscription** if not purchased 
- Apply the **Payment Premium Subscription** only if already purchased a Care Plan & Support

A Daily scheduler is run to check for the above requirements via **Mass Action: Genereate 60 Day Payment Missing** that runs the following report "**\[Batch]Generate 60 Days Prem (Tenders)**" 
https://eposnow.my.salesforce.com/00O3X00000COwlD

## Epos Now Payments Payment Premium 
We charge a customer the payment premium if they are not using EN payments as agreed after the first 45 days. In a rolling 28 day period, a customer needs to process £2000 through EN Payments, or 75% of their transactions need to go through it otherwise it will fall into the report that generates a payment premium record. 

A daily scheduler is run to check for the abouve requirements cia runs the **Mass ACtion: Generate ENP Payment Premium** that runs the following report "**\[Batch] ENPayments Generate PP 1st Feb**" https://eposnow.my.salesforce.com/00O7T000000FEzd
*This mass action runs apex class **EposNowPaymentsPremium\.cls** to generate or reactivate the related payments premium.*

_Pricing update 1st July 2023:_ 
GBP 29 => GBP 49  
AUD 49 => AUD 69  
CAD 44 => CAD 64  
EUR 35 => EUR 55  
USD 39 => USD 59

## Cancelling the Payment Premium if begin trading again:
If the customer meets the minimum trading criteria then the payment premium will be removed/cancelled. 
A daily scheduler is run to check for the above requirements via runs the **Mass Action: Cancel Payment Premium** that runs the following report "**\[MA] Payment Premiums Since Integrated**" 
https://eposnow.my.salesforce.com/00O3X00000CNgTV

## Bespoke Pricing
We apply bespoke pricing from the "**New Payment Premium Pricing**" product for when our pricing was changed. This is to ensure existing customers prior to this change are not charged with new pricing.

## Customer Emails
Within the first 60/45 days we send the customer reminders to integrate on days **7,12,21,30 via the following mass actions:**
- Unintegrated Payments Email Reminder (Payment Premium)
- Unintegrated Payments Email Reminder (Care Plan)
- Unintegrated Payments Email Reminder (Support)
- (*For ENP all three mass actions trigger the same Payment Premium email alert "ENPayments Urgent Reminder - Payments Premium Surcharge 45 Days \[Multilingual]")

If a EN payments Premium has been generated this following email will be sent informing the customer that the payment premium is starting:
https://eposnow.my.salesforce.com/01Q7T000000CiJo 

## Delaying or preventing Payment Premium
This can be done via a contract flow:
https://eposnow.atlassian.net/servicedesk/customer/portal/5/article/1760985186 