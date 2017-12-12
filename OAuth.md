# CareerBuilder OAuth 2.0 Instructions

Contents

- [Overview](#overview)
- [Obtaining OAuth2 credentials](#obtaining-oAuth2-credentials)
- [Constructing a Valid JWT](#constructing-a-valid-jwt)
- [Using the JWT to Obtain an Access Token](#using-the-jwt-to-obtain-an-access-token)
- [Calling CareerBuilder APIs using an Access Token](#calling-careerbuilder-apis-using-an-access-token)
- [More Information](#more-information)

## Overview

In order to facilitate external customers accessing resources(Services) owned by CareerBuilder, we have implemented the OAuth 2.0 Client Credentials flow.  To increase the security of this flow, we have augmented it with JWT-Bearer Client Assertions.  In this augmented flow, you are given a shared secret and use it to sign a JSON Web Token(JWT) that is used to validate your client.  This JWT contains your client id, the token endpoint URL, and an expiration.  As a result of sending a valid JWT, you will receive an access token for our OAuth 2.0 protected APIs.

This document will describe the process of obtaining OAuth 2.0 client credentials, using those credentials to generate a JWT and obtain an access token, and finally, using that token to interact with OAuth 2.0 protected CareerBuilder APIs.

## Obtaining OAuth2 credentials

Partners need to request client credentials for their application from CareerBuilder. This is a pair of strings, `client_id` and `client_secret`. The `client_secret` should be stored securely and treated as sensitive information. For ease of tracking, we recommend that consumers of CareerBuilder APIs request a new set of credentials for each distinct application.

`client_id`

`client_secret`

Client credentials are generated for a specific API environment. The three environments for CareerBuilder APIs are Staging, Production US, and Production EU. When requesting credentials, you should also specify the environment(s) in which you need them.

## Constructing a Valid JWT

Once OAuth2 credentials have been obtained, the user may use them to generate a JWT for obtaining OAuth access tokens.

The JWT must contain the following JSON document format:  

```
{ 
  iss: "<client_id>",
  sub: "<client_id>",
  aud: "<token_url>",
  exp: Time.now + 30s
}
```

Where `exp` is the expiration date of the JWT. The JWT is short-lived and should only last for 30-60 seconds.  The value is in epoch time.    

For example:
```
{
  iss: "abc12345", 
  sub: "abc12345", 
  aud: "https://api.careerbuilder.com/oauth/token", 
  exp: 1473378085
}
```

After encoding the JWT, you should get something similar to the following:

`eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJhYmMxMjMiLCJzdWIiOiJhYmMxMjMiLCJhdWQiOiJodHRwczovL3d3d3Rlc3QuYXBpLmNhcmVlcmJ1aWxkZXIuY29tL29hdXRoL3Rva2VuIiwiZXhwIjozMDB9.c1mdrM-PV6SOBcdajRqhQPal5qa_m00Lkw9dy6ZCU2o`

Note: The JWT must be signed with the `client_secret` issued by Careerbuilder using a HS512 algorithm.  

For example, in Ruby using the jwt gem:

`JWT.encode(json_document, client_secret, 'HS512')`

## Using the JWT to Obtain an Access Token

After constructing a valid JWT, your application can issue a POST request (x-www-form-urlencoded) with this JWT to get an access token from the authorization server. The authorization server's URL is: `https://api.careerbuilder.com/oauth/token`.

Request body format:

`grant_type=client_credentials&client_assertion_type:urn:ietf:params:oauth:client-assertion-type:jwt-bearer&client_id=<client_id>&client_assertation=<jwt_signed_with_client_secret>`

Example request:

`curl -X POST -H "Cache-Control: no-cache" -H "Content-Type: application/x-www-form-urlencoded" -d 'client_assertion_type=urn:ietf:params:oauth:client-assertion-type:jwt-bearer&grant_type=client_credentials&client_id=abc12345&client_assertion=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJhYmMxMjMiLCJzdWIiOiJhYmMxMjMiLCJhdWQiOiJodHRwczovL3d3d3Rlc3QuYXBpLmNhcmVlcmJ1aWxkZXIuY29tL29hdXRoL3Rva2VuIiwiZXhwIjozMDB9.c1mdrM-PV6SOBcdajRqhQPal5qa_m00Lkw9dy6ZCU2o' "https://wwwtest.api.careerbuilder.com/oauth/token"` 

Example response:

```
{ 
  "data": { 
    "access_token": "tabc1234567xyz", 
    "token_type": "bearer", 
    "expires_in": "2100" 
  }
}
```

## Calling CareerBuilder APIs using an Access Token

Once you have successfully obtained an access token, your application may use the token to call CareerBuilder web APIs. The access token must be added as a value in the Authorization header as follows:

`Authorization: Bearer access_token` 

For example:

`curl -X GET -H "Authorization: Bearer tabc1234567xyz" "https://wwwtest.api.careerbuilder.com/[API-URI]"`

Note: Only request a new bearer token when your current token has expired (or is near expiration). Requesting a new token for each request you perform will result in your account being flagged for token generation abuse and being locked out.

## More Information

For more information on OAuth 2.0, the Assertion Flow, the JWT-Bearer Assertion Flow, or JWTs, please see the following IETF specifications and drafts: 

OAuth 2.0: http://tools.ietf.org/html/rfc6749  

OAuth 2.0 Bearer Token: http://tools.ietf.org/html/rfc6750  

JWT-Bearer Client Assertion: http://tools.ietf.org/html/draft-ietf-oauth-jwt-bearer-06 

JWT: http://tools.ietf.org/html/draft-ietf-oauth-json-web-token-12 

JWS: http://tools.ietf.org/html/draft-ietf-jose-json-web-signature-17 