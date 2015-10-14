---
title: BDNA Data Platform API Reference

language_tabs:
  - json
  - http
  - plaintext


toc_footers:
  - <a href='http://github.com/tripit/slate'>Documentation Powered by Slate</a>


search: true
---

# Introduction

Welcome to the BDNA Data Platform API!

BDNA Data Platform APIs are designed to adhere to the REST principles. Our APIs are designed to have predictable, resource-oriented URLs and to use HTTP response codes to indicate API errors. JSON will be returned in all responses from the API.

BDNA Data Platform APIs provide easy interface to access Technopedia Catalog and Content, and Normalize data. These APIs provide full CRUD functionality on Technopedia Catalog and Content and Retrieve functionality on Normalize data.

This version of Data Platform APIs also provide Private Catalog functionality where users can create, update and delete private content to Technopedia. 


# Authentication

The BDNA Data Platform APIs support both LDAP/AD as well as Windows Local User authentication.

The client application needs to first login to the API by using the Login API.

The response from the Login api is an Authorization token that is valid for 24 hours.

For subsequent requests to the resources, this Authorization token needs to be sent in the HTTP Header **Authorization-Token**.

The json payload for the Login api should contain the username and password.

To obtain the Authorization Token, HTTP POST has to be made to

`http://Hostname of the Data Platform Server/bdna-api/api/Login`

with headers:

`Content-Type: application/json`

`Accept: application/json`

The payload in Json format should look like:

{ "UserName": "username", "Password":"Password"}


The above request would result in a response that contains the Authorization Token.  This Authorization-Token should be passed in as a HTTP Header in subsequent requests to the APIs. The Authorization-Token is valid for 24 hours from the time of generation, after which the client application needs to login back into the API using the steps given above to obtain the Authorization-Token.


# Authorization

Authorization is at three levels – at the Role level, at the Feature level and at the Content Pack Subscription level.

If authorization fails at any of the three levels, the user would get a 401 Unauthorized response with a message indicating where the request failed. 

## Role Level
The user should have **User Console Viewer** role to make GET API requests.

The user should have **User Console Editor** role or **Platform Administrator** role for PUT, POST and DELETE API requests. 

## Feature Level
In order to retrieve the Normalize data via APIs, the Data Platform license key shoud have the Normalize feature enabled. Otherwise, the user can only access the Technopedia APIs.

In order to create, update and delete private data, the Data Platform license key should have the Private Catalog feature enabled. 
 

## Content Pack Subscription Level
The Data Platform license key by default enables GET access to Technopedia Catalog APIs.

Access to Technopedia Content Pack APIs depend on the subscription level to Technopedia. 


# Private Catalog

Private Catalog is a new feature in BDNA Data Platform v5.0. With Private Catalog, the user can

1. Update Technopedia public data with custom data. For example, update the price of software
2. Create and manage privately-defined catalog objects
3. Create and track applications that you have developed internally

APIs facilitate the creation of private catalog data. 

# API Response

BDNA Data Platform API uses conventional HTTP response codes to indicate success or failure of an API request. In general, codes in the 2xx range indicate success, codes in the 4xx range indicate an error that resulted from the provided information (e.g. a required parameter was missing, authorization failure, etc.), and codes in the 5xx range indicate an internal error.

When a response comes back, there are two pieces of information to consider:

HTTP Status code - this tells you at a protocol level what the status of the response is

Response Payload - this is the body of the response that provides further details about the response where applicable.

## Response Formats

All responses are returned using JSON. JSON is a lightweight serialization language that is compatible with many different languages. JSON is syntactically correct JavaScript code, meaning you can use the Javascript eval() function to parse it.

## HTTP Status Code

The BDNA Data Platform APIs use standard HTTP status codes to indicate success or failure of API calls.

Error Code | Meaning
---------- | -------
200 | OK
201 | Created -- We would get this response code upon a successful POST
202 | Accepted
204 | NoContent -- We would get this response code upon a success PUT (update)
205 | ResetContent
206 | Partial Content
301 | Moved
302 | Redirect
400 | BadRequest
401 | Unauthorized -- Either you don't have the role or the feature or the subscription to the specific content pack information
403 | Forbidden
404 | NotFound -- The specified resource could not be found
405 | MethodNotAllowed -- You tried to access a resource with an invalid method
406 | NotAcceptable -- You requested a format that isn't json
408 | RequestTimeout
410 | Gone
500 | InternalServerError
501 | NotImplemented
502 | BadGateway
503 | ServiceUnavailable -- We're temporarially offline for maintanance. Please try again later.
504 | GatewayTimeout
505 | HttpVersionNotSupported


