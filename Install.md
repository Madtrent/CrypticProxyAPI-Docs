Sure, here's the updated guide with a section explaining the `FloodgatePrefix`.

---

## How to Install CrypticProxyAPI

Follow these steps to install and configure CrypticProxyAPI on your Minecraft proxy server:

### Step-by-Step Installation Guide

1. **Download and Install**
   - **Download** the CrypticProxyAPI plugin.
   - **Drag and Drop** the downloaded plugin file into the plugins folder of your proxy server.

2. **Open a Port**
   - **Open a Port** on your proxy server for the API to listen to. This is necessary for the API to receive requests.
   - **Update the Configuration**:
     - Open the configuration file (`config.yml`) located in the CrypticProxyAPI plugin directory.
     - Define the port you opened under the `port` section:
       ```yaml
       port: 1234
       ```

3. **Link Your Domain/IP**
   - **Link Your Domain/IP** to the port you've opened:
     - In the same configuration file (`config.yml`), update the `domain` section with your domain or IP address:
       ```yaml
       domain: 0.0.0.0
       ```

4. **Enable Access Key (Optional)**
   - **Enable the Access Key** if you want to use it for securing your API:
     - In the configuration file, set `enabled` to `true` under the `AccessKey` section:
       ```yaml
       AccessKey:
         enabled: true
       ```

5. **Generate an Access Key**
   - **Run the API** by starting your proxy server with the CrypticProxyAPI plugin installed.
   - **Generate an Access Key**:
     - Open your proxy server console and run the following command:
       ```
       capi keygen
       ```
     - The console will generate a new access key and update the configuration file:
       ```yaml
       AccessKey:
         key: 2910294e-eee7-490e-9264-eb07fbd78519
       ```

6. **Save and Restart**
   - **Save** the configuration file after making the necessary changes.
   - **Restart** your proxy server to apply the new configuration.

### Example Configuration

Here is an example of what your `config.yml` might look like after configuration:

```yaml
port: 1234
domain: 0.0.0.0
limit: 100
FloodgatePrefix: ''
AccessKey:
  enabled: true
  key: 2910294e-eee7-490e-9264-eb07fbd78519
```

### FloodgatePrefix Configuration

`FloodgatePrefix` is a configuration setting used to specify a prefix for players connecting through GeyserMC, a tool that enables Bedrock Edition players to join Java Edition servers. Setting this prefix helps in distinguishing between Java and Bedrock players.

The prefix configured here needs to match the prefix set in your Floodgate configuration. This allows the API to correctly distinguish between different player UUIDs and ensure proper identification of Bedrock players.

**Example Usage:**
- If in your floodgate config you have bedrock player to have a prefix like `BR-` you would set the `FloodgatePrefix` as follows:
  ```yaml
  FloodgatePrefix: 'BR-'
  ```
- If your server does not have bedrock support this is not needed
  ```yaml
  FloodgatePrefix: ''
  ```
  **Note:** Make sure the prefix you set in the `FloodgatePrefix` configuration of CrypticProxyAPI matches exactly with the prefix configured in your Floodgate plugin.

This setting helps server administrators manage and identify Bedrock players more easily, ensuring a seamless experience for both Java and Bedrock users.

### Quick Recap

1. **Drag and drop** the plugin into your proxy server's plugins folder.
2. **Open a port** on your proxy server and define it in the config.
3. **Link your domain/IP** to the port and set it in the config.
4. **Enable the access key** if needed and generate a new key using `capi keygen`.
5. **Set the `FloodgatePrefix`** if needed.
6. **Save and restart** your proxy server.

By following these steps, you will have CrypticProxyAPI installed and running on your Minecraft proxy server, ready to manage and retrieve player data securely and efficiently.

---
