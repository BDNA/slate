# API Naming Convention

All API resource urls are case sensitive. The APIs are named after the corresponding object name in the Publish database.

## CAT_P

API whose name starts with CAT_P_ indicates it's a Private Catalog API (i.e. Catalog Private). These APIs allow user to POST, PUT and DELETE. The CAT_P objects in the database contain only the customer provided private data. 


## CAT_T

API whose name starts with CAT_T_ indicates it's a Technopedia Catalog API (i.e. Catalog Technopedia). These APIs allow only GET requests. The data in CAT_T objects in the database is kept up-to-date with BDNA's Technopedia Catalog with the Technopedia Sync feature.  

## CAT_

API whose name just starts with CAT_ (indicates Catalog) is a combination of Technopedia Catalog (i.e. CAT_T) and Private Catalog (i.e. CAT_P).