# OData
Technopedia APIs are based on OData (Open Data Protocol).

OData is a data access protocol for the web. It is an OASIS standard that defines the best practice for building and consuming RESTful APIs. It provides a uniform way to query and manipulate data sets.

OData RESTful APIs are easy to consume. BDNA Data Platform APIs support Version 4 of the OData protocol.

The OData metadata, a machine-readable description of the data model of the APIs, enables the creation of powerful generic client proxies and tools. 

Please refer to <a href='http://www.odata.org/libraries/'>http://www.odata.org/libraries/</a> for a list of OData libraries available to implement Client applications in .NET, Java, JavaScript, Python, C++, and other platforms. 

# Supported OData Query Options

```plaintext
To retrieve all software products manufacturered by Microsoft, along with the count of the result:

http://servername/bdna-api/odata/CAT_SW_PRODUCT?$filter=CAT_MANUFACTURER/MANUFACTURER eq 'Microsoft' & $count=true

In the above request $filter and $count are called OData system query options. The two query options are seperated by &. 

The system query options and its value should be seperated by = without any spaces before and after the equal like in $filter=CAT_MANUFACTURER/MANUFACTURER eq 'Microsoft'

For example, $filter = CAT_MANUFACTURER/MANUFACTURER eq 'Microsoft' would result in 400 Bad Request error with the message "The query parameter '$filter ' is not supported."

```

The common OData query options supported by Data Platform API Get requests are

$expand, $filter, $select, $count, $orderby, $skip and $top.

All values specified for the OData Query options are case-sensitive.

Following is the evaluation order for the system query options.

Prior to applying any server-driven paging:

* $filter
* $count
* $orderby
* $skip
* $top

After applying any server-driven paging:

* $expand
* $select

The system query options and its value should be seperated by a = without any space before and after the =.

The system query options are seperated by & at the same level. i.e when multiple system query options are specified, they must be seperated by &, unless the system query options are nested using paranthesis.

System query options can be nested.

For example, within $expand system query option, we can nest $filter and $select and another $expand.

The maximum expansion depth varies from one api resource to another, but it is always a good practice to not exceed expansion depth of 2.   


## $expand

