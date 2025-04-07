---
icon: lock
---

# OAuth

## Authorization Code Grant

There are three parties involved in the Authorization Code flow -- the client (your application), the server (Ramp) and the user (data owner). The overall flow follows these steps:

1. Your application sends the user to authenticate with Ramp.
2. The user sees the authorization prompt and approves the app’s request for data access.
3. The user is redirected back via a `redirect_uri` with a temporary `authorization_code`.
4. Your application exchanges the `authorization_code` for an `access_token`.
5. Ramp verifies the params and returns an `access_token`.
6. Your application gets a new `access_token` with the `refresh_token`.

<figure><img src="https://docs.ramp.com/assets/oauth_diagram-Cv7n-U3P.png" alt=""><figcaption></figcaption></figure>

## Step 1: User authenticates with Ramp

To start the `Authorization Code` flow, your application must direct the prospective user to authenticate with Ramp and approve access. This is done making a request in your browser to `https://app.ramp.com/v1/authorize`.

An example URL looks like:

```
https://app.ramp.com/v1/authorize?response_type=code&scope=business:read transactions:read&client_id=ramp_id_i8ytO4Ad7W83VRupR1CpP7sPx4hlGQT479uj0xJof&redirect_uri=https://awesomeapp.io/callback&state=meHH_yy2irpl8UYAvv-my
```

The following parameters are expected:

* `response_type` Required — Must be set to `code`.
* `scope` Required — A space-separated list of to be granted to the returned token. The requested token scopes must be a subset (or the set) of the app's allowed scopes. For example, if you only wish to use the token to read transactions, the scope should be set to `transactions:read`.
* `client_id` Required — ID of your application. It can be found in the [developer settings page](https://app.ramp.com/settings/ramp-developer).
* `redirect_uri` Required — The URL where the user should return after granting access. You need to make sure you have added the URI into the `redirect uris` list of your application (through the [developer settings page](https://app.ramp.com/settings/ramp-developer)).
* `state` Required — Allows you to restore the previous state of your application after the user is redirected back. It is helpful to protect against [Cross-Site Request Forgery](https://datatracker.ietf.org/doc/html/rfc6749#section-10.12). More details can be found [here](https://auth0.com/docs/secure/attack-protection/state-parameters).

## Step 2: User is authenticated and prompted to grant data access

Following successful authentication, the user will see the following pop-up asking them to approve the connection to your app.

## Step 3: User is redirected back with `authorization_code`

Once the user clicks the `Allow` button, they will be redirected back to the `redirect_uri` with an `authorization_code` and a `state`.

```
<redirect_uri>?code=<authorization_code>&state=<state>
```

* Retrieve the returned `state` param from the redirect and compare it with the one you stored earlier. If it doesn't match, you may be the target of an attack because this is either a response for an unsolicited request or someone trying to forge the response.
* Retrieve the `authorization_code` and it will be used to exchange for an `access_token` in the next step

## Step 4: Request for an `access_token`

With the `authorization_code` obtained from the previous step, you now can exchange it for an `access_token` by calling [POST /developer/v1/token](https://docs.ramp.com/developer-api/v1/api/authorization). You must include an `Authorization` header containing a base-64 representation of the client credentials, `Authorization: Basic <base64-encoded client_id:client_secret>`.

The request body need to contain the following fields:

* `grant_type` Required — Must be set to `authorization_code`.
* `code` Required — The authorization code obtained from the previous step.
* `redirect_uri` Required — The redirect URI previously used in the authorization request, for validation purposes.

```
curl --location --request POST 'https://api.ramp.com/developer/v1/token' \    --header 'Authorization: Basic Sm1ic1JvUkF0ODZoVGJtRFQ4ZnppczA5SlFIUXlIWmVXYnE1ZHBvY3lhS3UxbGpzOmJHT3I1aGxraGVOVTcxZWpsTEtwalVabDVHc1lhc1F3NE91ZnFWaTdCM1p2UXlkNA==' \    --header 'Content-Type: application/x-www-form-urlencoded' \    --data-urlencode 'grant_type=authorization_code' \    --data-urlencode 'code=wUvTfJmrDiTogz3dRNsUJdBRtSl658ibsH8xLKwKQ2PbttRr' \    --data-urlencode 'redirect_uri=https://www.yourawesomeapp.com'  
```

## Step 5: Obtain an `access_token`

If the token request goes through, Ramp will return a response containing the `access_token`.

```
{    "access_token": "ramp_tok_o4bfbrfhDBdXcjjTBMD1iTTXdprRuHdcmne8gR5zfSV78uPe",    "token_type": "bearer",    "expires_in": 3600,    "refresh_token": "ramp_tok_IwOGYzYTlmM2YxOTQ5MGE3YmNmMDFkNTVk",    "scope": "business:read transactions:read"}
```

### Token security <a href="#r68l" id="r68l"></a>

Access and refresh tokens should never be transmitted or store without encryption. You should consider using a strong encryption algorithm (e.g. AES). You can [revoke individual access and refresh tokens using the API](https://docs.ramp.com/developer-api/v1/api/authorization#post-developer-v1-token-revoke) if the need arises.

## Step 6: Get a new `access_token` using `refresh_token`

Optionally, implement the [Refresh Token Grant↗](https://datatracker.ietf.org/doc/html/rfc6749#section-6) flow. The returned `access_token` has a limited lifespan; you may exchange the `refresh_token` for a new `access_token` without having to ask the user to re-grant access. To do so, add `Refresh Token` to the list of `Grant types` of your application in the [developer settings page↗](https://app.ramp.com/settings/ramp-developer).

To refresh the token, make a `POST` request to the token endpoint `https://api.ramp.com/developer/v1/token` with following fields:

* `grant_type` Required — Must be set to `refresh_token`
* `refresh_token` Required — The `refresh_token` returned from the previous request.

```
curl --location --request POST 'https://api.ramp.com/developer/v1/token' \    --header 'Authorization: Basic Sm1ic1JvUkF0ODZoVGJtRFQ4ZnppczA5SlFIUXlIWmVXYnE1ZHBvY3lhS3UxbGpzOmJHT3I1aGxraGVOVTcxZWpsTEtwalVabDVHc1lhc1F3NE91ZnFWaTdCM1p2UXlkNA==' \    --header 'Content-Type: application/x-www-form-urlencoded' \    --data-urlencode 'grant_type=refresh_token' \    --data-urlencode 'refresh_token=IwOGYzYTlmM2YxOTQ5MGE3YmNmMDFkNTVk' 

```
