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
