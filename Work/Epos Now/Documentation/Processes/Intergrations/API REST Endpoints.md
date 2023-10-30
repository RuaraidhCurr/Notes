**These are custom endpoints created using REST Apex Classes**
## Apex Classes:
- **RESTAxiumSerials - POST /PutSerials/**
	Endpoint to create or update Assets for Axium Devices. Used in the **Lambda function in the Backoffice** that fetches data from Axium FTP. 
- **RESTCheckSerialNumber - GET /CheckSerial/**
	Endpoint to lookup a serial number and return resellerId and support number. Used in **Backoffice** to display correct support number for reseller
- **RESTCreateHQAccount - POST ** 