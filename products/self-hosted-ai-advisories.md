# Datasheet: self-hosted AI stack vulnerability feed

## Summary
A curated list of security advisories for self-hosted AI applications, mapped to the exact version ranges they affect. It answers one question: does the version I run have a known advisory, and which release fixes it?

## Coverage (as of 2026-10-14)
- **Apps (12):** Ollama, Open WebUI, vLLM, Langflow, Flowise, LiteLLM, n8n, Dify, RAGFlow, AnythingLLM, ComfyUI, MCP Inspector.
- **Entries:** 144 version-range entries in advisories.json. An advisory whose fix was released on several release lines gets one entry per line. pending-advisories.json is empty: everything found in the 2026-10-14 sweep has been merged.
- **Not in public databases:** 36 entries are marked `inPublicDatabases: false`. This means OSV.dev had no record of the advisory when it was checked, so most dependency scanners would not report it.
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
- CVE records (CVE Terms of Use) and NVD.
- OSV.dev.
- National CERT publications (for example, CERT Polska).
- Project release tags, used to check version ranges.

## How it's maintained
- MV2 of Munim, Inc. maintains the feed with AI agents. They sweep each covered project's advisories and compare them with OSV.dev and the release tags.
- Sweeps are run by hand, not on a schedule yet, so a new advisory can take days to reach the file. The `checked` field gives the date of the last full sweep.
- `check_freshness.py` in the feed's repository automates the first step of a sweep. It pages through each project's published GitHub security advisories and lists every one whose GHSA and CVE IDs appear nowhere in the feed. It also says whether the feed's existing entries already flag the advisory's whole affected range. It uses only the Python standard library: `python check_freshness.py` (set `GITHUB_TOKEN` for a higher rate limit). Ollama publishes no GitHub advisories, so its CVEs are checked on NVD by keyword.
- Between sweeps, advisories that are found but not yet merged go into `pending-advisories.json`, in the same entry format, and are listed on this page.

## Change log

### 2026-10-14 (130 to 144 entries)
**Versions now flagged that were not before:**

| App | Advisory | Severity | Newly flagged |
|---|---|---|---|
| MCP Inspector | [GHSA-g9hg-qhmf-q45m](https://github.com/modelcontextprotocol/inspector/security/advisories/GHSA-g9hg-qhmf-q45m) (CVE-2025-58444): connecting to an untrusted MCP server allows cross-site scripting through a returned redirect URL, which through the built-in proxy can lead to command execution | HIGH | 0.14.1 to 0.16.5 (fixed in 0.16.6) |
| Ollama | [CVE-2026-108836](https://nvd.nist.gov/vuln/detail/CVE-2026-108836): SSRF through model pulls (published 12 October 2026; in OSV.dev with a git-commit range only) | null (CVSS 4.0 5.5 from the CNA) | 0.34.0 and 0.34.1 (fixed in 0.34.2) |
| AnythingLLM | [GHSA-r872-gr59-vf5w](https://github.com/Mintplex-Labs/anything-llm/security/advisories/GHSA-r872-gr59-vf5w) (CVE-2026-55611): the fix for GHSA-p5rf-8p88-979c is incomplete, so in multi-user mode a manager or admin can delete other users' parsed files | LOW | 1.16.2 and later (no fixed release named) |
| n8n | [GHSA-2r5r-xgvc-rj4p](https://github.com/n8n-io/n8n/security/advisories/GHSA-2r5r-xgvc-rj4p) and [GHSA-p3pg-xw4f-m72c](https://github.com/n8n-io/n8n/security/advisories/GHSA-p3pg-xw4f-m72c): cross-project agent takeover by ID; a project member can resume another user's agent run | MODERATE | 1.x from 1.123.83 on (the published ranges, < 2.41.4, take in all 1.x releases and name no 1.x fix) |

**IDs added where the versions were already flagged** (no change to scan results): vLLM GHSA-34qw-q2h2-jp3m, GHSA-w2v2-5662-8v5q, GHSA-6pgw-f3pv-4h8q, GHSA-rwfw-xvg3-3937, GHSA-6cxc-2vcg-w5qc; Open WebUI GHSA-whgc-vpjx-m3qh, GHSA-jx3w-3pqq-7f3w; n8n GHSA-3p2g-2wpm-8h3g; LiteLLM GHSA-72m8-9m7m-h278 (CVE-2026-59821); Dify GHSA-cg9f-q34p-p9h3. Each got its own entry with its own conditions (for example, vLLM's gRPC server or disaggregated endpoint only).

**Severity and ID fixes on existing entries:**
- ComfyUI: both entries were severity null with no GHSA IDs. They are now HIGH, as ComfyUI's own advisories rate them, with GHSA-rvxv-29p8-pxgq, GHSA-pj59-g5vv-74q4, GHSA-53g8-45wq-pcv8 and GHSA-rj8c-c4p8-3c5h.
- Open WebUI: five CVE-only entries now carry the GitHub-rated severity and GHSA ID (CVE-2026-44721 and CVE-2026-45349 HIGH; CVE-2026-45317 MODERATE; CVE-2026-54010 HIGH; CVE-2026-70480 MODERATE, whose first affected version is now 0.6.34 as its advisory states; CVE-2026-87011 and CVE-2026-87015 HIGH). GHSA-hm62-64q2-6gxw got CVE-2026-109296 on 12 October 2026, and its ID is added.
- Langflow CVE-2026-33017 (GHSA-vwmf-pq79-vjvx): CRITICAL. Flowise CVE-2026-70474/70472/70471: HIGH, with their GHSA IDs.
- MCP Inspector CVE-2025-49596: GHSA-7f8r-222p-6f5g added.

A correction to an earlier version of this page (2026-10-14 01:20Z): it listed 9 advisories as missing from the feed without saying that 8 of them were already covered by version range. That was corrected the same day.

## Access
- **Raw file, free and Apache-2.0 licensed:** https://github.com/munimv2/ai-stack-advisories/blob/main/advisories.json
- **Apify Actor:** https://apify.com/munim-inc/ai-stack-advisories. Give it your app versions and it returns the advisories that affect them.
- **GitHub Action:** `action.yml` in the same repository, for checking a stack in CI. The data ships with each release, so use the newest release tag.
- **Freshness check:** `check_freshness.py` in the same repository, described above.
- **Managed delivery on request:** CSV or Parquet exports, a change log of new and changed entries, and coverage of extra apps you run. Contact munimversion2@gmail.com.

## Known limitations
- It covers only the 12 apps above.
- Severity is copied from the publisher. It is not re-scored. The seven Ollama entries have a null severity, because Ollama publishes CVEs through other organisations rather than GitHub advisories.
- Some summaries merge several advisories that share a fix. Use `ids` to look up each one.
- This is information, not a security audit. Always check the linked source and update to the latest release.
