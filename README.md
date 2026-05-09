# Valency Skills — Research Corpus Workflows for Gemini CLI

A Gemini CLI extension that exposes seven skills for searching, profiling, and
analyzing millions of research papers (arXiv, PubMed, bioRxiv, medRxiv) via the
Valency MCP server. Each skill is a prescribed chain of tool calls — not a
single search — so questions like "what should David Hogg be reading next?" or
"who should he be talking to that he isn't already?" produce a structured,
multi-step answer instead of a raw search dump.

This is a port of the [Claude Code Valency plugin](https://github.com/valency-oss/valency-plugin)
to Gemini CLI's extension format. The skills, tool chains, and output shapes
are identical; the packaging is Gemini-native.

## Prerequisites

This extension depends on the **Valency Gemini connector extension**, which
registers the `valency` MCP server and handles OAuth authentication. Install
and authenticate the connector first:

```bash
gemini extensions install https://github.com/valency-oss/valency-gemini-extension
gemini  # then inside the CLI:
/mcp auth valency
```

Without the connector, the skills have no MCP tools to call.

## What's bundled

- **Skills**: `profile`, `landscape`, `similar`, `trends`, `network`,
  `reading-list`, `fresh-collaborators` — auto-discovered from `skills/` by
  Gemini CLI. The model activates them on demand via `activate_skill`.
- **Slash commands**: `/valency:profile`, `/valency:landscape`,
  `/valency:similar`, `/valency:trends`, `/valency:network`,
  `/valency:reading-list`, `/valency:fresh-collaborators` — thin wrappers in
  `commands/valency/*.toml` that trigger the matching skill.
- **Context file**: `GEMINI.md` — extension-level rules (no fabrication,
  consistent markdown output, MCP-server expectations).

This extension does **not** register an MCP server. The connector does that.

## Installation

### From a local checkout

```bash
gemini extensions link /path/to/valency-gemini-skills
```

`link` symlinks the dev folder into Gemini CLI's extensions directory so edits
are reflected immediately. Restart your Gemini CLI session to pick up new
files.

To install a frozen copy:

```bash
gemini extensions install /path/to/valency-gemini-skills
```

### From a Git repo (once published)

```bash
gemini extensions install https://github.com/valency-oss/valency-gemini-skills
```

## Usage

**TL;DR:** Gemini auto-activates the right skill from natural-language
questions. The slash commands are explicit triggers when you want a specific
workflow.

### Profile — researcher overview

```
/valency:profile Yoshua Bengio
```

Or:

```
Who is Yoshua Bengio?
What does Sara Walker work on?
Show me Daphne Koller's publications
```

### Landscape — field overview

```
/valency:landscape cs.LG
/valency:landscape protein folding
```

### Similar — find related papers

```
/valency:similar 1706.03762
/valency:similar Attention Is All You Need
```

### Trends — topic evolution over time

```
/valency:trends cs.AI
/valency:trends CRISPR
/valency:trends cs.LG, cs.CL
```

### Network — collaboration mapping

```
/valency:network Yoshua Bengio
```

The network skill includes a Divergence Analysis section showing how each top
collaborator's research focus has drifted from the focal author's — useful for
seeing how a researcher's intellectual position has shifted relative to their
closest peers.

### Reading list — curated bibliography for a researcher

```
/valency:reading-list David W. Hogg
```

Triangulates off the author's most-cited and most-recent papers to surface 5–8
recommendations per intellectual thread, with cross-thread papers (those that
connect multiple threads of the author's work) highlighted separately.

### Fresh collaborators — expand a researcher's network

```
/valency:fresh-collaborators David W. Hogg
```

Returns researchers doing recent (last 12–18 months) thematically relevant
work who are **not** in the focal author's top-100 coauthor network. Each
candidate is tagged junior/senior and attributed to the theme(s) that surfaced
them.

## Layout

```
valency-gemini-skills/
├── gemini-extension.json       # Manifest: name, version, contextFileName
├── GEMINI.md                   # Extension-level context
├── package.json                # npm-style metadata (homepage, keywords, etc.)
├── LICENSE                     # MIT
├── README.md
├── commands/
│   └── valency/
│       ├── profile.toml
│       ├── landscape.toml
│       ├── similar.toml
│       ├── trends.toml
│       ├── network.toml
│       ├── reading-list.toml
│       └── fresh-collaborators.toml
└── skills/
    ├── profile/SKILL.md
    ├── landscape/SKILL.md
    ├── similar/SKILL.md
    ├── trends/SKILL.md
    ├── network/SKILL.md
    ├── reading-list/SKILL.md
    └── fresh-collaborators/SKILL.md
```

## License

This repo is MIT-licensed. See [LICENSE](./LICENSE).

Org-wide contribution, trademark, and security policies for the `valency-oss` organization are maintained at [valency-oss/.github](https://github.com/valency-oss/.github).
