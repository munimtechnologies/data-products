# Datasheet: self-hosted AI stack vulnerability feed

## Summary
A curated list of security advisories for self-hosted AI applications, mapped to the exact version ranges they affect. It answers one question: does the version I run have a known advisory, and which release fixes it?

## Coverage (as of 2026-10-14)
- **Apps (12):** Ollama, Open WebUI, vLLM, Langflow, Flowise, LiteLLM, n8n, Dify, RAGFlow, AnythingLLM, ComfyUI, MCP Inspector.
- **Entries:** 130 version-range entries in advisories.json, plus 14 advisories in pending-advisories.json (see "Pending" below). An advisory whose fix was released on several release lines gets one entry per line.
- **Not in public databases:** 25 entries in advisories.json are marked `inPublicDatabases: false`. This means OSV.dev had no record of the advisory when it was checked, so most dependency scanners would not report it. 12 of the 14 pending advisories had no OSV.dev record on 2026-10-14 either.
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
- `check_freshness.py` in the feed's repository automates the first step of a sweep. It pages through each project's published GitHub security advisories and lists every one whose GHSA and CVE IDs appear nowhere in the feed. It also says whether the feed's existing entries already flag the advisory's whole affected range. It uses only the Python standard library: `python check_freshness.py` (set `GITHUB_TOKEN` for a higher rate limit).
- Between sweeps, advisories that are found but not yet merged go into `pending-advisories.json`, in the same entry format. They are also listed below, so you can check them yourself.

## Pending: found on 2026-10-14, not yet in advisories.json
A check of every covered project's published GitHub security advisories (all 12 apps) found 14 advisories whose IDs are not in advisories.json.

