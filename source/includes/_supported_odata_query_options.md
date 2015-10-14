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

