---
icon: backward-step
---

# Quick Start

Follow this guide to setup authorization and make your first request to the API.

## Step 1: Access a Ramp Account

[Permanent link: Step 1: Access a Ramp Account](https://docs.ramp.com/developer-api/v1/guides/getting-started#step-1-access-a-ramp-account)

Before you can begin using the API, you’ll need access to a Ramp account. The Ramp API is available in two environments: **Sandbox** and **Production**. To switch between environments, modify the host URL of your API requests.

| Environment | Host                                                     | Description                                                                                                                                                                                               |
| ----------- | -------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Sandbox     | [https://demo-api.ramp.com↗](https://demo-api.ramp.com/) | A Sandbox environment is available upon request and includes robust data for comprehensive testing. It is recommended to fully test your integration here before deploying to the Production environment. |
| Production  | [https://api.ramp.com↗](https://api.ramp.com/)           | The Production environment is intended for live operations, allowing you to read and modify data in actual Ramp accounts. Only move to Production after thorough testing in the Sandbox environment.      |

* **Ramp Customers**: If you need access to a Sandbox environment, reach out to your account manager.
* **Technology Partners**: For more information on how to get access to Sandbox and Production environments, follow our [Build with Ramp](https://docs.ramp.com/developer-api/v1/overview/build-with-ramp) guide.

## Step 2: Set Up Your Application

[Permanent link: Step 2: Set Up Your Application](https://docs.ramp.com/developer-api/v1/guides/getting-started#step-2-set-up-your-application)

After accessing your account, the next step is to configure your App:

1. Within your account, navigate to **Settings > Developer API**
2. Click **Create New App**.
3. Provide your app name and description, sign the Terms of Service, and click **Create**.
4. Under Grant types, click **Add new grant type**
5. Select **Client Credentials** and press **Add**
6. Under Scopes, click **Configure allowed scopes**
7. Select `transactions:read` and press **Set**

## Step 3: Obtain an Access Token

[Permanent link: Step 3: Obtain an Access Token](https://docs.ramp.com/developer-api/v1/guides/getting-started#step-3-obtain-an-access-token)

Ramp supports two main authorization methods:

* **Client Credentials** (used for this guide).
* **OAuth 2.0 Authorization Code Grant** (required for Ramp partners integrating for multiple customers).

In this guide, we’ll focus on using Client Credentials to obtain an access token, which allows you to make API calls directly from your application.

### **Copy Your Client Credentials**

Before making any API requests, you will need your **Client ID** and **Client Secret**. These credentials are generated when you register your application.

1. Navigate to the **Ramp Developer Dashboard** and select your app.
2. In the app details page, copy the **Client ID** and **Client Secret**. Keep these credentials secure as they are required for authorization.

### **Request an Access Token**

To obtain an access token, you will need to send a POST request to the /token endpoint. In the response, you will receive a token which can be used to make API calls.

Here is a detailed breakdown of the request parameters:

* **Authorization Header**: This contains the **Client ID** and **Client Secret**, encoded in base64. The format is Authorization: Basic `<base64(client_id:client_secret)>`.
* **Content-Type**: Set this to `application/x-www-form-urlencoded`.
* **grant\_type**: This should be set to client\_credentials for this flow.
* **scope**: Specify the permissions you need. For this guide, we will use `transactions:read`.

Requesting a tokenShell Copy "curl --location --request POST 'https://api.ramp.com/developer/v1/token' \ --header "Authorization: Basic $(echo -n "$CLIENT\_ID:$CLIENT\_SECRET" | base64)" \ --header "Content-Type: application/x-www-form-urlencoded" \ --data "grant\_type=client\_credentials" \ --data "scope=transactions:read"" to clipboard

```
curl --location --request POST 'https://api.ramp.com/developer/v1/token' \    --header "Authorization: Basic $(echo -n "$CLIENT_ID:$CLIENT_SECRET" | base64)" \    --header "Content-Type: application/x-www-form-urlencoded" \    --data "grant_type=client_credentials" \    --data "scope=transactions:read"

```

In the response, you will receive an access token:

```
{  "access_token": "ramp_tok_o4bfbrfhDBdXcjjTBMD1iTTXdprRuHdcmne8gR5zfSV78uPe",  "token_type": "bearer",  "expires_in": 864000,  "scope": "transactions:read"}
```

## Step 4: Make your first API Call

[Permanent link: Step 4: Make your first API Call](https://docs.ramp.com/developer-api/v1/guides/getting-started#step-4-make-your-first-api-call)

With your **`access_token`**, you’re ready to make your first API call. Here’s an example of retrieving transactions:

Retrieving transactionsShell Copy "curl \ -H "Accept: application/json" \ -H "Authorization: Bearer $RAMP\_API\_TOKEN" \ 'https://api.ramp.com/developer/v1/transactions'" to clipboard

```
curl \  -H "Accept: application/json" \  -H "Authorization: Bearer $RAMP_API_TOKEN" \    'https://api.ramp.com/developer/v1/transactions'
```

You can see a sample response [here](https://docs.ramp.com/developer-api/v1/api/transactions#get-developer-v1-transactions).

Congratulations! You’ve made your first API call and are ready to start developing your application with the Ramp API.

\
