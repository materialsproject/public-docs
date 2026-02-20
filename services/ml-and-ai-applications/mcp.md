---
description: >-
  Model context protocols for integrating Materials Project data with large lanuage models.
---

# Model Context Protocol

While large lanugage models (LLMs) are often trained on a fraction of the Materials Project data via web scraping, they lack more detailed data which is only accessible once logged into your account. Certain LLMs can leverage tools to access databases via [model context protocol (MCP)](https://modelcontextprotocol.io/docs/getting-started/intro). With version `0.46.0` of MP's API client, `mp_api`, an MCP server is included to help with agentic database retrieval.

This guide describes the tools available, in development, and setup for the MP MCP. A "tool" in MCP world simply describes a function which an LLM can use in a task. The inputs and outputs of a tool must (roughly) be JSONable.

## Tools

MP's MCP is designed to be agnostic to the agent using it. Thus we have designed the MCP with key `fetch` and `search` tools which are compatible with [OpenAI's ChatGPT requirements](https://developers.openai.com/api/docs/mcp/#create-an-mcp-server). The `search` tool performs bulk retrieval of materials data from MP, with limited metadata attached to it. The `fetch` tool can then be used to retrieve more detailed information for a single material.

`search` accepts searching either via chemical formula, dash-delimited chemical system (ex: `"Na-Cl"`), or by keywords. If a formula or chemical system are input, the `materials.summary` collection (the primary data you see on a materials web page) will be queried directly. If keywords are used, the `robocrys` automatically generated crystal description is queried instead.

`fetch` can then be used to aggregate metadata across the `materials.summary`, `materials.similarity`, and `materials.robocrys` endpoints. Specifically, crystallographic and thermodynamic information from `summary`, crystallographically similar materials from `similarity`, and `robocrys` automatically generated descriptions are aggregated.

If submitting a Materials Project ID, `fetch` will return that document, if it exists.
`fetch` also accepts a chemical system or formula, and will return only the most stable result in that space.

### In-development

The next release of the API client will also include the following tools:
- `fetch_all`: to retrieve detailed metadata for all materials in MP
- `fetch_many`: to retrieve up to 100 detailed entries from MP
- `get_phase_diagram_from_elements`: to obtain phase diagram information from MP

As well as a CLI tool `mpmcp` which will locally deploy an MCP server.

## Setup

The MP MCP is based on [`fastmcp`](https://gofastmcp.com/getting-started/welcome). You will also need [`uv`](https://docs.astral.sh/uv/) to handle package management for the MCP's environment.

To start, run from a shell:
```console
git clone https://github.com/esoteric-ephemera/mp_api.git
cd mp_api
git checkout v0.46.0
pip install -e '.[mcp]'
```

In the following, ensure you have `$MP_API_KEY` set as an environment variable. Assume these commands are run from the `mp_api` directory.

The following setup guides are in alphabetical order. These specific examples have been tested by the Materials Project staff and should not be construed as an endorsement of any particular architecture, model, corporation, etc.

### Anthropic Claude Desktop

```console
fastmcp install claude-desktop $(pwd)/mp_api/mcp/server.py --project $(pwd) --python 3.12 --env MP_API_KEY=$MP_API_KEY
```

You may need to initialize the JSON file containing the MCP server configurations. For MacOS users, run this:

```console
echo '{"mcpServers": {}}' > ~/Library/Application\ Support/Claude/claude_desktop_config.json
```

Windows and Linux users will need to modify the path to their desktop application.

### Google Gemini CLI

```console
fastmcp install gemini-cli $(pwd)/mp_api/mcp/server.py --project $(pwd) --python 3.12 --env MP_API_KEY=$MP_API_KEY
```

### OpenAI Codex

To modify only the local directory and therefore only access the MCP once in the API client code directory, run this code block:
```console
mkdir .codex
printf "[mcp_servers.materials_project_mcp]\ntype = \"command\"\ncommand = \"uv\"\nargs = [\n  \"run\",\n  \"--extra\",\n  \"mcp\",\n  \"--directory\",\n  \"$(pwd)\",\n  \"python\",\n  \"-m\",\n  \"mp_api.mcp.server\",\n]\nenv = {\"MP_API_KEY\" = \"$MP_API_KEY\"}" > .codex/config.toml
```

If you instead want to use the MCP with your Codex environment generally, append to the configuration file at your home:
```console
printf "\n[mcp_servers.materials_project_mcp]\ntype = \"command\"\ncommand = \"uv\"\nargs = [\n  \"run\",\n  \"--extra\",\n  \"mcp\",\n  \"--directory\",\n  \"$(pwd)\",\n  \"python\",\n  \"-m\",\n  \"mp_api.mcp.server\",\n]\nenv = {\"MP_API_KEY\" = \"$MP_API_KEY\"}" >> ~/.codex/config.toml
```