**What a scan misses today.** For 10 of the 14, other entries in advisories.json (mostly the vendors' batch entries) already flag every affected version. Only the advisory ID is missing, so a version scan already gives the right answer. These are the 4 where a scan does not yet flag every affected version:

| App | Advisory | Severity | Affected | Fixed in | Versions not flagged yet |
|---|---|---|---|---|---|
| MCP Inspector | [GHSA-g9hg-qhmf-q45m](https://github.com/modelcontextprotocol/inspector/security/advisories/GHSA-g9hg-qhmf-q45m) (CVE-2025-58444): connecting to an untrusted MCP server allows cross-site scripting through a returned redirect URL, which through the built-in proxy can lead to command execution | HIGH | <0.16.6 | 0.16.6 | 0.14.1 to 0.16.5 |
| AnythingLLM | [GHSA-r872-gr59-vf5w](https://github.com/Mintplex-Labs/anything-llm/security/advisories/GHSA-r872-gr59-vf5w) (CVE-2026-55611): the fix for GHSA-p5rf-8p88-979c is incomplete, so in multi-user mode a manager or admin can delete other users' parsed files | LOW | releases with the GHSA-p5rf fix (1.11.2 and later) | none named | 1.16.2 and later |
| n8n | [GHSA-2r5r-xgvc-rj4p](https://github.com/n8n-io/n8n/security/advisories/GHSA-2r5r-xgvc-rj4p): a user with view access to another project's Agents can pull an agent into their own project by its ID | MODERATE | <2.41.4; 2.42.0 | 2.41.4, 2.42.1 | 1.x from 1.123.83 on (the published range takes in all 1.x releases and names no 1.x fix) |
| n8n | [GHSA-p3pg-xw4f-m72c](https://github.com/n8n-io/n8n/security/advisories/GHSA-p3pg-xw4f-m72c): a project member can resume another user's agent run, read their private agent chats and approve their pending tool call | MODERATE | <2.41.4 | 2.41.4, 2.42.0 | 1.x from 1.123.83 on (same reason) |

**Missing IDs only** (affected versions already flagged):

| App | Advisory | Severity | Affected | Fixed in |
|---|---|---|---|---|
| vLLM | [GHSA-34qw-q2h2-jp3m](https://github.com/vllm-project/vllm/security/advisories/GHSA-34qw-q2h2-jp3m): crafted multimodal fields sent to the disaggregated generate endpoint can crash the engine | MODERATE | >=0.20.0, <0.30.0 | 0.30.0 |
| vLLM | [GHSA-w2v2-5662-8v5q](https://github.com/vllm-project/vllm/security/advisories/GHSA-w2v2-5662-8v5q): Rust gRPC server accepts out-of-vocabulary prompt token IDs | MODERATE | >=0.22.0, <0.24.0 | 0.24.0 |
| vLLM | [GHSA-6pgw-f3pv-4h8q](https://github.com/vllm-project/vllm/security/advisories/GHSA-6pgw-f3pv-4h8q): Rust gRPC server bypasses logprob token limits | MODERATE | >=0.22.0, <0.24.0 | 0.24.0 |
| vLLM | [GHSA-rwfw-xvg3-3937](https://github.com/vllm-project/vllm/security/advisories/GHSA-rwfw-xvg3-3937): Rust gRPC server bypasses max_logprobs | MODERATE | >=0.22.0, <0.24.0 | 0.24.0 |
| vLLM | [GHSA-6cxc-2vcg-w5qc](https://github.com/vllm-project/vllm/security/advisories/GHSA-6cxc-2vcg-w5qc): another client's token allowlist can stay on a recycled batch row | LOW | <0.31.0 | 0.31.0 |
| Open WebUI | [GHSA-whgc-vpjx-m3qh](https://github.com/open-webui/open-webui/security/advisories/GHSA-whgc-vpjx-m3qh): automation recurrence rules can stall the whole instance (only where non-admins have the automations permission, which is off by default) | MODERATE | >=0.11.0, <0.11.4 | 0.11.4 |
| Open WebUI | [GHSA-jx3w-3pqq-7f3w](https://github.com/open-webui/open-webui/security/advisories/GHSA-jx3w-3pqq-7f3w): folder names and owners of knowledge bases you can't access are disclosed through a known directory ID | MODERATE | >=0.9.6, <0.11.4 | 0.11.4 |
| n8n | [GHSA-3p2g-2wpm-8h3g](https://github.com/n8n-io/n8n/security/advisories/GHSA-3p2g-2wpm-8h3g): prototype pollution through the AI Workflow Builder's connection merge | MODERATE | <1.123.83; <2.41.4; 2.42.0 | 1.123.83, 2.41.4, 2.42.1 |
| LiteLLM | [GHSA-72m8-9m7m-h278](https://github.com/BerriAI/litellm/security/advisories/GHSA-72m8-9m7m-h278) (CVE-2026-59821): Custom Code Guardrails create and update endpoints skip the code safety checks | LOW | <1.82.0 | 1.82.0-stable |
| Dify | [GHSA-cg9f-q34p-p9h3](https://github.com/langgenius/dify/security/advisories/GHSA-cg9f-q34p-p9h3): SSRF through an API Extension endpoint | LOW | 0.6.7 | 0.6.8 |

A correction to the 2026-10-14 01:20Z version of this page: it listed 9 of these advisories as missing from the feed without saying that 8 of them were already covered by version range. The vLLM, Open WebUI and Dify ones only lack their IDs.

## Access
- **Raw file, free and MIT-licensed:** https://github.com/munimv2/ai-stack-advisories/blob/main/advisories.json
- **Pending file:** https://github.com/munimv2/ai-stack-advisories/blob/main/pending-advisories.json (the Actor, the GitHub Action and `scan_stack.py` don't read it yet)
- **Apify Actor:** https://apify.com/munim-inc/ai-stack-advisories. Give it your app versions and it returns the advisories that affect them.
- **GitHub Action:** `action.yml` in the same repository, for checking a stack in CI.
- **Freshness check:** `check_freshness.py` in the same repository, described above.
- **Managed delivery on request:** CSV or Parquet exports, a change log of new and changed entries, and coverage of extra apps you run. Contact munimversion2@gmail.com.

## Known limitations
- It covers only the 12 apps above.
- Severity is copied from the publisher. It is not re-scored. Some CVE-only entries (four early Open WebUI entries and both ComfyUI entries) have a null severity, although the ComfyUI project's own GitHub advisories rate all four ComfyUI issues HIGH. Their GHSA IDs (GHSA-rvxv-29p8-pxgq, GHSA-53g8-45wq-pcv8, GHSA-pj59-g5vv-74q4, GHSA-rj8c-c4p8-3c5h) are not yet listed either.
- Some summaries merge several advisories that share a fix. Use `ids` to look up each one.
- This is information, not a security audit. Always check the linked source and update to the latest release.