```json
GET /odata/CAT_SW_PRODUCT?$top=2&$expand=CAT_MANUFACTURER HTTP/1.1
Host: DataPlatformServerName
Content-Type: application/json
Accept: application/json
Authorization-Token: xxxxxxxxxxxxxxxxxxxxx
Cache-Control: no-cache

Example Request:

http://DataPlatformServer/bdna-api/odata/CAT_SW_PRODUCT?$top=2&$expand=CAT_MANUFACTURER

Example Response:

{
    "@odata.context": "http://DataPlatformServer/bdna-api/odata/$metadata#CAT_SW_PRODUCT",
    "value": [
        {
            "CAT_SW_PRODUCT_ID": 2,
            "CREATE_DATE": "2005-07-11T12:09:23Z",
            "LAST_MODIFIED_DATE": "2011-08-29T17:02:23Z",
            "CAT_MANUFACTURER_ID": 16414,
            "SOFTWARE": "ACCURAT Multipers",
            "FAMILY": null,
            "COMPONENT": null,
            "ALIAS": null,
            "IS_SUITE": null,
            "PLICSABLE": null,
            "NFAMILY": 0,
            "CAT_TAXONOMY2012_ID": 19892759,
            "VENDOR_CATEGORY": null,
            "DESUPFLAG": null,
            "DISCONTINUEDFLAG": null,
            "TO_BE_DELETED": null,
            "TO_BE_DELETED_ON": null,
            "DELETE_REASON": null,
            "REPLACEMENT_ID": null,
            "PRIVATE_FLAG": 0,
            "IS_PRIVATE": "n",
            "PRIVATE_TYPE": "Not Modified",
            "CAT_MANUFACTURER": {
                "CAT_MANUFACTURER_ID": 16414,
                "CREATE_DATE": "2005-05-31T00:23:54Z",
                "LAST_MODIFIED_DATE": "2013-09-30T16:20:07Z",
                "MANUFACTURER": "ACCURAT Informatik",
                "LEGAL": "GmbH",
                "REVENUE": null,
                "SYMBOL": "Private",
                "TIER": 3,
                "WEBSITE": "http://www.accurat.de/",
                "CAT_MANUFACTURER_OWNER_ID": null,
                "DATE_ACQUIRED": null,
                "DESCRIPTION": null,
                "KNOWNAS": null,
                "PHONE": null,
                "FAX": null,
                "CITY": null,
                "COUNTRY": null,
                "EMAIL": null,
                "PUBLICLY_TRADED": null,
                "STREET": null,
                "STATE": null,
                "ZIP": null,
                "REVENUE_DATE": null,
                "FISCAL_ENDDATE": null,
                "PROFITS_PERYEAR": null,
                "PROFITS_DATE": null,
                "EMPLOYEES": null,
                "EMPLOYEES_DATE": null,
                "CAT_INDUSTRYTAG_ID": null,
                "TO_BE_DELETED": null,
                "TO_BE_DELETED_ON": null,
                "DELETE_REASON": null,
                "REPLACEMENT_ID": null,
                "PRIVATE_FLAG": 0,
                "IS_PRIVATE": "n",
                "PRIVATE_TYPE": "Not Modified"
            }
        },
        {
            "CAT_SW_PRODUCT_ID": 5,
            "CREATE_DATE": "2005-08-18T13:44:46Z",
            "LAST_MODIFIED_DATE": "2015-08-27T10:10:48Z",
            "CAT_MANUFACTURER_ID": 4358,
            "SOFTWARE": "Report Designer and SDK",
            "FAMILY": "BIRT",
            "COMPONENT": null,
            "ALIAS": null,
            "IS_SUITE": 0,
            "PLICSABLE": 0,
            "NFAMILY": 1,
            "CAT_TAXONOMY2012_ID": 19892850,
            "VENDOR_CATEGORY": "Application Architecture and Design",
            "DESUPFLAG": null,
            "DISCONTINUEDFLAG": null,
            "TO_BE_DELETED": null,
            "TO_BE_DELETED_ON": null,
            "DELETE_REASON": null,
            "REPLACEMENT_ID": null,
            "PRIVATE_FLAG": 0,
            "IS_PRIVATE": "n",
            "PRIVATE_TYPE": "Not Modified",
            "CAT_MANUFACTURER": {
                "CAT_MANUFACTURER_ID": 4358,
                "CREATE_DATE": "2005-05-31T00:23:54Z",
                "LAST_MODIFIED_DATE": "2014-12-08T09:21:07Z",
                "MANUFACTURER": "Actuate",
                "LEGAL": "Corporation",
                "REVENUE": 139,
                "SYMBOL": "ACTU",
                "TIER": 2,
                "WEBSITE": "http://www.actuate.com/",
                "CAT_MANUFACTURER_OWNER_ID": 7796,
                "DATE_ACQUIRED": "12/5/2014",
                "DESCRIPTION": "Actuate Corporation provides software and services to develop and deploy Rich Internet Applications (RIA) that deliver interactive content. (Google Finance: http://www.google.com/finance?q=Actuate)",
                "KNOWNAS": null,
                "PHONE": "1-888-422-8828",
                "FAX": "1-650-645-3700",
                "CITY": "San Mateo",
                "COUNTRY": "United States",
                "EMAIL": "info@actuate.com",
                "PUBLICLY_TRADED": "yes",
                "STREET": "951 Mariners Island Boulevard",
                "STATE": "California",
                "ZIP": "94404",
                "REVENUE_DATE": "2012-12-31T00:00:00Z",
                "FISCAL_ENDDATE": "2012-12-31T00:00:00Z",
                "PROFITS_PERYEAR": 117,
                "PROFITS_DATE": "2012-12-31T00:00:00Z",
                "EMPLOYEES": 624,
                "EMPLOYEES_DATE": "2013-06-30T00:00:00Z",
                "CAT_INDUSTRYTAG_ID": null,
                "TO_BE_DELETED": null,
                "TO_BE_DELETED_ON": null,
                "DELETE_REASON": null,
                "REPLACEMENT_ID": null,
                "PRIVATE_FLAG": 0,
                "IS_PRIVATE": "n",
                "PRIVATE_TYPE": "Not Modified"
            }
        }
    ]
}

```

The OData $expand system query option indicates the related entities that MUST be represented inline in the response. 

The value of the $expand query option is a comma-separated list of navigation property names, and optionally a parenthesized set of expand options (for filtering, sorting, selecting, paging, or expanding the related entities). The navigation property names are case sensitive. 

*Example*

*To retrieve software products and expand on the related entity CAT_MANUFACTURER*

