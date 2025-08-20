# Segment Public API MCP Server

A Model Context Protocol (MCP) server that provides access to all Segment Public API endpoints as tools for AI assistants like Claude.

## Prerequisites

- Node.js (v18 or higher)
- npm
- A valid Segment API token

## Setup

### 1. Install Dependencies

```bash
npm install
```

### 2. Set Environment Variable

Export your Segment API token as an environment variable:

```bash
export SEGMENT_API_TOKEN="your_segment_api_token_here"
```

Add this to your shell profile (`~/.zshrc`, `~/.bashrc`, etc.) to make it persistent:

```bash
echo 'export SEGMENT_API_TOKEN="your_segment_api_token_here"' >> ~/.zshrc
source ~/.zshrc
```

### 3. Build the Project

```bash
npm run build
```

This compiles the TypeScript source code to JavaScript in the `build/` directory.

## Running Locally

### Test with MCP Inspector

The MCP Inspector provides a web interface to test your server:

```bash
SEGMENT_API_TOKEN="YOUR_PAPI_TOKEN" npx @modelcontextprotocol/inspector node build/index.js
```

This will:

- Start the MCP server
- Launch a web interface (usually at `http://localhost:6274`)
- Allow you to test individual tools and see all available Segment API endpoints

### Run the Streamable HTTP server

If your build of src/index.ts starts an Express HTTP server (streamable HTTP transport or a custom HTTP endpoint), run the compiled server directly:

1. Build the project
```bash
npm run build
```

2. Start the server (temporary env var for this run)
```bash
SEGMENT_API_TOKEN="YOUR_PAPI_TOKEN" npm start
# or
SEGMENT_API_TOKEN="YOUR_PAPI_TOKEN" node build/index.js
```

3. Open the server in your browser
- The server will log the listening address/port when it starts. Default port used in examples is `8000`, so try:
  - http://localhost:8000/mcp
- If no port appears in the logs, check src/index.ts for the configured port or the "streamableHttp" host/port printed at startup.

## Installing in Claude Desktop

### 1. Locate Claude's Configuration File

The config file is located at:

```text
~/Library/Application Support/Claude/claude_desktop_config.json
```

### 2. Add the MCP Server

Add your server to the `mcpServers` section:

```json
{
  "mcpServers": {
    "segment-public-api": {
      "command": "node",
      "args": ["/absolute/path/to/your/project/build/index.js"],
      "env": {
        "SEGMENT_API_TOKEN": "your_segment_api_token_here"
      }
    }
  }
}
```

**Replace `/absolute/path/to/your/project/` with the actual path to this project directory.**

### 3. Restart Claude Desktop

Quit and restart Claude Desktop to load the new configuration.

## Usage

Once installed, you can ask Claude to:

- List your Segment workspaces
- Get information about sources and destinations
- Create audiences and computed traits
- Manage user permissions
- Access any of the 200+ Segment Public API endpoints

Example prompts:

- "List my Segment workspaces"
- "Show me all sources in my workspace"
- "Create a new audience in space [space-id]"

## Development

### Project Structure

```text
src/
  index.ts          # Main MCP server implementation
build/
  index.js          # Compiled JavaScript (generated)
package.json        # Dependencies and build scripts
tsconfig.json       # TypeScript configuration
```

### Making Changes

1. Edit the TypeScript source in `src/index.ts`
2. Rebuild the project: `npm run build`
3. Test with the inspector: `SEGMENT_API_TOKEN="YOUR_PAPI_TOKEN" npx @modelcontextprotocol/inspector node build/index.js`
4. Restart Claude Desktop to use the updated server

## Troubleshooting

### "Unauthorized" Error

This usually means the `SEGMENT_API_TOKEN` is not properly set:

1. Verify the token is set: `echo $SEGMENT_API_TOKEN`
2. Use the explicit `env` block in Claude's config (recommended)

### Inspector Won't Start

If you get "PORT IS IN USE" errors:

```bash
# Kill existing inspector processes
pkill -f "inspector"

# Wait a moment, then try again
SEGMENT_API_TOKEN="YOUR_PAPI_TOKEN" npx @modelcontextprotocol/inspector node build/index.js
```

### Build Errors

Make sure all dependencies are installed:

```bash
npm install
npm run build
```

## API Reference

This server provides access to all Segment Public API endpoints. Each API endpoint becomes an available tool in Claude with:

- Automatic request validation
- Proper authentication headers
- Error handling and response formatting
- Full parameter support as defined in the Segment OpenAPI specification

For complete API documentation, visit: [Segment API Documentation](https://docs.segmentapis.com/)
