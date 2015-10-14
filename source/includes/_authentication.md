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
