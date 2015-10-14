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
