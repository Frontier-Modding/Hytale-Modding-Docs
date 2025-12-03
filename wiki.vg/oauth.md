# OAuth documentation for Arcanite Games (Hytale)



### Configuration for the Hytale Game Client:

| Type                    | Value                                                        |
| ----------------------- | ------------------------------------------------------------ |
| Authentication URL      | `https://oauth.accounts.arcanitegames.ca/oauth2/auth?access_type=offline` |
| Token URL               | `https://oauth.accounts.arcanitegames.ca/oauth2/token`       |
| Redirect URL            | `https://accounts.arcanitegames.ca/consent/client`           |
| Client ID               | `hytale-launcher`                                            |
| OAuth Scopes            | `openid`, `offline`                                          |
| Authorization Code Flow | Public Client, with [Proof Key for Code Exchange (PKCE)](https://oauth.net/2/pkce/) |

### Notes (sort me!)

- `access_type` for the `Authentication URL` should be `offline`

- The game client generates a random local port to redirect to, and hides it in the CSRF token in the original Auth URL presented to the user. It does so by Base64-encoding the following JSON payload and sending that as the CSRF token value:

  ```json
  {
  	"state": "actual CSRF nonce value",
      "port": 50000
  }
  ```

  Note that for later verification only the value of the `state` field inside the JSON is used, not the whole base64-encoded JSON that was sent as `state` query parameter to the auth server.

- The port appears to be somewhere between `50000-60000` but no guarantees this is correct.

