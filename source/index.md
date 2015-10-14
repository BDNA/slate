---
title: BDNA Data Platform API Reference

language_tabs:
  - shell


toc_footers:
  - <a href='http://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - errors

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
The user should have TECHNOPEDIA_VIEWER role to make GET API requests.

The user should have TECHNOPEDIA_EDITOR or TECHNOPEDIA_ADMINISTRATOR role for PUT, POST and DELETE API requests. 

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

# Common OData Query Options

```shell
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


## $expand

The OData $expand system query option indicates the related entities that MUST be represented inline in the response. 

The value of the $expand query option is a comma-separated list of navigation property names, and optionally a parenthesized set of expand options (for filtering, sorting, selecting, paging, or expanding the related entities). The navigation property names are case sensitive. 


## $select

The $select system query option requests that the service return only the propertiesy requested by the client. The service returns the specified content, if available, along with any available expanded navigation properties, and MAY return additional information.

The value of the $select query option is a comma-separated list of properties. The property names are case-senstive. 

## Filter
The OData $filter system query option restricts the set of items returned.

## Count
The OData $count system query option with a value of true specifies that the total count of items within a collection matching the request be returned along with the result.

## Skip
The OData $skip system query option specifies a non-negative integer n that excludes the first n items of the queried collection from the result. The service returns items starting at position n+1.

## Top
The OData $top system query option specifies a non-negative integer n that limits the number of items returned from a collection. The service returns the number of available items up to but not greater than the specified value n.

# Methods

## GET

### Expand

### Filter

## POST

## PUT

## DELETE

# Pagination

# Delta 

# APIs

## Technopedia APIs

## Normalize APIs

## Known issues