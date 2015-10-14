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

Welcome to the BDNA Data Platform API! Technopedia APIs are now integrated with BDNA Data Platform Version 5.0. These APIs offer easy access to the Technopedia Catalog and Content and Normalize data.

Technopedia APIs are designed to adhere to the REST principles and provide the interface to access Technopedia and Normalize data.
These APIs provide full CRUD functionality on Technopedia Catalog and Content and Retrieve functionality on Normalize data.


# Authentication

The BDNA Data Platform APIs support both LDAP/AD as well as Windows Local User authentication.
The client application needs to first login to the API by using the Login API. The json payload for the Login api should contain the username and password. The response from the Login api is an Authorization token that is valid for 24 hours. For subsequent requests to the resources, this Authorization token needs to be sent in the HTTP Header “Authorization-Token”.

To obtain the Authorization Token, HTTP POST has to be made to

http://<DataPlatformHostName>/bdna-api/api/Login

with Headers:

Content-Type: application/json

Accept: application/json

Json Payload:

{ "UserName": "username", "Password":"Password"}


The above request would result in a response that contains the Authorization Token.  This Authorization-Token should be passed in as a HTTP Header in subsequent requests to the APIs. The Authorization-Token is valid for 24 hours from the time of generation, after which the client application needs to login back into the API using the steps given above to obtain the Authorization-Token.


# Authorization

Authorization is at three levels – at the Role level, at the Feature level and at the Content Pack Subscription level.  

## Role Level
To make GET requests, the user should have TECHNOPEDIA_VIEWER role. 
For PUT, POST and DELETE, the user should have TECHNOPEDIA_EDITOR or TECHNOPEDIA_ADMINISTRATOR role.

## Feature Level
The Data Platform License Key should have Normalize feature enabled in order to access the Normalize data via API. 

## Content Pack Subscription Level
The Data Platform License Key by default enables access to Technopedia Catalog APIs.  API access to Technopedia Content Pack APIs depend on the subscription level to Technopedia. 


# Private Catalog

# Response Format

# Errors

# OData
Technopedia APIs are based on OData (Open Data Protocol). OData is a data access protocol for the web. It is an OASIS standard that defines the best practice for building and consuming RESTful APIs. It provides a uniform way to query and manipulate data sets. OData RESTful APIs are easy to consume. Technopedia APIs support Version 4 of the OData protocol.

The OData metadata, a machine-readable description of the data model of the APIs, enables the creation of powerful generic client proxies and tools. 

Please refer to <a href='http://www.odata.org/libraries/'>http://www.odata.org/libraries/</a> for a list of OData libraries available to implement Client applications in .NET, Java, JavaScript, Python, C++,
and other platforms. 


# Methods

## GET

## POST

## PUT

## DELETE

# Pagination

# APIs

## Technopedia APIs

## Normalize APIs