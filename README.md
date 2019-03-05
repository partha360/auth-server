# Node JS Authentication Reference Implementation

## Tech Stack

- passport - For Authentication
- winston & morgon - For logging
- expressSession - Store logged in session (Azure AD only)
- mongodb - To store users (JWT & local strategy)
- bcrypt - Hash password in db
- jwt-simple - Generate jwt token

## Config & Keys

Add the following files under "config" folder.

### **config.js**

```json
exports.creds = {
  // Required
  identityMetadata:
    "https://login.microsoftonline.com/<tenant-name>.onmicrosoft.com/.well-known/openid-configuration",
  clientID: "xxxxx-xxxx-xxx-xxxxx-xxxx",

  // Required, must be "code", "code id_token", "id_token code" or "id_token"
  responseType: "code id_token",

  // Required
  responseMode: "form_post",

  // Required, the reply URL registered in AAD for your app
  redirectUrl: "http://localhost:3000/auth/openid/return",

  // Required if we use http for redirectUrl
  allowHttpForRedirectUrl: true,

  clientSecret: "xxxxxxxxxxxxxxxxx===",

  // Required to set to false if you don"t want to validate issuer
  validateIssuer: true,

  // Required to set to true if you are using B2C endpoint
  // This sample is for v1 endpoint only, so we set it to false
  isB2C: false,

  // Required if you want to provide the issuer(s) you want to validate instead of using the issuer from metadata
  issuer: null,

  // Required to set to true if the `verify` function has "req" as the first parameter
  passReqToCallback: false,

  // Recommended to set to true. By default we save state in express session, if this option is set to true, then
  // we encrypt state and save it in cookie instead. This option together with { session: false } allows your app
  // to be completely express session free.
  useCookieInsteadOfSession: true,

  // Required if `useCookieInsteadOfSession` is set to true. You can provide multiple set of key/iv pairs for key
  // rollover purpose. We always use the first set of key/iv pair to encrypt cookie, but we will try every set of
  // key/iv pair to decrypt cookie. Key can be any string of length 32, and iv can be any string of length 12.
  cookieEncryptionKeys: [
    { key: "12345678901234567890123456789012", iv: "123456789012" },
    { key: "abcdefghijklmnopqrstuvwxyzabcdef", iv: "abcdefghijkl" }
  ],

  // Optional. The additional scope you want besides "openid", for example: ["email", "profile"].
  scope: null,

  // Optional, "error", "warn" or "info"
  loggingLevel: "info",

  // Optional. The lifetime of nonce in session or cookie, the default value is 3600 (seconds).
  nonceLifetime: null,

  // Optional. The max amount of nonce saved in session or cookie, the default value is 10.
  nonceMaxAmount: 5,

  // Optional. The clock skew allowed in token validation, the default value is 300 seconds.
  clockSkew: null
}

// Optional.
// If you want to get access_token for a specific resource, you can provide the resource here; otherwise,
// set the value to null.
// Note that in order to get access_token, the responseType must be "code", "code id_token" or "id_token code".
exports.resourceURL = "https://graph.windows.net"

// The url you need to go to destroy the session with AAD
exports.destroySessionUrl =
  "https://login.microsoftonline.com/common/oauth2/logout?post_logout_redirect_uri=http://localhost:3000"
```

### **keys.js**

```json
// App secrets and configs

module.exports = {
  "secret": "any-jibberish",
  "mongoConnectionString": "mongo connection string"
}
```

### .env

```json
AzureClientId=<Azure Client ID>
AzureClientSecret=<Azure Client Secret>
MongoConnectionString=<Connection String>
```

## Install and Run

```shell
yarn install / npm install

yarn dev / npm run dev
```

## Custom Auth (Local & JWT Strategy)

http://localhost:3000/signin - Signin with Email, Password

http://localhost:3000/signup - SignUp with Email, Password and provides token

## Azure AD Auth (OIDC Strategy)

http://localhost:3000/login - Login with Azure AD and receive response

http://localhost:3000/account - Get logged in user"s details from Azure AD (if succesfully logged in)

## Testing

Import Postman Collection - https://www.getpostman.com/collections/0691e111e380d9115bca

## Code Quality

Pre-configured ESLint and Prettier

```sh
yarn lint or npm run lint

yarn format or npm run format
```

## References

- https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-devquickstarts-openidconnect-nodejs
