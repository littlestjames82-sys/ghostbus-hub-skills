# GhostBus Hub Skills

[![Hub skills](https://img.shields.io/badge/dynamic/json?url=https%3A%2F%2Fraw.githubusercontent.com%2Flittlestjames82-sys%2Fghostbus-hub-skills%2Fmain%2Fcatalog.json&query=%24.skills.length&label=Hub%20skills&color=blue)](https://ghostdeveloperstudio.com)
[![Free skill](https://img.shields.io/badge/free%20skill-Agent%20Seatbelt%20Guardrails-brightgreen)](https://github.com/littlestjames82-sys/ghostbus-hub-skills/blob/main/agent-seatbelt-guardrails/SKILL.md)
[![Hub gate](https://img.shields.io/badge/Hub%20gate-135%20of%20135%20checks-blue)](https://ghostdeveloperstudio.com)
[![Storefront](https://img.shields.io/badge/storefront-ghostdeveloperstudio.com-orange)](https://ghostdeveloperstudio.com)
[![Last commit](https://img.shields.io/github/last-commit/littlestjames82-sys/ghostbus-hub-skills)](https://github.com/littlestjames82-sys/ghostbus-hub-skills/commits/main)

Vetted agent skills from Ghost Developer Studio, sold through the
GhostBus Hub. This repo is the public record of that catalog: the one
free skill published in full, and the whole catalog in
machine-readable form, so anyone can check a listing from the outside.

**Live storefront:** https://ghostdeveloperstudio.com
**Contact:** littlestjames82@gmail.com

**On this page:** [The catalog](#the-catalog) ·
[Free skill](#free-skill-in-this-repo) ·
[Featured skills](#featured-skills) ·
[Start in 60 seconds](#start-in-60-seconds) ·
[How vetting works](#how-vetting-works) ·
[What's in this repo](#whats-in-this-repo) ·
[How buying works](#how-buying-works) ·
[Verify a bundle yourself](#verify-a-bundle-yourself) ·
[Sell on the Hub](#sell-on-the-hub) · [FAQ](#faq) ·
[Related projects](#related-projects) · [About](#about)

## The catalog

Snapshot, Nov 10, 2026: **158 skills, 1 free and 157 paid** (paid
skills are $12, $15, or $19), 1,448 bundle files across 31 categories,
$2,135 if bought separately. The catalog is growing in waves, so do
not trust the snapshot count: `catalog.json` in this repo is
regenerated from the Hub registry and republished at every wave, and
the registry behind it is hash-verified on every run of the Hub's
135-check, 14-suite gate.

`catalog.json` lists every skill's id, version, price, category,
blurb, full file list, and full bundle hash, so anyone can hash a
delivered bundle and verify it exactly against this catalog.

Every listing on the storefront carries the same public record: a
dated dual-scan vetting record, its full file list, a preview taken
from the skill itself, a worked example, and the bundle hash.

For machines: fetch the catalog directly at
https://raw.githubusercontent.com/littlestjames82-sys/ghostbus-hub-skills/main/catalog.json
The badge at the top of this page reads its skill count from that
file, so what it shows is what the catalog holds right now.

Each entry in `catalog.json` carries these fields:

| Field | What it holds |
| --- | --- |
| `id` | The skill's stable identifier, also its directory name in the Hub registry. |
| `title`, `version` | Display name and the version this listing sells. |
| `price_usd`, `pricing` | Price in dollars, and the pricing band (`free` or the paid tier). |
| `category` | The catalog lane the skill sits in (see the category table below). |
| `blurb` | The listing's one-line outcome statement. |
| `files`, `file_list` | File count, and every file path in the package. |
| `bundle_bytes` | Total size of the package's files. |
| `bundle_hash` | The full sha256 bundle hash, computed by the recipe in "Verify a bundle yourself". |
| `vetted` | The dated dual-scan vetting record for this exact bundle. |
| `composition` | File counts by top-level folder in the package. |
| `example_file` | The package's worked example, where it has one. |
| `source` | Seller identity, and whether the package itself is published here. |

### Browse by category

Counts from the Nov 10, 2026 snapshot (158 skills). The live catalog
on the storefront is always the current one.

| Category | Skills | Category | Skills |
| --- | ---: | --- | ---: |
| Agent Operations | 22 | Local Services / Field Operations | 18 |
| Data, Files, and Research | 18 | Agent Safety and Governance | 14 |
| Local Services / Money | 15 | Creator and Audience | 11 |
| Studio / Publishing | 10 | Studio / Shipping | 10 |
| Local Services / Marketing | 6 | Local Services / Sales | 9 |
| Local Services / Reputation | 5 | Agent Comms (Email, Marketplace, Phone) | 3 |
| Security (Code Review, DevOps, Guardrails) | 3 | Agent Ops (10 focused lanes) | 10 |
| Local Services / Quoting and Scoping | 2 | Agent Skills / Authoring | 1 |
| Transport / Load Safety | 1 |  |  |

## Free skill in this repo

**Agent Seatbelt Guardrails** (free, v0.3.2), in
`agent-seatbelt-guardrails/`: a deterministic policy gate between an
agent's decision and the tool call. It works with any host that
supports the SKILL.md format, including the open `skills` CLI.

Install it with the standard skills CLI (tested Nov 4, 2026):

    npx skills add littlestjames82-sys/ghostbus-hub-skills

## Featured skills

A few listings that show the range of the catalog. Every blurb below
is the listing's own, taken from `catalog.json`.

| Skill | Price | What it does |
| --- | ---: | --- |
| Agent Seatbelt Guardrails | Free | Your agent stops asking permission for the safe calls, and can never quietly make the dangerous ones. |
| Seatbelt Policy Authoring | $19 | A policy that fits your project, written from your answers, with every risky rule proven in check mode before it can block anything real. |
| Context Cost Control | $15 | The same agent, doing the same work, on a fraction of the context. |
| Verified Research | $15 | Every figure you act on has been traced to the source that actually said it. |
| Agent Daily Briefing | $12 | One message each morning that earns its place, or a deliberate silence. |
| Marketplace Replies | $12 | Every inquiry gets a fast, honest reply, and you still send each one yourself. |
| Load Safety Photo Check | $12 | Eight checks on a load photo, each closed honestly: pass, check this, or could not tell. |

## Start in 60 seconds

Install the free skill:

    npx skills add littlestjames82-sys/ghostbus-hub-skills

Then query the catalog like data, because it is data:

    # how many skills are listed right now
    jq '.skills | length' catalog.json

    # everything in one category
    jq '.skills[] | select(.category == "Agent Operations") | .title' catalog.json

    # the bundle hash of one skill, to verify a delivery against
    jq -r '.skills[] | select(.id == "context-cost-control") | .bundle_hash' catalog.json

## How vetting works

No skill lists on a promise. Before a listing goes live, its package
is scanned two ways (the studio scanner and gitleaks), a human reads
the findings, and the listing publishes the dated result of that
scan next to the bundle hash. The registry behind the catalog is
also hash-verified on every run of the Hub's own gate: 135 checks
across 14 suites. If a package changes, its bundle hash changes, and
the catalog here is what proves which version was vetted.

## What's in this repo

| Path | What it is |
| --- | --- |
| `catalog.json` | The full Hub catalog in machine-readable form: ids, versions, prices, categories, blurbs, file lists, and full bundle hashes. |
| `agent-seatbelt-guardrails/SKILL.md` | The free Agent Seatbelt Guardrails skill, published in full. |
| `README.md` | This page. |

That is the whole tree, on purpose. The paid packages are **not**
published here: a public repo would make their full text free. Their
public record lives in `catalog.json`, and the packages themselves
are delivered with purchase.

## How buying works

Paid skills are bought through the storefront at
https://ghostdeveloperstudio.com. Today that means Cash App: pay
$RyanCotten2 the skill's exact listed price, put the skill name and
your delivery email in the payment note, then email
littlestjames82@gmail.com from that same address so the payment can
be matched. Each payment is confirmed by hand, and the skill is then
delivered by email: the bundle plus a license key.

License terms, in short: the license is permanent, personal, and
non-transferable, and it covers the version bought. Refunds are
goodwill, per the Hub terms published on the storefront. Card
checkout opens at launch; Cash App is the way to buy today.

## Verify a bundle yourself

Every bundle hash in `catalog.json` is computed the same way, and
anyone can reproduce it from a package's files:

1. Take the package's text files (the ones listed for the skill in
   `catalog.json`), sorted by their relative paths.
2. Feed a sha256 hasher, for each file in that order: the relative
   path as UTF-8, one zero byte, the file's bytes, one zero byte.
3. The hex digest is the bundle hash. If it matches the catalog,
   the bundle is exactly what was vetted and listed.

## Sell on the Hub

Every skill here is first-party for now, written and maintained by
Ghost Developer Studio. Third-party seller terms are published on
the storefront as final v1.1: the Hub keeps a 20% commission and the
seller receives 80%, and every submission is scanned and reviewed
before it can list. Public submissions open at launch; questions go
to littlestjames82@gmail.com.

## FAQ

**Why are the paid packages not in this repo?**
A public repo would make their full text free. Their public record
(ids, prices, file lists, bundle hashes, vetting records) lives in
`catalog.json`, and the packages are delivered with purchase.

**How do I know a delivered bundle is the vetted one?**
Recompute its bundle hash with the recipe in the "Verify a bundle
yourself" section of this README and compare it to `catalog.json`.
A match means the bytes are exactly what was scanned and listed.

**What does the license cover?**
The version you bought, permanently, for your own use. It is
personal and non-transferable. The full buyer terms are on the
storefront.

**What if a skill is not right for me?**
Refunds are goodwill, handled personally by email, per the Hub
terms. Write to littlestjames82@gmail.com.

**Can I sell my own skills here?**
Third-party submissions open at launch, on the published terms:
20% commission, scanning and review before anything lists. See
"Sell on the Hub" above.

## Related projects

- **Agent Seatbelt**, the governed tool-call layer the two Seatbelt
  skills come from: https://github.com/littlestjames82-sys/agent-seatbelt
- **GhostBus**, the agent message bus and MCP server the Hub runs
  on: https://github.com/littlestjames82-sys/ghostbus
- **Ghost Hands**, the governed browser tool:
  https://github.com/littlestjames82-sys/ghost-hands

## About

GhostBus Hub is Ghost Developer Studio's curated marketplace for
agent skills. Ghost Developer Studio is a one-person studio in
Oakdale, Tennessee, building developer tools, apps, and AI systems
since 2022. The studio's site is https://ghostdeveloperstudio.com.
