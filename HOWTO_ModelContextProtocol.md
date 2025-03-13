# Introduction to Model Context Protocol (MCP)

Model Context Protocol (MCP) is a standardized framework that allows AI models and assistants to interact with external tools, APIs, and services. MCP servers provide specific functionalities, and MCP clients connect to these servers to utilize their capabilities. The protocol enables seamless integration of third-party services into AI-powered workflows.

## Why Use MCP?

- Extend Functionality: Add new capabilities.
- Access External Data: Connect to databases, APIs, or other services.
- Custom Integrations: Create custom tools tailored to your specific workflow.
- Community Contributions: Share and use community tools.

## How MCP Works

- MCP Servers: Provide tools and services, such as web search or data retrieval.
- MCP Clients: Connect to MCP servers and use their tools.
- MCP Hosts: Platforms (e.g., VSCode extensions) that facilitate communication between clients and servers.

## Finding MCP Servers

- Community Repositories:  Check for community-maintained lists of MCP servers.
- GitHub Search: Search GitHub for "MCP server" or "Model Context Protocol server."
- Ask AI! It can help you find or even create MCP servers.

## Installing an MCP Server Package

We'll use the Brave Search API Server from modelcontextprotocol as an example.

Before installing the MCP server package, ensure that npm is installed on your system with:

```sh
sudo apt install npm
```

Once npm is installed, you need to install the required MCP package. Run the following command:

```sh
npm install -g @modelcontextprotocol/server-brave-search
```

If you prefer to install it locally within your project directory, use:

```sh
npm install @modelcontextprotocol/server-brave-search
```

## Setting Up (Connecting To) an MCP Server in VSCode

### 1. Define the MCP Server in Configuration

To integrate an MCP server, update your settings file with the correct configuration. For example, to set up the Brave Search MCP Server, use:

```json
{
  "mcpServers": {
    "brave-search": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-brave-search"
      ],
      "env": {
        "BRAVE_API_KEY": "YOUR_API_KEY_HERE"
      }
    }
  }
}
```

Replace `YOUR_API_KEY_HERE` with your actual Brave Search API key.

### 2. Ensure the MCP Server is Recognized

After adding the server configuration:

- Restart VSCode.
- Check available MCP servers using VS Code's or your extension's interface.
- If the server does not appear, reload the VSCode window (`Command Palette -> Developer: Reload Window`).

### 3. Troubleshooting

- Make sure your JSON configuration is correctly formatted.
- Check if the server is running by listing connected MCP servers.
  ```npx @modelcontextprotocol/list-mcp```
- If manually running the server, try restarting VS Code.
- If you encounter rate limits or authentication issues, ensure your API key is correct and valid.

## Using MCP Tools and Resources

Once an MCP server is connected, its tools and resources become available.

- Tools: MCP tools are invoked to perform functions. You'll need to provide the server name and the tool name.
- Resources: MCP resources can be accessed using the server name and the resource URI.

### Key Lessons Learned

- Follow the Example Configuration Exactly: Any deviation may cause connection failures.
- Let VSCode (via RooCode, Cline, or another MCP-enabled extension) handle the server lifecycle: Avoid manually starting/stopping the server.
- Ensure the API Key is Set: Some failures occur due to missing authentication.

By following these steps, you can successfully set up and use MCP servers within your development environment.
