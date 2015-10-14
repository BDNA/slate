# Authorization

Authorization is at three levels – at the Role level, at the Feature level and at the Content Pack Subscription level.

If authorization fails at any of the three levels, the user would get a 401 Unauthorized response with a message indicating where the request failed. 

## Role Level
The user should have TECHNOPEDIA_VIEWER role to make GET API requests.

The user should have TECHNOPEDIA_EDITOR or TECHNOPEDIA_ADMINISTRATOR role for PUT, POST and DELETE API requests. 

## Feature Level
In order to retrieve the Normalize data via APIs, the Data Platform license key shoud have the Normalize feature enabled. Otherwise, the user can only access the Technopedia APIs.

In order to create, update and delete private data, the Data Platform license key should have the Private Catalog feature enabled. 
 

## Content Pack Subscription Level
The Data Platform license key by default enables GET access to Technopedia Catalog APIs.

Access to Technopedia Content Pack APIs depend on the subscription level to Technopedia. 
