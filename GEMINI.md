# Valency Skills Extension

This extension provides seven research-corpus skills (profile, landscape,
similar, trends, network, reading-list, fresh-collaborators) for Gemini CLI.
The skills orchestrate multi-step Valency MCP tool chains into structured
research workflows.

## MCP server

This extension does **not** declare the Valency MCP server itself. Tool calls
go through the `valency` MCP server registered by the companion **Valency
connector extension**, which handles OAuth authentication.

If a skill is invoked but no `valency` MCP server is registered, surface the
failure plainly and tell the user to install the connector and authenticate:

```
gemini extensions install https://github.com/valencyio/valency-gemini-extension
/mcp auth valency
```

Do not fabricate results when the MCP server is unreachable.

## Rules

- Never fabricate paper titles, authors, abstracts, or metadata. All paper data
  must come from MCP tool results.
- If an MCP tool returns no results, say so plainly and suggest the user try a
  different query.
- Produce clean, scannable output with consistent markdown formatting.
- When suggesting follow-up commands, use the `/valency:<command>` format.
