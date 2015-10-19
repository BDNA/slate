## MANUFACTURER

### Retrieve manufacturer

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



### Create a proprietary manufacturer

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


### Update an existing manufacturer

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

### Delete an existing manufacturer
