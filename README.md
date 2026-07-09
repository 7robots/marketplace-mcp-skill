# ⚠️ ARCHIVED (2026-07-08)

Superseded: this skill targeted FastMCP 2 + the fastmcp.cloud/mcp-marketplace registry, replaced by the plugin-marketplace-manager MCP server and the `mcp-deploy-utils` skill in [7robots/my-skills](https://github.com/7robots/my-skills).

---

# Marketplace MCP Skill

A Claude skill for building and deploying MCP servers to [FastMCP Cloud](https://fastmcp.cloud).

## Overview

This skill provides patterns, templates, and guidance for creating production-ready MCP servers with FastMCP 2 that can be deployed to the fastmcp.cloud marketplace.

## Installation

Add this skill to your Claude Code configuration:

```bash
claude mcp add-skill marketplace-mcp-skill
```

Or add to your `.claude/settings.json`:

```json
{
  "skills": [
    "github:7robots/marketplace-mcp-skill"
  ]
}
```

## Usage

When building an MCP server, invoke the skill:

```
/marketplace-mcp
```

Or simply describe your task and Claude will use the skill automatically:

> "Build an MCP server for the GitHub API and deploy it to fastmcp.cloud"

## What's Included

### Skill Guide (`SKILL.md`)
- Quick start workflow
- Development process
- Tool naming conventions
- Response format patterns
- Error handling patterns
- Secrets management

### Reference Documentation

- **[FastMCP Patterns](reference/fastmcp_patterns.md)**: Tool registration, Pydantic models, pagination, HTTP client patterns
- **[Deployment Guide](reference/deployment.md)**: fastmcp.cloud deployment, secrets management, CI/CD

### Example Server (`example/`)
Complete, deployable MCP server template demonstrating all best practices:
- 5 example tools with full documentation
- JSON and Markdown response formats
- Pagination support
- Error handling
- Ready for fastmcp.cloud deployment

## Quick Start

### 1. Create a new MCP server

```bash
mkdir my-mcp-server && cd my-mcp-server
uv venv
uv pip install "fastmcp>=2.0.0,<3" "starlette>=1.0.1" httpx pydantic python-dotenv
```

### 2. Create server.py

```python
import os
from fastmcp import FastMCP

mcp = FastMCP("my-service-mcp")

@mcp.tool()
async def my_tool(param: str) -> str:
    """Tool description."""
    return f"Result: {param}"

if __name__ == "__main__":
    host = os.getenv("MCP_HOST", "0.0.0.0")
    port = int(os.getenv("MCP_PORT", "8000"))
    mcp.run(transport="http", host=host, port=port)
```

### 3. Test locally

```bash
python server.py
# Or: fastmcp inspect server.py
```

### 4. Deploy to fastmcp.cloud

```bash
# Create GitHub repo
gh repo create my-mcp-server --public --source=. --push

# Then visit fastmcp.cloud to connect and deploy
```

Your server will be available at: `https://my-mcp-server.fastmcp.app/mcp`

## Secrets Management

### Local Development

Create `.env` file (never commit):
```bash
API_KEY=your-secret-key
DATABASE_URL=postgresql://user:pass@host/db
```

### FastMCP Cloud

1. Go to project settings on fastmcp.cloud
2. Add environment variables under "Secrets"
3. Variables are encrypted and injected at runtime

## External Resources

- **FastMCP Documentation**: [gofastmcp.com](https://gofastmcp.com)
- **FastMCP Docs MCP Server**: `https://gofastmcp.com/mcp`
- **FastMCP Cloud**: [fastmcp.cloud](https://fastmcp.cloud)
- **MCP Protocol**: [modelcontextprotocol.io](https://modelcontextprotocol.io)

## File Structure

```
marketplace-mcp-skill/
├── SKILL.md                    # Main skill file for Claude
├── README.md                   # This file
├── reference/
│   ├── fastmcp_patterns.md     # FastMCP 2 implementation patterns
│   └── deployment.md           # Deployment and secrets guide
└── example/
    ├── server.py               # Complete example server
    ├── pyproject.toml          # Dependencies
    └── .gitignore              # Git ignore template
```

## License

MIT
