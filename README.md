# YouTube MCP Server made in Mule 4

Easily connect your YouTube API to an MCP server using MuleSoft. This guide will help you set up, run, and use the server locally and with your favorite apps.

This server is connecting to the [YouTube Data API (v3)](https://developers.google.com/youtube/v3/docs/search/list).


## Getting Your YouTube API Key

1. Go to the [Google Cloud Console](https://console.cloud.google.com/).
2. Create a new project.
3. Navigate to **APIs & Services** from the hamburger menu.
4. Click on **Enable APIs and Services**.
5. Add the **YouTube Data API v3** option.
6. Go back to **APIs & Services**.
7. Click on **Credentials**.
8. Select **Create Credentials > API Key**.
9. Open the newly created API key and under **API Restrictions**, select **Restrict Key**
10. Add the **YouTube Data API v3** checkmark.
11. Save your changes.

> [!NOTE] 
> This API key will be used to connect to the MCP server.


## Running the Mule App Locally

1. Clone this repository:
   ```sh
   git clone <repo-url>
   ```
2. Open the project in Anypoint Code Builder or Anypoint Studio.
3. Run the Mule app.
4. Note the port for the HTTP Listener (default: `8081`).

## Running Supergateway

1. Open a terminal window.
2. Run the following command (ensure the port matches your Mule app's HTTP Listener):
   ```sh
   npx -y supergateway --sse http://localhost:8081/sse --ssePath /sse --messagePath /message
   ```
3. You should see output similar to:
   ```
   [supergateway] Starting...
   [supergateway] Supergateway is supported by Supermachine (hosted MCPs) - https://supermachine.ai
   [supergateway]   - outputTransport: stdio
   [supergateway]   - sse: http://localhost:8081/sse
   [supergateway]   - Headers: (none)
   [supergateway] Connecting to SSE...
   [supergateway] Stdio server listening
   ```
   
> [!TIP] 
> Do not close this terminal window.

## Checking if the MCP Server is Running

1. Open a new terminal window and run:
   ```sh
   npx @modelcontextprotocol/inspector
   ```
2. You should see output like:
   ```
   Starting MCP inspector...
   ⚙️ Proxy server listening on 127.0.0.1:6277
   🔑 Session token: <your-session-token>
   Use this token to authenticate requests or set DANGEROUSLY_OMIT_AUTH=true to disable auth

   🔗 Open inspector with token pre-filled:
      http://localhost:6274/?MCP_PROXY_AUTH_TOKEN=<your-session-token>
      (Auto-open is disabled when authentication is enabled)

   🔍 MCP Inspector is up and running at http://127.0.0.1:6274 🚀
   ```
3. Copy the `Open inspector with token` link and open it in your browser.
4. Select transport type `SSE` and URL `http://0.0.0.0:8081/sse`.
5. Click **Connect**.
6. If you can't connect, ensure you're using the correct proxy auth/session token.
7. Once connected, go to **Tools** and click on the `youtube-search` server.
8. Add your YouTube API key in the `key` parameter and click **Run Tool**.
9. You should receive a response or a successful result.

## Adding the MCP Server to Your App

1. Open the JSON config file for your app (e.g., `claude_desktop_config.json` for Claude).
2. Add the following configuration (ensure the port matches your setup):
   ```json
   {
     "mcpServers": {
       "youtube": {
         "command": "npx",
         "args": [
           "-y",
           "supergateway",
           "--sse",
           "http://0.0.0.0:8081/sse",
           "--ssePath",
           "/sse",
           "--messagePath",
           "/message"
         ]
       }
     }
   }
   ```
3. Restart your app if needed (for Claude, a restart is required).

## Usage Examples
Once everything is set up, you can ask your agent questions like:

- Search for the Prostdev YouTube channel and give me the last 5 videos. Use this api key: `<paste your key>`
- Create a LinkedIn post that highlights the first 4 videos.
- Make sure to add the links to the videos in the post.

Enjoy using your YouTube MCP Server Mule integration! 🎬🚀