`http://DataPlatformServer/bdna-api/odata/CAT_SW_PRODUCT?$expand=CAT_MANUFACTURER`

* To retrieve software products and expand on the related entity CAT_MANUFACTURER and only select MANUFACTURER (i.e. name) and TIER from CAT_MANUFACTURER*

`http://DataPlatformServer/bdna-api/odata/CAT_SW_PRODUCT?$top=2&$expand=CAT_MANUFACTURER($select=MANUFACTURER,TIER)`

## $select

The $select system query option requests that the service return only the propertiesy requested by the client. The service returns the specified content, if available, along with any available expanded navigation properties, and MAY return additional information.

The value of the $select query option is a comma-separated list of properties. The property names are case-senstive.

*Example*

*To retrieve software products and display only the SOFTWARE (i.e. name) and FAMILY*

`http://DataPlatformServer/bdna-api/odata/CAT_SW_PRODUCT?$select=SOFTWARE,FAMILY`


## $filter

```json
GET /odata/CAT_SW_PRODUCT?$filter=startswith(SOFTWARE, 'Office')&$count=true&$top=2 HTTP/1.1
Host: DataPlatformServer
Content-Type: application/json
Accept: application/json
Authorization-Token: xxxxxxxxxxx
Cache-Control: no-cache

{
    "@odata.context": "http://DataPlatformServer/bdna-api/odata/$metadata#CAT_SW_PRODUCT",
    "@odata.count": 208,
    "value": [
        {
            "CAT_SW_PRODUCT_ID": 393816,
            "CREATE_DATE": "2005-08-31T11:27:18Z",
            "LAST_MODIFIED_DATE": "2011-07-29T15:18:45Z",
            "CAT_MANUFACTURER_ID": 6451,
            "SOFTWARE": "Office Server",
            "FAMILY": "Compaq",
            "COMPONENT": null,
            "ALIAS": null,
            "IS_SUITE": 0,
            "PLICSABLE": 1,
            "NFAMILY": 1,
            "CAT_TAXONOMY2012_ID": 19889424,
            "VENDOR_CATEGORY": "OpenVMS Systems | Commercial software",
            "DESUPFLAG": null,
            "DISCONTINUEDFLAG": null,
            "TO_BE_DELETED": null,
            "TO_BE_DELETED_ON": null,
            "DELETE_REASON": null,
            "REPLACEMENT_ID": null,
            "PRIVATE_FLAG": 0,
            "IS_PRIVATE": "n",
            "PRIVATE_TYPE": "Not Modified"
        },
        {
            "CAT_SW_PRODUCT_ID": 393817,
            "CREATE_DATE": "2005-08-31T11:26:45Z",
            "LAST_MODIFIED_DATE": "2011-07-29T15:18:45Z",
            "CAT_MANUFACTURER_ID": 6451,
            "SOFTWARE": "Office Server Web Interface (COSWI)",
            "FAMILY": "Compaq",
            "COMPONENT": null,
            "ALIAS": null,
            "IS_SUITE": 0,
            "PLICSABLE": 1,
            "NFAMILY": 1,
            "CAT_TAXONOMY2012_ID": 19889424,
            "VENDOR_CATEGORY": "OpenVMS Systems | Commercial software",
            "DESUPFLAG": null,
            "DISCONTINUEDFLAG": null,
            "TO_BE_DELETED": null,
            "TO_BE_DELETED_ON": null,
            "DELETE_REASON": null,
            "REPLACEMENT_ID": null,
            "PRIVATE_FLAG": 0,
            "IS_PRIVATE": "n",
            "PRIVATE_TYPE": "Not Modified"
        }
    ]
}

```

The OData $filter system query option restricts the set of items returned.

*Example*

*To retrieve software products where product name contains 'office'*

`http://DataPlatformServer/bdna-api/odata/CAT_SW_PRODUCT?$filter=startswith(SOFTWARE, 'Office')&$count=true&$top=2`


## $count

The OData $count system query option with a value of true specifies that the total count of items within a collection matching the request be returned along with the result.


## $skip

The OData $skip system query option specifies a non-negative integer n that excludes the first n items of the queried collection from the result. The service returns items starting at position n+1.


## $top

The OData $top system query option specifies a non-negative integer n that limits the number of items returned from a collection. The service returns the number of available items up to but not greater than the specified value n.


# API Naming Convention

All API resource urls are case sensitive. The APIs are named after the corresponding object name in the Publish database.

## CAT_P

