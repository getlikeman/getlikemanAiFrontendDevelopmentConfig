# getlikemanAiDevelopmentConfig

My basic skills and mcp setup for react front-end developing 

## Included MCP server

The current MCP configuration exposes a local Figma Desktop MCP endpoint and the official MCP memory server:

| Server   | Reference                                                                                                                       |
|----------|---------------------------------------------------------------------------------------------------------------------------------|
| `Figma`  | Connects the agent to the local Figma Desktop MCP bridge for design context, screenshots, assets, and related Figma workflows. | https://help.figma.com/hc/en-us/articles/35281186390679-Figma-MCP-collection-How-to-set-up-the-Figma-MCP-server-for-desktop |
| `memory` | Adds persistent local knowledge-graph memory via the official MCP memory server package.                                        | https://github.com/modelcontextprotocol/servers/tree/main/src/memory |

Current `mcp.json`:

```json
{
  "mcpServers": {
    "Figma": {
      "url": "http://127.0.0.1:3845/mcp"
    },
    "memory": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-memory"
      ]
    }
  }
}
```

Optional custom memory file path:

```json
{
  "mcpServers": {
    "memory": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-memory"
      ],
      "env": {
        "MEMORY_FILE_PATH": "/path/to/custom/memory.jsonl"
      }
    }
  }
}
```

## Bundled skills

| Skill                              | Local path                                | Purpose                                                                                                      | Public URL                                                                                 |
|------------------------------------|-------------------------------------------|--------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------|
| `agent-browser`                    | `skills/agent-browser`                    | Browser automation CLI for navigation, form filling, screenshots, scraping, and web app testing.             | https://skills.sh/vercel-labs/agent-browser/agent-browser                                  |
| `figma`                            | `skills/figma`                            | Base Figma MCP workflow for pulling design context, screenshots, variables, and assets into agent workflows. | https://skills.sh/openai/skills/figma                                                      |
| `figma-create-design-system-rules` | `skills/figma-create-design-system-rules` | Generates project-specific design system rules for Figma-to-code workflows.                                  | https://developers.figma.com/docs/figma-mcp-server/skill-figma-create-design-system-rules/ |
| `figma-implement-design`           | `skills/figma-implement-design`           | Converts Figma designs into production-ready code with a structured design-to-code workflow.                 | https://developers.figma.com/docs/figma-mcp-server/skill-figma-implement-design/           |
| `find-docs`                        | `skills/find-docs`                        | Uses Context7 to retrieve current library and framework documentation.                                       | https://github.com/upstash/context7                                                        |
| `find-skills`                      | `skills/find-skills`                      | Helps discover and install additional skills from the skills ecosystem.                                      | https://skills.sh/vercel-labs/skills/find-skills                                           |
| `vercel-react-best-practices`      | `skills/react-best-practices`             | React and Next.js performance and architecture guidance from Vercel Engineering.                             | https://skills.sh/vercel-labs/agent-skills/vercel-react-best-practices                     |

## Installation

### Requirements
- Figma Desktop installed, signed in, and configured to expose the local MCP endpoint at `http://127.0.0.1:3845/mcp`.
- The Figma Desktop app running when you want to use the `Figma` MCP server.
- Node.js 18+ with `npm` or `npx` available for CLI-backed skills and the `memory` MCP server.

1. Install Figma Desktop and enable the Figma Desktop MCP server so the local endpoint `http://127.0.0.1:3845/mcp` is available.
2. Copy or merge the contents of [`mcp.json`](./mcp.json) into the MCP configuration used by your client. If your client supports project-local MCP files, you can keep this file in the repository root as-is.
3. Install the external packages required by the skills you want to use.

   For `find-docs` / Context7:

   ```bash
   npm install -g ctx7
   ctx7 login
   ```
   For `agent-browser`:

   ```bash
   npm install -g agent-browser
   agent-browser install
   agent-browser --version
   ```

4. Make the bundled skills available to your agent:
   - keep this repository in place and point your tool to the local `skills/` directory, or
   - copy the specific skill folders you want into the skill directory used by your agent.
5. Restart the IDE or agent so it reloads MCP servers and local skills.
6. Verify setup by checking that the `Figma` and `memory` MCP servers are available, `ctx7 --version` works, `agent-browser --version` works, and the desired skills appear in the agent's skill list.

## Recommended usage

- Use `figma` and `figma-implement-design` together for design-to-code work.
- Use `figma-create-design-system-rules` when you want repeatable Figma implementation rules in `AGENTS.md`, `CLAUDE.md`, or Cursor rules.
- Use `find-docs` before answering library-specific API or setup questions.
- Use `agent-browser` for browser automation tasks that require navigation or interaction.
- Use `vercel-react-best-practices` during React or Next.js implementation and review.
- Use `memory` when you want the agent to persist user preferences, entities, and observations across sessions.
