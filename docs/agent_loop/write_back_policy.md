# Write-Back Policy

Write back only the minimum durable context needed for the next fresh session.

## Always Update

- the active task's `restart_note`

## Update Only If Accepted Facts Changed

- [docs/project/current_state.md](../project/current_state.md)
- linked decision docs
- task status tracking

## Update Only If Sequencing Changed

- [docs/project/critical_path.md](../project/critical_path.md)
- dependency notes
- follow-up task decomposition

## Keep Out Of Write-Back

- long implementation summaries
- raw transcript excerpts
- speculative design notes without accepted impact
- duplicated runbook detail
