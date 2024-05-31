# CrypticProxyAPI Documentation v1.0.0-beta2

## Overview

The CrypticProxyAPI is a RESTful API designed to manage and retrieve player data in a Minecraft server environment. It provides various endpoints for accessing and manipulating player data, as well as server information. Key features include player data management, server requests, rate limiting, and access key security.

### Key Features
- **Player Data Management**: Retrieve and update player data such as last logout time, the server from which the player logged out, and other player-specific information.
- **Server Requests**: Access information about proxy servers, specific server players, and player data by UUID or username.
- **Rate Limiting**: Prevent abuse or overuse of the API with a request limiter.
- **Access Key Checking**: Ensure secure access by validating an access key in request headers.

## Endpoints

### Root Endpoint
`GET <domain>/`
- **Description**: Returns a body message indicating if the server is running.
- **Response**:
```json
{
  "status": "CrypticProxyDataAPI is running!"
  "version": "<Api version> (String)"
}
```

### Player Data Endpoint
`GET <domain>/player/<player>`
- **Description**: Retrieves player data if available.
- **Parameters**: 
  - `player` (string): The player's UUID or username.
- **Responses**:
  - **If the player has a data file**:
  ```json
  {
    "LastLogout": long,
    "Uuid": "String",
    "JoinedServers": {
      "Server1": {
        "LastLogout": long,
        "Amount": int
      },
      "Server2": {
        "LastLogout": long,
        "Amount": int
      }
    },
    "Server": "String/null",
    "Username": "madtrent",
    "PastUsernames": [
      "Name 1"
    ],
    "Connected": boolean,
    "LastLogoutServer": "String",
    "JoinedBefore": boolean,
    "FavoriteServer": "String"
  }
  ```
  - **If the player doesn't have a data file**:
  ```json
  {
    "Uuid": "String",
    "Username": "String",
    "JoinedBefore": boolean,
  }
  ```

### Proxy Servers Endpoint
`GET <domain>/proxy`
- **Description**: Retrieves the status of all proxy servers.
- **Response**:
```json
{
  "server1": {
    "playerCount": int,
    "online": boolean
  },
  "server2": {
    "playerCount": int,
    "online": boolean
  }
}
```

### Specific Proxy Server Endpoint
`GET <domain>/proxy/<server>`
- **Description**: Retrieves the list of players and the server name for a specific proxy server.
- **Parameters**: 
  - `server` (string): The name of the proxy server.
- **Response**:
```json
{
  "players": ["player1", "player2"],
  "serverName": "String"
}
```

### Specific Player Data in Proxy Server Endpoint
`GET <domain>/proxy/<server>/<uuidofplayer>`
- **Description**: Retrieves specific player data from a given proxy server.
- **Parameters**:
  - `server` (string): The name of the proxy server.
  - `uuidofplayer` (string): The UUID of the player.
- **Response**:
```json
{
  "Uuid": "String",
  "Username": "String",
  "ServerName": "String",
  "JoinAmount": int,
  "LastSeen": long
}
```

## Errors

### Error Responses
- **General Error**:
```json
{
  "error": "description"
}
```

Errors are returned with an appropriate HTTP status code and a description of the error encountered.

### Common Errors
- **400 Bad Request**: The server could not understand the request due to invalid syntax or missing required parameters.
- **404 Not Found**: The requested resource could not be found.
- **401 Unauthorized**: Access key is missing or invalid.
- **500 Internal Server Error**: An unexpected error occurred on the server.

## Configuration
Configuration options for the CrypticProxyAPI are specified in the config file:

```yaml
port: <port you want the server to run on>
domain: <The domain you want the server to run on>
limit: <The rate limiter>
FloodgatePrefix: <If your using floodGate your bedrock player prefix>
AccessKey:
  enabled: <If you want it to require an access key>
  key: <The required access key if enabled>
```

## Commands Overview

### Console Commands

#### `capi`
- **Description**: Opens a help page with a list of available commands.
- **Usage**: `capi`

#### `capi reload`
- **Description**: Reloads the configuration file.
- **Usage**: `capi reload`

#### `capi keygen`
- **Description**: Generates a new access key for the API.
- **Usage**: `capi keygen`
  
#### `capi version`
- **Description**: Checks the version of the plugin and if there are any updates.
- **Usage**: `capi version`

### Player Commands

#### `lastseen <player>`
- **Description**: Checks when a specified player was last seen on the proxy and which server they were on.
- **Permission**: `CrypticProxyAPI.lastseen.use`
- **Usage**: `/lastseen <player>`

## Example Usage in Different Languages

### Java
```java
import java.io.IOException;
import java.net.URI;
import java.net.URISyntaxException;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;
import org.json.JSONObject;

public class Main {
    private static final String ACCESS_KEY = "2910294e-eee7-490e-9264-eb07fbd78519";
    private static final String BASE_URL = "http://localhost:7000/";
    private static final HttpClient CLIENT = HttpClient.newHttpClient();

    public static void main(String[] args) {
        sendRequest("", "status");
    }

    public static void sendRequest(String value, String json) {

        try {
            HttpRequest request = HttpRequest.newBuilder()
                    .uri(new URI(BASE_URL + value))
                    .header("Access-Key", ACCESS_KEY)
                    .GET()
                    .build();

            HttpResponse<String> response = CLIENT.send(request, HttpResponse.BodyHandlers.ofString());

            JSONObject jsonObject = new JSONObject(response.body());

            System.out.println(jsonObject.get(json));

        } catch (URISyntaxException | IOException | InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

### Python
```python
import requests
import json

ACCESS_KEY = '2910294e-eee7-490e-9264-eb07fbd78519'
BASE_URL = "http://localhost:7000/";

def send_request(value, json_key):
    headers = {'Access-Key': ACCESS_KEY}
    response = requests.get(BASE_URL + value, headers=headers)

    if response.status_code == 200:
        json_object = json.loads(response.text)
        print(json_object.get(json_key))
    else:
        print(f'Request failed with status code {response.status_code}')

def main():
    send_request('', 'status')

if __name__ == '__main__':
    main()
```

### JavaScript (Node.js)
```javascript
const axios = require('axios');

const ACCESS_KEY = "2910294e-eee7-490e-9264-eb07fbd78519";
const BASE_URL = "http://localhost:7000/";

async function sendRequest(value, jsonKey) {
    try {
        const response = await axios.get(`${BASE_URL}/${value}`, {
            headers: {
                'Access-Key': ACCESS_KEY
            }
        });

        console.log(response.data[jsonKey]);
    } catch (error) {
        console.error(`Request failed with status code ${error.response.status}`);
    }
}

sendRequest('', 'status');
```
```
