# kHilltopConnector  

## What is this  
A python module to fetch environmental data from NZ regional councils and others over internet.  

### Hilltop
Hilltop is the software/database that majority of the regional councils in New Zealand use to store the environmental information. This is a robust setup and has a web api.  

### Native Hilltop library
The native Hilltop library _module_ access the database directly and is quite fast. However, running an online app, demands the system to be in the same network. The system also does have an web API, when made available is quite robust.  
The LAWA website is the best example of external applications retrieving the datasets. This module uses similar framework and is relatively lean on the requirements.  

### Existing state of the art  
please see following repository, which is extensive  
https://github.com/mullenkamp/hilltop-py  

### kHilltopConnector Module
This is the module that would fetch information and the there is a list of regional council apis,and can be called into your existing code without much effort.  
Please **note** that this system is **slow**, as a single server has to deliver humungous datasets to multiple requests. Cache is built into the module to cater for the responsiveness, however, it is advised to avoid large data requests passed through this module.  
  
### Available functions  
1. **kHilltopConnector()**  
  **Returns**	: Object with below listed functions  
  **Note**		: _Relatively slow over internet_  
	+ **Arguments**
	1. **apiUrl**	- _default_ HBRC  
		Needs url with hts endpoint & there are a set of preloaded keys available  
	2. onCache	- _default_ True  
		Helps reducing overload on data server  
	3. refreshInterval	- _default_ 900  
		**units are in seconds**  
	4. enableDebug	- _default_ False  
		Only for debugging purposes  
	5. minimalist	- _default_ False  
		if True would not prefetch any information.  
	+ **Available variables**
	1. measurementsList	- Measurements available through selected server - list || static
	2. selectMeasurement	- user selection, should be from above list
	3. siteList		- Available site for above select measurement - list || dynamic
	4. selectSite		- user selection, should be from above list
	5. selectSiteLocation	- array of current site lat,long values  
	**Note**		: _The above 3 current operating variables are available upon calling listed functions_   
	
2. **kHilltopConnector.kHilltopConnector().fetchData()**  
  **Returns**	: Get the time series data  
	**Note** : _all parameters are required if minimalist is True during initialisation_  
	Calling the function without arguments is ok if selectSite and selectMeasurement are assigned before hand
	1. Site				- _default_ None  
		The site can be selected from list returned by above function (3)
	2. myEndDate		- _default_ last observation  
		The format is a string in YYYY-MM-DD  
	3. myStartDate		- _default_ first observation  
		Same format as above  
	4. measurement 		- _default_ None  
		The selection from previous function (2) is used.
		However, this function can be forced to fetch data for a different site, provided such measurement exits for the requested site
	5. daily			- _default_ True  
		If set False, subdaily data sets are fetched
	6. scaleFactor		- _default_ 1  
		This is a division factor on fetched time series
	
5. **kHilltopConnector.kHilltopConnector().clobberCache()**  
	**Note** : Flushes the cache and will be unsuccessfull if file is in use, try manual deletion when required  
  
## Usage
### Python  
import kHilltopConnector as kHK  
kHTop = kHK.kHilltopConnector(apiUrl='HBRC')  
mList = kHTop.measurementsList  
kHTop.selectMeasurement = mList[0] #select relevant array position for measurement of interest  
sList = kHTop.siteList  
kHTop.selectSite = sList[178] #select proper array position for your site  
print(kHTop.selectSiteLocation)  
print(kHTop.selectSiteMeasurementEndTime)  
print((kHTop.fetchData()).head())  
  
##### search for unitTests.ipynb for more info  

### R  
**note** : Please use R Studio if possible  
library(reticulate)  
#point to the right version of python (x32 bit or x64 bits, reticulate is super keen on it  
use_python("C:\\Users\\karunakar.kintada\\AppData\\Local\\Programs\\Python\\Python39\\python.exe", required=TRUE)  
Sys.which("python")  
  
kHTop <- import("kHilltopConnector")  
kHTop1 <- kHTop$kHilltopConnector(apiUrl = 'HBRC')  
mList <- kHTop1$measurementsList  
print(mList)  
...
