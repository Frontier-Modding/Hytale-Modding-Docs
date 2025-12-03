# Api Endpoints

This section documents all known API endpoints that the Hytale Launcher/Client/Server might access.

### Misc
These endpoints are listed here for completeness sake, but will likely be removed in the full release of the game.

| Endpoint URL                                            | HTTP Method | Format Notes                   | Description                                                  |
| ------------------------------------------------------- | ----------- | ------------------------------ | ------------------------------------------------------------ |
| `https://account-data.arcanitegames.ca/my-account/ping` | GET         | Bearer Token Auth              | Every couple seconds the game client sends a ping with the current session token. |
| `https://api.ipify.org/`                                | GET         |                                | The game client uses this to look up the external IP address of the current host. |
| `https://sentry.hytale.com/api/2/envelope/`             | POST        | Content-Type: application/json | Statistics and error collection via [Sentry](https://sentry.io). |



### Game Updates

> [!NOTE]
> for all of these endpoints, the launcher sends the current running launcher version in the `X-Hytale-Launcher-Version` header, and the current launcher update channel in the `X-Hytale-Launcher-Branch` header.

| Endpoint URL                                                 | HTTP Method | Format Notes                                                 | Description                                                  |
| ------------------------------------------------------------ | ----------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| `https://launcher.arcanitegames.ca/launcher-feed/feed.json`  | GET         |                                                              | Update news and other Articles to be rendered inside the launcher. Currently only placeholder text. |
| `https://launcher.arcanitegames.ca/version/<update channel>/launcher.json` | GET         |                                                              | Used to get the launcher version and downloads. Does not require auth. |
| `https://launcher.arcanitegames.ca/version/<update channel>/jre.json` | GET         |                                                              | Used to get the Java JRE version and downloads. Does not require auth. |
| `https://account-data.arcanitegames.ca/my-account/get-launcher-data?arch=<hardware architecture>&os=<operating system>` | GET         | - Bearer Token Auth<br />- Query parameters: `arch` (operating system architecture), `os` (operating system type) | Used to get account information as well as check for game updates. response contains account ID, profiles, as well as the latest game version for each `update channel` (currently only `release`). |
| `https://account-data.arcanitegames.ca/patches/<operating system>/<hardware architecture>/<update channel>/<current local build>` | GET         | - Bearer Token Auth<br/>- the last parameter is which build the client is updating *from*, or `0` if the client has no existing local files. | Used to retrieve the delta patches to get from a given game build to the latest avaliable build. |
