# Errors

The BDNA Data Platform API uses the following error codes:


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
400 | BadRequest -- Your request is improperly formatted, or lacks required fields
401 | Unauthorized -- Either you do not have the role or the feature or the subscription to the specific content pack information
403 | Forbidden
404 | NotFound -- The specified resource could not be found
405 | MethodNotAllowed -- You tried to access a resource with an invalid method
406 | NotAcceptable -- You requested a format that is not json
408 | RequestTimeout
410 | Gone
500 | InternalServerError
501 | NotImplemented
502 | BadGateway
503 | ServiceUnavailable -- We are temporarially offline for maintenance. Please try again later.
504 | GatewayTimeout
505 | HttpVersionNotSupported
