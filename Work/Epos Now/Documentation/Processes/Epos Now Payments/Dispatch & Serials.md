*[https://eposnow.atlassian.net/browse/PI-406](https://eposnow.atlassian.net/browse/PI-406)  - HQ Endpoint will not be available for MVP*

On dispatch salesforce receives serai lnumbers from out dispatch provider. A trigger will then be fired to check if they are for a payment product and is so trigger a request to the **xxxxx endpoint.**
```JSON
{  
  "serials":[  
    {  
      "SKU":"EN-2439",  
      "SerialNumber":"38967387533",  
      "Product":"p400",  
    }  
  ],  
  "CompanyGuid":"d1a51404-c607-4294-830f-079a9149e93b"  
}
```