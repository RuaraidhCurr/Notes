**Processes and communications when a Payment has failed to be collected**

See **Contracts** for internal collection flows used by the 'Customer Success' team to collect failed payments.

### Till lock out:
Whenever the primary subscription is resynced, the expiry in the backoffice is set to  

1. The temporary access date if set and in the future
2. Yesterday if there are any non-app failed payments over 7 days due
3. otherwise if there is no end date its set 10 years in the future,
4. If there is an end date set on the primary its set to that

 in the order listed here above. If the expiry date is at any point in the past it shows the reactivation screen
## Customer Email Cadences
*(number of occurrences within a given time period)*


Sent via a series of workflows on Payment transaction. Reset day 1, day 2, day 3, day 4, day 5, day 6