API whose name starts with CAT_P_ indicates it's a Private Catalog API (i.e. Catalog Private). These APIs allow user to POST, PUT and DELETE. The CAT_P objects in the database contain only the customer provided private data. 


## CAT_T

API whose name starts with CAT_T_ indicates it's a Technopedia Catalog API (i.e. Catalog Technopedia). These APIs allow only GET requests. The data in CAT_T objects in the database is kept up-to-date with BDNA's Technopedia Catalog with the Technopedia Sync feature.  

## CAT_

API whose name just starts with CAT_ (indicates Catalog) is a combination of Technopedia Catalog (i.e. CAT_T) and Private Catalog (i.e. CAT_P).


# Methods

## GET

## POST

## PUT

## DELETE

# Pagination

To help control the quantity of response data returned in a call, we support the Odata system query options $top and $skip.

Responses that include only a partial set of the items identified by the request URL MUST contain a link that allows retrieving the next partial set of items. This link is called a next link; The final partial set of items MUST NOT contain a next link.


# Delta 

# Technopedia APIs

# MANUFACTURER

## Retrieve manufacturer

```json
Example Request:
To retrieve manufacturers that are only in Private Catalog
http://DataPlatformServer/bdna-api/odata/CAT_MANUFACTURER?$filter=IS_PRIVATE eq 'Y'&$top=1

Example Response:
{
    "@odata.context": "http://localhost:2023/odata/$metadata#CAT_MANUFACTURER",
    "value": [
        {
            "CAT_MANUFACTURER_ID": -160,
            "CREATE_DATE": "2015-09-28T01:00:18Z",
            "LAST_MODIFIED_DATE": "2015-09-28T01:01:36Z",
            "MANUFACTURER": "Test Mfr1",
            "LEGAL": null,
            "REVENUE": null,
            "SYMBOL": null,
            "TIER": null,
            "WEBSITE": null,
            "CAT_MANUFACTURER_OWNER_ID": null,
            "DATE_ACQUIRED": null,
            "DESCRIPTION": null,
            "KNOWNAS": null,
            "PHONE": null,
            "FAX": null,
            "CITY": null,
            "COUNTRY": null,
            "EMAIL": null,
            "PUBLICLY_TRADED": null,
            "STREET": null,
            "STATE": null,
            "ZIP": null,
            "REVENUE_DATE": null,
            "FISCAL_ENDDATE": null,
            "PROFITS_PERYEAR": null,
            "PROFITS_DATE": null,
            "EMPLOYEES": null,
            "EMPLOYEES_DATE": null,
            "CAT_INDUSTRYTAG_ID": null,
            "TO_BE_DELETED": null,
            "TO_BE_DELETED_ON": null,
            "DELETE_REASON": null,
            "REPLACEMENT_ID": null,
            "PRIVATE_FLAG": 2,
            "IS_PRIVATE": "Y",
            "PRIVATE_TYPE": "Proprietary entry"
        }
    ]
}
```

To retrieve manufacturers from both Technopedia Catalog and Private Catalog, the API to use is CAT_MANUFACTURER.

`http://DataPlatformServer/bdna-api/odata/CAT_MANUFACTURER`

To retrieve manufacturers that are only in Private Catalog, the query would be:

`http://DataPlatformServer/bdna-api/odata/CAT_MANUFACTURER?$filter=IS_PRIVATE eq 'Y'`

To retrieve manufacturers that are only in Technopedia Catalog and which have not been edited with private values, the query would be:

`http://DataPlatformServer/bdna-api/odata/CAT_MANUFACTURER?$filter=IS_PRIVATE eq 'N'`



## Create a proprietary manufacturer

