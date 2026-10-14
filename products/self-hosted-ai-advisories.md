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
- Sweeps are run by hand, not on a schedule yet, so a new advisory can take days to weeks to reach the file. The `checked` field gives the date of the last full sweep.
- Between sweeps, advisories that are found but not yet added are listed under "Pending" below, so you can check them yourself.

## Pending: found on 2026-10-14, not yet in advisories.json
A check against each project's published GitHub repository advisories found these 9 advisories missing from the file. They will be added in the next update.

| App | Advisory | Severity | Affected | Fixed in |
|---|---|---|---|---|
| vLLM | [GHSA-34qw-q2h2-jp3m](https://github.com/vllm-project/vllm/security/advisories/GHSA-34qw-q2h2-jp3m): crafted multimodal fields sent to the disaggregated generate endpoint can crash the engine | MODERATE | >=0.20.0, <0.30.0 | 0.30.0 |
| vLLM | [GHSA-w2v2-5662-8v5q](https://github.com/vllm-project/vllm/security/advisories/GHSA-w2v2-5662-8v5q): Rust gRPC server accepts out-of-vocabulary prompt token IDs | MODERATE | >=0.22.0, <0.24.0 | 0.24.0 |
| vLLM | [GHSA-6pgw-f3pv-4h8q](https://github.com/vllm-project/vllm/security/advisories/GHSA-6pgw-f3pv-4h8q): Rust gRPC server bypasses logprob token limits | MODERATE | >=0.22.0, <0.24.0 | 0.24.0 |
| vLLM | [GHSA-rwfw-xvg3-3937](https://github.com/vllm-project/vllm/security/advisories/GHSA-rwfw-xvg3-3937): Rust gRPC server bypasses max_logprobs | MODERATE | >=0.22.0, <0.24.0 | 0.24.0 |
| vLLM | [GHSA-6cxc-2vcg-w5qc](https://github.com/vllm-project/vllm/security/advisories/GHSA-6cxc-2vcg-w5qc): stale allowed_token_ids mask on recycled batch rows | LOW | <0.31.0 | 0.31.0 |
| Open WebUI | [GHSA-whgc-vpjx-m3qh](https://github.com/open-webui/open-webui/security/advisories/GHSA-whgc-vpjx-m3qh): automation recurrence rules can stall the whole instance (only where non-admins have the automations permission, which is off by default) | MODERATE | >=0.11.0, <0.11.4 | 0.11.4 |
| Open WebUI | [GHSA-jx3w-3pqq-7f3w](https://github.com/open-webui/open-webui/security/advisories/GHSA-jx3w-3pqq-7f3w): folder names and owners of knowledge bases you can't access are disclosed through a known directory ID | MODERATE | >=0.9.6, <0.11.4 | 0.11.4 |
| AnythingLLM | [GHSA-r872-gr59-vf5w](https://github.com/Mintplex-Labs/anything-llm/security/advisories/GHSA-r872-gr59-vf5w) (CVE-2026-55611): the fix for GHSA-p5rf-8p88-979c is incomplete, so in multi-user mode a manager or admin can delete other users' parsed files | LOW | releases with the GHSA-p5rf fix (1.11.2 and later) | none listed |
| Dify | [GHSA-cg9f-q34p-p9h3](https://github.com/langgenius/dify/security/advisories/GHSA-cg9f-q34p-p9h3): SSRF through an API Extension endpoint | LOW | 0.6.7 | 0.6.8 |

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
