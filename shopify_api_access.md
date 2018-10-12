
Shopify API documentation: 

https://help.shopify.com/en/api/reference

## Step 1: Create Shopify App

1. Register for a Shopify Partner's account
2. Create an app in the account and set up the app URL and callback URL accordingly
3. In the bottom of the settings page, there is an API key and secret.

Public apps authenticate to Shopify by providing the X-Shopify-Access-Token header field in each HTTP request to the Shopify API. This access token is obtained through an OAuth handshake.

If you are creating a private app - those can be created in the 'apps' section in the admin panel of the shop. Private apps have some limitations but you don't need an access token for those. You can use the API key and password to authenticate directly.

## Step 2: Obtain Access Token (using OAuth)

The instructions is here: https://help.shopify.com/en/api/getting-started/authentication/oauth

The steps are basically:

1. Install the app by redirecting the user to the following URL:
https://{shop}.myshopify.com/admin/oauth/authorize?client_id={api_key}&scope={scopes}&redirect_uri={redirect_uri}&state={nonce}&grant_options[]={option}

2. Once the user (or you, in this case) clicks the "install" button, they will be redirected to a client server, which provides a Authorization Code (along with other parameters). Use the code to obtain access token:

`POST https://{shop}.myshopify.com/admin/oauth/access_token`

With `{shop}` substituted for the name of the user's shop and with the following parameters provided in the body of the request:

* client_id: The API Key for the app (see the credentials section of this guide).
* client_secret: The Secret Key for the app (see the credentials section of this guide).
* code: The authorization code provided in the redirect described above. 

The server will respond with an access token:

```
{
  "access_token": "f85632530bf277ec9ac6f649fc327f17",
  "scope": "write_orders,read_customers"
}
```


You can now make HTTP requests by providing the header X-Shopify-Access-Token: {access_token} . There is no need to provide the API key and password. The token grants permanent access.

## Step 3: make HTTP requests to the REST API

For example: To create a customer with only an email address (remember to use header above for public app, or key/password for private app)

`POST /admin/customers.json`

```
{
  "customer": {
    "email": "emailAddress@gmail.com", 
    "first_name": null,
    "last_name": null
  }
}
```

Remember to include `read_customers` and `write_customers` permissions in the scope when you're requesting the access token.

Detailed documentation can be found here: https://help.shopify.com/en/api/reference/customers/customer