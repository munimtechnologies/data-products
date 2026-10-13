# Datasheet: self-hosted AI stack vulnerability feed

## Summary
A curated list of security advisories for self-hosted AI applications, mapped to the exact version ranges they affect. It answers one question: does the version I run have a known advisory, and which release fixes it?

## Coverage (as of 2026-10-13)
- **Apps (12):** Ollama, Open WebUI, vLLM, Langflow, Flowise, LiteLLM, n8n, Dify, RAGFlow, AnythingLLM, ComfyUI, MCP Inspector.
- **Entries:** 130 version-range entries. An advisory whose fix was released on several release lines gets one entry per line.
- **Not in public databases:** 25 entries are marked `inPublicDatabases: false`. This means OSV.dev had no record of the advisory when it was checked, so most dependency scanners would not report it.
- **Corrected ranges:** where an advisory's stated range disagrees with the project's release tags or its own patched-versions list, the entry follows the code and explains why in its summary.

## Fields
| Field | Meaning |
|---|---|
| `introduced` | First affected version (`null` = from the start) |
| `fixed` | First fixed version (`null` = no fixed release yet) |
| `lastAffected` | When nothing is fixed yet, the newest version named as affected |
| `only` | When only specific versions are affected, those versions (for example, malicious releases) |
| `kind` | `server`, `feature` (only when an optional feature is used) or `windows` (Windows builds only) |
| `ids` | CVE and GHSA identifiers |
| `severity` | As published (CRITICAL / HIGH / MODERATE / LOW), or null |
| `summary` | A plain-language description of the impact, including conditions and workarounds |
| `url` | The primary source |
| `inPublicDatabases` | `false` when the advisory was missing from OSV.dev at the last check |

## Sources
- Project security advisories on GitHub, and the GitHub Advisory Database (CC BY 4.0).
- CVE records (CVE Terms of Use).
- OSV.dev.
- National CERT publications (for example, CERT Polska).
- Project release tags, used to check version ranges.

## How it's maintained
- MV2 of Munim, Inc. maintains the feed with AI agents. They sweep each covered project's advisories and compare them with OSV.dev and the release tags.
- New advisories have been added within days of publication since October 2026.
- The `checked` field in the file gives the date of the last full check.

## Access
- **Raw file, free and MIT-licensed:** https://github.com/munimv2/ai-stack-advisories/blob/main/advisories.json
- **Apify Actor:** https://apify.com/munim-inc/ai-stack-advisories. Give it your app versions and it returns the advisories that affect them.
- **GitHub Action:** `action.yml` in the same repository, for checking a stack in CI.
- **Managed delivery on request:** CSV or Parquet exports, a change log of new and changed entries, and coverage of extra apps you run. Contact munimversion2@gmail.com.

## Known limitations
- It covers only the 12 apps above.
- Severity is copied from the publisher. It is not re-scored.
- Some summaries merge several advisories that share a fix. Use `ids` to look up each one.
- This is information, not a security audit. Always check the linked source and update to the latest release.
