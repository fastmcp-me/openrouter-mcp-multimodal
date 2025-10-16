[![Add to Cursor](https://fastmcp.me/badges/cursor_dark.svg)](https://fastmcp.me/MCP/Details/554/openrouter)
[![Add to VS Code](https://fastmcp.me/badges/vscode_dark.svg)](https://fastmcp.me/MCP/Details/554/openrouter)
[![Add to Claude](https://fastmcp.me/badges/claude_dark.svg)](https://fastmcp.me/MCP/Details/554/openrouter)
[![Add to ChatGPT](https://fastmcp.me/badges/chatgpt_dark.svg)](https://fastmcp.me/MCP/Details/554/openrouter)
[![Add to Codex](https://fastmcp.me/badges/codex_dark.svg)](https://fastmcp.me/MCP/Details/554/openrouter)
[![Add to Gemini](https://fastmcp.me/badges/gemini_dark.svg)](https://fastmcp.me/MCP/Details/554/openrouter)

# OpenRouter MCP Multimodal Server

[![Build Status](https://github.com/stabgan/openrouter-mcp-multimodal/actions/workflows/publish.yml/badge.svg)](https://github.com/stabgan/openrouter-mcp-multimodal/actions/workflows/publish.yml)
[![npm version](https://img.shields.io/npm/v/@stabgan/openrouter-mcp-multimodal.svg)](https://www.npmjs.com/package/@stabgan/openrouter-mcp-multimodal)
[![Docker Pulls](https://img.shields.io/docker/pulls/stabgandocker/openrouter-mcp-multimodal.svg)](https://hub.docker.com/r/stabgandocker/openrouter-mcp-multimodal)

An MCP (Model Context Protocol) server that provides chat and image analysis capabilities through OpenRouter.ai's diverse model ecosystem. This server combines text chat functionality with powerful image analysis capabilities.

## Features

- **Text Chat:**
  - Direct access to all OpenRouter.ai chat models
  - Support for simple text and multimodal conversations
  - Configurable temperature and other parameters

- **Image Analysis:**
  - Analyze single images with custom questions
  - Process multiple images simultaneously 
  - Automatic image resizing and optimization
  - Support for various image sources (local files, URLs, data URLs)

- **Model Selection:**
  - Search and filter available models
  - Validate model IDs
  - Get detailed model information
  - Support for default model configuration

- **Performance Optimization:**
  - Smart model information caching
  - Exponential backoff for retries
  - Automatic rate limit handling

## What's New in 1.5.0

- **Improved OS Compatibility:**
  - Enhanced path handling for Windows, macOS, and Linux
  - Better support for Windows-style paths with drive letters
  - Normalized path processing for consistent behavior across platforms

- **MCP Configuration Support:**
  - Cursor MCP integration without requiring environment variables
  - Direct configuration via MCP parameters
  - Flexible API key and model specification options

- **Robust Error Handling:**
  - Improved fallback mechanisms for image processing
  - Better error reporting with specific diagnostics
  - Multiple backup strategies for file reading

- **Image Processing Enhancements:**
  - More reliable base64 encoding for all image types
  - Fallback options when Sharp module is unavailable
  - Better handling of large images with automatic optimization

## Installation

### Option 1: Install via npm

```bash
npm install -g @stabgan/openrouter-mcp-multimodal
```

### Option 2: Run via Docker

```bash
docker run -i -e OPENROUTER_API_KEY=your-api-key-here stabgandocker/openrouter-mcp-multimodal:latest
```

## Quick Start Configuration

### Prerequisites

1. Get your OpenRouter API key from [OpenRouter Keys](https://openrouter.ai/keys)
2. Choose a default model (optional)

### MCP Configuration Options

Add one of the following configurations to your MCP settings file (e.g., `cline_mcp_settings.json` or `claude_desktop_config.json`):

#### Option 1: Using npx (Node.js)

```json
{
  "mcpServers": {
    "openrouter": {
      "command": "npx",
      "args": [
        "-y",
        "@stabgan/openrouter-mcp-multimodal"
      ],
      "env": {
        "OPENROUTER_API_KEY": "your-api-key-here",
        "DEFAULT_MODEL": "qwen/qwen2.5-vl-32b-instruct:free"
      }
    }
  }
}
```

#### Option 2: Using uv (Python Package Manager)

```json
{
  "mcpServers": {
    "openrouter": {
      "command": "uv",
      "args": [
        "run",
        "-m",
        "openrouter_mcp_multimodal"
      ],
      "env": {
        "OPENROUTER_API_KEY": "your-api-key-here",
        "DEFAULT_MODEL": "qwen/qwen2.5-vl-32b-instruct:free"
      }
    }
  }
}
```

#### Option 3: Using Docker

```json
{
  "mcpServers": {
    "openrouter": {
      "command": "docker",
      "args": [
        "run",
        "--rm",
        "-i",
        "-e", "OPENROUTER_API_KEY=your-api-key-here",
        "-e", "DEFAULT_MODEL=qwen/qwen2.5-vl-32b-instruct:free",
        "stabgandocker/openrouter-mcp-multimodal:latest"
      ]
    }
  }
}
```

#### Option 4: Using Smithery (recommended)

```json
{
  "mcpServers": {
    "openrouter": {
      "command": "smithery",
      "args": [
        "run",
        "stabgan/openrouter-mcp-multimodal"
      ],
      "env": {
        "OPENROUTER_API_KEY": "your-api-key-here",
        "DEFAULT_MODEL": "qwen/qwen2.5-vl-32b-instruct:free"
      }
    }
  }
}
```

## Examples

For comprehensive examples of how to use this MCP server, check out the [examples directory](./examples/). We provide:

- JavaScript examples for Node.js applications
- Python examples with interactive chat capabilities
- Code snippets for integrating with various applications

Each example comes with clear documentation and step-by-step instructions.

## Dependencies

This project uses the following key dependencies:

- `@modelcontextprotocol/sdk`: ^1.8.0 - Latest MCP SDK for tool implementation
- `openai`: ^4.89.1 - OpenAI-compatible API client for OpenRouter
- `sharp`: ^0.33.5 - Fast image processing library
- `axios`: ^1.8.4 - HTTP client for API requests
- `node-fetch`: ^3.3.2 - Modern fetch implementation

Node.js 18 or later is required. All dependencies are regularly updated to ensure compatibility and security.

## Available Tools

### mcp_openrouter_chat_completion

Send text or multimodal messages to OpenRouter models:

```javascript
use_mcp_tool({
  server_name: "openrouter",
  tool_name: "mcp_openrouter_chat_completion",
  arguments: {
    model: "google/gemini-2.5-pro-exp-03-25:free", // Optional if default is set
    messages: [
      {
        role: "system",
        content: "You are a helpful assistant."
      },
      {
        role: "user",
        content: "What is the capital of France?"
      }
    ],
    temperature: 0.7 // Optional, defaults to 1.0
  }
});
```

For multimodal messages with images:

```javascript
use_mcp_tool({
  server_name: "openrouter",
  tool_name: "mcp_openrouter_chat_completion",
  arguments: {
    model: "anthropic/claude-3.5-sonnet",
    messages: [
      {
        role: "user",
        content: [
          {
            type: "text",
            text: "What's in this image?"
          },
          {
            type: "image_url",
            image_url: {
              url: "https://example.com/image.jpg"
            }
          }
        ]
      }
    ]
  }
});
```