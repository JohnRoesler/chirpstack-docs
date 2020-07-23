---
description: Information on API tokens for authentication and authorization.
---

# Authentication / authorization

Both the gRPC and the JSON REST interface are protected by an authentication
and authorization mechanism. For this [JSON web-tokens](https://jwt.io) are
used, using the `--jwt-secret` / `JWT_SECRET` value for signing. Therefore
it is important to choose an unique and strong secret.

To generate a random secret, you could use the following command:

```bash
openssl rand -base64 32
```

To create a claim, follow the JWT standard to create a claim and sign it with your secret. 

Example claim:

```json
{
	"iss": "chirpstack-application-server", // issuer of the claim
	"aud": "chirpstack-application-server", // audience for which the claim is intended
	"nbf": 1489566958,                  // unix time from which the token is valid
	"exp": 1489653358,                  // unix time when the token expires
	"sub": "user",                      // subject of the claim (a user)
	"username": "admin"                 // username the client claims to be
}
```

## Setting the authentication token

### gRPC

When using [gRPC](http://grpc.io/), the JWT token needs to be stored in the
`authorization` key of the request metadata. For example in Go, this can be
done by the [grpc.WithPerRPCCredentials](https://godoc.org/google.golang.org/grpc#WithPerRPCCredentials)
method.

### REST API

For requests to the RESTful JSON interface, you need to set the JWT token
using the `Grpc-Metadata-Authorization` header field. The token needs to
be present for each request. For example as a cURL header `--header "Grpc-Metadata-Authorization: Bearer <jwt token>"`