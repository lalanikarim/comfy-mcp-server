# Comfy MCP Server

[![smithery badge](https://smithery.ai/badge/@lalanikarim/comfy-mcp-server)](https://smithery.ai/server/@lalanikarim/comfy-mcp-server)

> A server using FastMCP framework to generate images based on prompts via a remote Comfy server.

## Overview

This script sets up a server using the FastMCP framework to generate images based on prompts using a specified workflow. It interacts with a remote Comfy server to submit prompts and retrieve generated images.

## Prerequisites

- [uv](https://docs.astral.sh/uv/) package and project manager for Python.
- Workflow file exported from Comfy UI. This code includes a sample `Flux-Dev-ComfyUI-Workflow.json` which is only used here as reference. You will need to export from your workflow and set the environment variables accordingly.

You can install the required packages for local development:

```bash
uvx mcp[cli]
```

## Configuration

Set the following environment variables:

- `COMFY_URL` to point to your Comfy server URL.
- `COMFY_WORKFLOW_JSON_FILE` to point to the absolute path of the API export json file for the comfyui workflow.
- `PROMPT_NODE_ID` to the id of the text prompt node.
- `OUTPUT_NODE_ID` to the id of the output node with the final image.
- `OUTPUT_MODE` to either `url` or `file` to select desired output.

Example:

```bash
export COMFY_URL=http://your-comfy-server-url:port
export COMFY_WORKFLOW_JSON_FILE=/path/to/the/comfyui_workflow_export.json
export PROMPT_NODE_ID=6 # use the correct node id here
export OUTPUT_NODE_ID=9 # use the correct node id here
export OUTPUT_MODE=file
```

## Usage

Comfy MCP Server can be launched by the following command:

```bash
uvx comfy-mcp-server
```

### Example Claude Desktop Config

```json
{
  "mcpServers": {
    "Comfy MCP Server": {
      "command": "/path/to/uvx",
      "args": [
        "comfy-mcp-server"
      ],
      "env": {
        "COMFY_URL": "http://your-comfy-server-url:port",
        "COMFY_WORKFLOW_JSON_FILE": "/path/to/the/comfyui_workflow_export.json",
        "PROMPT_NODE_ID": "6",
        "OUTPUT_NODE_ID": "9",
        "OUTPUT_MODE": "file",
      }
    }
  }
}

```

## Functionality

### `generate_image(prompt: str, ctx: Context) -> Image | str`

This function generates an image using a specified prompt. It follows these steps:

1. Checks if all the environment variable are set.
2. Loads a prompt template from a JSON file.
3. Submits the prompt to the Comfy server.
4. Polls the server for the status of the prompt processing.
5. Retrieves and returns the generated image once it's ready.

## Dependencies

- `mcp`: For setting up the FastMCP server.
- `json`: For handling JSON data.
- `urllib`: For making HTTP requests.
- `time`: For adding delays in polling.
- `os`: For accessing environment variables.

## License

This project is licensed under the MIT License - see the [LICENSE](https://github.com/lalanikarim/comfy-mcp-server/blob/main/LICENSE) file for details.
