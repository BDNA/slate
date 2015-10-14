# Pagination

To help control the quantity of response data returned in a call, we support the Odata system query options $top and $skip.

Responses that include only a partial set of the items identified by the request URL MUST contain a link that allows retrieving the next partial set of items. This link is called a next link; The final partial set of items MUST NOT contain a next link.
