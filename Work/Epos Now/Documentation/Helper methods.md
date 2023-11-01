Epos now has a helper class with multiple helper methods in it that can be quickly called on throughout the orgs code base. This helper class is called `HelperClass.cls` and contains many methods

## isSandbox()
	No arguments
a helper class used to determine if the code is running in sandbox or not. 

## getHQDomainUrl()
	NO arguments
This class checks to see if `isSandbox()` returns true for false
If `True`: the code will return the HQ test URL 
If `False`: the code will return the normal HQ url

## getSalesForceDomainUrl()
	No arguments
This class checks to see if `isSandbox()` returns true for false
If `True`: returns the test sandbox org url
if `False`: returns the live Org url

## formatTimeZone(Datetime format, Integet offset)
	`Datetime format, Integet offset`
method returns the correct datetime for the current time offset

## formatTimeZone(Datetime format)
	`Datetime format`
method returns the datetime

## checkDisabled(String s)
	`String s`
checks the System.Labels.**Disabled_Methods** to see if it contains the string `s`

## getCurrencySymbolFromIso(String Iso)
	`String Iso`
Switch statement to return the currency symbol from the currencyisocode

## 