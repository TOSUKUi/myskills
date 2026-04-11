# ddgr recipes

These recipes are intended for Codex when the user wants local CLI search through DuckDuckGo.

In multi-tool workflows, use `ddgr` to discover targets and a page-level tool such as `playwright-cli` to inspect the targets.

## Baseline checks

```bash
ddgr --version
ddgr --help
```

## Best default for Codex

Use JSON plus no-prompt mode when you need deterministic output that can be summarized or piped into later commands:

```bash
ddgr --json --np -n 5 "openai api responses"
```

Observed locally with `ddgr` 2.2:

- `ddgr --json --np -n 2 openai` returned a JSON array of result objects containing `title`, `url`, and sometimes `abstract`.
- `ddgr --np -n 2 openai` printed an instant-answer block before numbered results, which is harder to parse reliably.

## Handoff pattern for page investigation

Use this sequence when the user wants deeper investigation after search:

1. Discovery with `ddgr`:

```bash
ddgr --json --np -n 5 "site:docs.example.com auth token refresh"
```

2. Select the best URLs from the JSON output.
3. Open those URLs with `playwright-cli` and inspect real page content, links, or interactions.

Practical guidance:

- Keep `-n` small so downstream page inspection stays tractable.
- Prefer site-restricted queries before handing off.
- Use `-x` or JSON URLs if domains are ambiguous.
- Report both the search query and the inspected URLs when reproducibility matters.

Example workflow:

```bash
ddgr --json --np -n 3 -w docs.stripe.com "webhooks signature verification"
```

Then pass the top 1 to 3 URLs to `playwright-cli` for content inspection.

## Query recipes

### Site-restricted

```bash
ddgr --json --np -n 5 -w github.com "jarun ddgr"
```

### Time-restricted

```bash
ddgr --json --np -n 5 -t d "duckduckgo privacy news"
ddgr --json --np -n 5 -t m "python 3.13 release notes"
```

### Region-specific

```bash
ddgr --json --np -n 5 -r us-en "federal reserve"
ddgr --json --np -n 5 -r jp-jp "生成AI"
```

### Expand full URLs

```bash
ddgr --np -n 5 -x "openai"
```

### Unsafe search off/on

Default is safe search on. Disable only when the user explicitly asks:

```bash
ddgr --json --np -n 5 --unsafe "query"
```

## Bang usage

DuckDuckGo bangs can be sent directly:

```bash
ddgr --np "!w terminal"
ddgr --np "\!w terminal"
```

Use the escaped form when the shell treats `!` specially.

## Browser actions

Open the first result:

```bash
ddgr -j "query"
```

Open a bang directly in a GUI browser:

```bash
ddgr --gb --np "!w terminal"
```

Use a custom opener:

```bash
ddgr --url-handler urlhandler --np "query"
```

## Interactive mode

Codex should usually avoid this because it waits for omniprompt input. Use only when the user explicitly wants an interactive terminal session.

Key commands:

- `n`, `p`, `f`: next, previous, first page
- `o 1 2` or `o 1-3`: open selected results
- `O ...`: open in GUI browser
- `d keywords`: new search preserving options
- `x`: toggle URL expansion
- `c 1`: copy URL to clipboard
- `?`: show help
- `q`: quit

## Environment-sensitive behavior

- Proxy: `-p URI` or `HTTPS_PROXY` / `https_proxy`
- Colors: `--colorize`, `-C`, `--colors`, or `DDGR_COLORS`
- Browser logging: add `--show-browser-logs` if a text browser is not whitelisted

## Reporting results back to the user

- The user does not see raw command output, so summarize the top hits.
- Include the exact command when reproducibility matters.
- If you chain into `playwright-cli`, distinguish clearly between what came from search snippets and what you confirmed by opening the page.
- If the query is date-sensitive, state the search date explicitly in your response.
