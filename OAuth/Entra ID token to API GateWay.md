# Accept the Entra access token directly at the AWS edge(recommended if you already have the token)
You make API Gateway (HTTP API) or Application Load Balancer (ALB) validate Entra‑issued JWTs:

# Register & expose your API in Entra ID

    Create an App Registration for the API; under Expose an API, set the Application ID URI (e.g., api://{clientId} or a custom URI) and define scopes (e.g., Todo.Read, Todo.Write).
    Clients (Copilot agent) request tokens for that audience and include required scopes.
    Your API will later enforce aud and scp/roles. [How to con...rosoft ...], [Tutorial:...y platform], [Access tok...y platform]

 # API Gateway HTTP API with a JWT Authorizer
    Configure a JWT authorizer with:
    Issuer: https://login.microsoftonline.com/{tenantId}/v2.0 (use tenant‑specific, not common).
    Audience(s): your Application ID URI (api://{clientId} or custom).

 # On each route, set authorizationScopes (e.g., Todo.Read). API Gateway will validate:

  signature (using Entra JWKS),
  iss, aud, exp/nbf/iat,
  and scopes (scp) if you configured them.

Works with RSA-signed JWTs (Entra uses RS256). API Gateway fetches keys from Entra’s JWKS and caches them. [Control ac...PI Gateway], [Where can...rosoft Q&A]

# Control access to HTTP APIs with JWT authorizers in API Gateway
https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api-jwt-authorizer.html
