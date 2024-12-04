# python_local MCP Server

An MCP Server that provides an interactive Python REPL (Read-Eval-Print Loop) environment.

## Components

### Resources

The server provides access to REPL session history:
- Custom `repl://` URI scheme for accessing session history
- Each session's history can be viewed as a text/plain resource
- History shows input code and corresponding output for each execution

### Tools

The server implements one tool:
- `python_repl`: Executes Python code in a persistent session
  - Takes `code` (Python code to execute) and `session_id` as required arguments
  - Maintains separate state for each session
  - Supports both expressions and statements
  - Captures and returns stdout/stderr output

## Configuration

### Install

#### Claude Desktop

On MacOS: `~/Library/Application\ Support/Claude/claude_desktop_config.json`
On Windows: `%APPDATA%/Roaming/Claude/config.json`

<details>
  <summary>Development/Unpublished Servers Configuration</summary>
  ```json
  "mcpServers": {
    "python_local": {
      "command": "uv",
      "args": [
        "--directory",
        "/path/to/python_local",
        "run",
        "python_local"
      ]
    }
  }
  ```
</details>

<details>
  <summary>Published Servers Configuration</summary>
  ```json
  "mcpServers": {
    "python_local": {
      "command": "uvx",
      "args": [
        "python_local"
      ]
    }
  }
  ```
</details>

## Development

### Building and Publishing

To prepare the package for distribution:

1. Sync dependencies and update lockfile:
```bash
uv sync
```

2. Build package distributions:
```bash
uv build
```

This will create source and wheel distributions in the `dist/` directory.

3. Publish to PyPI:
```bash
uv publish
```

Note: You'll need to set PyPI credentials via environment variables or command flags:
- Token: `--token` or `UV_PUBLISH_TOKEN`
- Or username/password: `--username`/`UV_PUBLISH_USERNAME` and `--password`/`UV_PUBLISH_PASSWORD`

### Debugging

Since MCP servers run over stdio, debugging can be challenging. For the best debugging
experience, we strongly recommend using the [MCP Inspector](https://github.com/modelcontextprotocol/inspector).

You can launch the MCP Inspector via [`npm`](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm) with this command:

```bash
npx @modelcontextprotocol/inspector uv --directory /path/to/python_local run python-local
```

Upon launching, the Inspector will display a URL that you can access in your browser to begin debugging.
