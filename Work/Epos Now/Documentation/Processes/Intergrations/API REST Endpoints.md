**These are custom endpoints created using REST Apex Classes**
## Apex Classes:
- **RESTAxiumSerials - POST /PutSerials/**
	Endpoint to create or update Assets for Axium Devices. Used in the **Lambda function in the Backoffice** that fetches data from Axium FTP. 
- **RESTCheckSerialNumber - GET /CheckSerial/**
	Endpoint to lookup a serial number and return resellerId and support number. Used in **Backoffice** to display correct support number for reseller
- **RESTCreateHQAccount - POST /HQAccount/** 
	Endpoint to creatae a backofffice account such as free trials. USed in **Free Trial pages, Test Account Injectors, BAckoffice Setup Wizard (e.g POSToGo, MePOS)**
- **RESTCreateResellerAccount - POST /ResellerAcccount/**
	Endpoiint used to create a reseller account and add an order to the account. Used in **Third party Sellers** Document: [https://docs.google.com/document/d/1hQIPFWFIzz4elhDAQxR1h1UL4QpdMdcymgt9XxUe_UA/edit#heading=h.deda5aq75ag](https://docs.google.com/document/d/1hQIPFWFIzz4elhDAQxR1h1UL4QpdMdcymgt9XxUe_UA/edit#heading=h.deda5aq75ag))
- **RESTGetAppStoreDetails - POST /AppStore/**
	Emdpoint for the old app store, to instal an app, get list of payment methods & upgrade plans. Used in **BAckoffice - Old App Store & Admin Install App Page** (Document: [https://docs.google.com/document/d/1CtIEI9_MwrulLAEZxyocIUzQk9ZO6KNGzzuz4siM1hk/edit?usp=sharing])
- **RESTGetAvailableReferralPartners (Not Used)**  
- **RESTGoogleAdsWebhook (Marketing)**  
- **RESTPartnerConversion (Partnerships)**  
- **RESTPaymentMessageCtrl  (Legacy Cloudsocius Endpoint - Not used)**
- **RESTPutAppUpdate (App Details Webhook)**
- **RESTPutBackofficeData (Last Activity)**  
- **RESTPutHardwareOrder (Old app store KDS order)**  
- **RESTPutOrderDetails (UK Dispatch Johnstons)**  
- **RESTPutPaymentStatus (Epos Now Payments)**  
- **RESTPutPaymentsCtrl  (Legacy Cloudsocius Endpoint - Not used)**  
- **RESTPutSubLogins (App Store Sub Login Details)**  
- **RESTSendPartnerReferral (Partnerships)**  
- **RESTSubmitLead (Marketing)**  
- **RESTWalkMeData (Walk Me Backoffice)**  
- **RESTWebOrder (Used internally, not called from Site)**  
- **RESTYouLendWebhooks (Partnerships YouLend)**  
- **RestLocationBilling (Used by software to query customer info from the HQ Data Object)**