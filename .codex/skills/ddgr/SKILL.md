---
name: ddgr
description: Use this skill when you want Codex to search the web through the local `ddgr` CLI instead of built-in browsing. Best for terminal-native DuckDuckGo searches, repeatable CLI recipes, JSON search output for downstream parsing, site/time/region-restricted queries, bang-based lookups, and workflows where `ddgr` finds candidate URLs before another tool such as `playwright-cli` investigates pages in detail.
---

# ddgr

Use `ddgr` for terminal-first web search when the task benefits from local CLI execution, reproducible flags, or JSON output that can be filtered further in-shell.

Treat `ddgr` as the discovery layer. Use it to find candidate URLs quickly, then hand selected URLs to a browser automation or page-inspection tool such as `playwright-cli` when the task requires reading page contents, following navigation, or validating page behavior.

## When To Use

- The user explicitly wants `ddgr` or DuckDuckGo via CLI.
- You need search results from a local command rather than the built-in web tool.
- You want machine-friendly output with `--json --np`.
- You need DuckDuckGo-specific features such as bangs, region, time, or site filtering.
- You want to collect likely target URLs first, then inspect the shortlisted pages with another tool.

## Default Workflow

1. Confirm `ddgr` is callable with `ddgr --version` or `ddgr --help` if needed.
2. Prefer non-interactive usage for Codex:
   `ddgr --json --np -n 5 <query>`
3. Add filters only as needed:
   `-w <site>`, `-t <d|w|m|y>`, `-r <region>`, `-x`, `--unsafe`.
4. If the user wants a browser-open action, use `-j`, `--gb`, or `--url-handler`.
5. Summarize the result set clearly because terminal command output is not shown directly to the user.
6. If deeper inspection is needed, pass only the most relevant URLs to the next tool rather than forwarding the whole result set.

## Recommended Patterns

### Structured output

Use JSON by default when you need to inspect or summarize results:

```bash
ddgr --json --np -n 5 "query terms"
```

This is the preferred handoff format when another tool will consume the results.

### Human-readable one-shot search

Use plain text only when the user wants terminal-style results:

```bash
ddgr --np -n 5 "query terms"
```

Note: plain output can include DuckDuckGo instant-answer text before normal results, which may be noisy. Prefer `--json` when reliability matters.

### Common filters

```bash
ddgr --json --np -n 5 -w example.com "query"
ddgr --json --np -n 5 -t m "query"
ddgr --json --np -n 5 -r jp-jp "query"
ddgr --json --np -n 5 -x "query"
```

## Tool Chaining

When a search should lead to deeper page inspection:

1. Run `ddgr --json --np` with a narrow query.
2. Select 1 to 5 likely URLs based on title, domain, and abstract.
3. Hand those URLs to the next tool.

Use `playwright-cli` when you need to:

- open the shortlisted pages
- inspect rendered content instead of snippets
- click through navigation
- verify page text, forms, tables, or dynamic behavior

Avoid sending raw `ddgr` plain-text output into downstream tooling. Prefer parsed URLs from `--json`.

## Interaction Guidance

- `--np` avoids the omniprompt and is usually the right choice for Codex.
- `-n` accepts `0..25`; keep it small unless the user asks for breadth.
- `-j` opens the first result and implies `--np`.
- `--json` also implies `--np`.
- For multi-tool workflows, keep the initial search broad enough to discover candidates but narrow enough that the handoff set stays small.
- Bang queries such as `!w` may need escaping in some shells: `\!w`.
- If `BROWSER` points to a text browser, `ddgr` can integrate with it; use `O` in interactive mode for GUI-browser intent.

## Caveats

- `ddgr` output reflects DuckDuckGo HTML results, not the built-in web tool.
- Results may differ by region, safe-search, proxy, and user agent settings.
- For reproducible automation, include explicit flags rather than relying on environment defaults.

## References

- For command recipes and practical notes, read [references/recipes.md](references/recipes.md).