```json
POST /odata/CAT_P_MANUFACTURER HTTP/1.1
Host: localhost:2023
Content-Type: application/json
Accept: application/json
Authorization-Token: 6MxfPcZ56Hu/ZMBGeloioLMqZOHiqoF2eOQ+FnBD23A9/vGJDnS+O/2FiSZwLAeNrXTV/azD84vaDM+9/TcNkLA11CwLlZplvAk8aSAW50EKqXGiU6Lii/P7u/zQnO2y
Cache-Control: no-cache

{"MANUFACTURER":"Private Manufacturer1", "TIER":2, "WEBSITE":"http://foo.com"}

Example Response:
Response Status: 201

{
    "@odata.context": "http://localhost:2023/odata/$metadata#CAT_P_MANUFACTURER/$entity",
    "CAT_MANUFACTURER_ID": -200,
    "CREATE_DATE": "2015-10-14T13:03:41Z",
    "LAST_MODIFIED_DATE": "2015-10-14T13:03:41Z",
    "MANUFACTURER": "Private Manufacturer1",
    "LEGAL": null,
    "REVENUE": null,
    "SYMBOL": null,
    "TIER": 2,
    "WEBSITE": "http://foo.com",
    "CAT_MANUFACTURER_OWNER_ID": null,
    "DATE_ACQUIRED": null,
    "DESCRIPTION": null,
    "KNOWNAS": null,
    "PHONE": null,
    "FAX": null,
    "CITY": null,
    "COUNTRY": null,
    "EMAIL": null,
    "PUBLICLY_TRADED": null,
    "STREET": null,
    "STATE": null,
    "ZIP": null,
    "REVENUE_DATE": null,
    "FISCAL_ENDDATE": null,
    "PROFITS_PERYEAR": null,
    "PROFITS_DATE": null,
    "EMPLOYEES": null,
    "EMPLOYEES_DATE": null,
    "CAT_INDUSTRYTAG_ID": null,
    "TO_BE_DELETED": null,
    "TO_BE_DELETED_ON": null,
    "DELETE_REASON": null,
    "REPLACEMENT_ID": null,
    "PRIVATE_FLAG": 2,
    "IS_PRIVATE": "Y",
    "PRIVATE_TYPE": "Proprietary entry"
}
```
POST http method is used to create proprietary manufacturers. All proprietary objects are part of Private Catalog.

Private Catalog object resources are assigned negative numbers for id fields.

Upon successful creation, HTTP status code 201 is returned. 

If the POST fails with 400 Bad Request and additional error message, it could be because of the following reasons:

* The user is trying to create a duplicate manufacturer

* Some required columns have missing values

* Incorrect column names or incompatible values for the column datatype

* Incorrect JSON format

The actual reason is indicated in the error message in the json response. 


## Update an existing manufacturer

```json
PUT /odata/CAT_P_MANUFACTURER(-200) HTTP/1.1
Host: localhost:2023
Content-Type: application/json
Accept: application/json
Authorization-Token: 6MxfPcZ56Hu/ZMBGeloioLMqZOHiqoF2eOQ+FnBD23A9/vGJDnS+O/2FiSZwLAeNrXTV/azD84vaDM+9/TcNkLA11CwLlZplvAk8aSAW50EKqXGiU6Lii/P7u/zQnO2y
Cache-Control: no-cache

{"CAT_MANUFACTURER_ID":-200, "WEBSITE":"http://foo.org", "PUBLICLY_TRADED":"Y"}

For successful update, response is 204 No Content

Now, if we query for this manufacturer,

http://DataPlatformServer/bdna-api/odata/CAT_MANUFACTURER(-200),

the response would be:

{
    "@odata.context": "http://localhost:2023/odata/$metadata#CAT_MANUFACTURER/$entity",
    "CAT_MANUFACTURER_ID": -200,
    "CREATE_DATE": "2015-10-14T13:03:41Z",
    "LAST_MODIFIED_DATE": "2015-10-14T13:08:07Z",
    "MANUFACTURER": "Private Manufacturer1",
    "LEGAL": null,
    "REVENUE": null,
    "SYMBOL": null,
    "TIER": 2,
    "WEBSITE": "http://foo.org",
    "CAT_MANUFACTURER_OWNER_ID": null,
    "DATE_ACQUIRED": null,
    "DESCRIPTION": null,
    "KNOWNAS": null,
    "PHONE": null,
    "FAX": null,
    "CITY": null,
    "COUNTRY": null,
    "EMAIL": null,
    "PUBLICLY_TRADED": "Y",
    "STREET": null,
    "STATE": null,
    "ZIP": null,
    "REVENUE_DATE": null,
    "FISCAL_ENDDATE": null,
    "PROFITS_PERYEAR": null,
    "PROFITS_DATE": null,
    "EMPLOYEES": null,
    "EMPLOYEES_DATE": null,
    "CAT_INDUSTRYTAG_ID": null,
    "TO_BE_DELETED": null,
    "TO_BE_DELETED_ON": null,
    "DELETE_REASON": null,
    "REPLACEMENT_ID": null,
    "PRIVATE_FLAG": 2,
    "IS_PRIVATE": "Y",
    "PRIVATE_TYPE": "Proprietary entry"
}
```

## Delete an existing manufacturer

# Normalize APIs

# Known issues