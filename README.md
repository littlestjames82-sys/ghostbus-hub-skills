# GhostBus Hub Skills

The public home of the free skill from the GhostBus Hub catalog, plus
the catalog itself in machine-readable form.

## Free skill in this repo

**Agent Seatbelt Guardrails** (free, v0.3.2), in
`agent-seatbelt-guardrails/`: a deterministic policy gate between an
agent's decision and the tool call. It works with any host that
supports the SKILL.md format, including the open `skills` CLI.

Install it with the standard skills CLI (tested Nov 4, 2026):

    npx skills add littlestjames82-sys/ghostbus-hub-skills

## The full catalog

`catalog.json` lists all 110 first-party skills from the Hub: ids,
versions, prices, categories, blurbs, file counts, and full bundle hashes,
so anyone can hash a bundle and verify it exactly against this
catalog. The registry behind it is hash-verified on every run of
the Hub's 135-check gate.

The other 109 skills are sold through the Hub, where every listing
carries a dated vetting record, its full file list, a preview from
the skill itself, and the bundle hash:

https://muse.ai/s/ghostbus-hub-da74krxm7h3xn/0kAjDkV3_AmPSjUaFmLWeUHZv7boE__UsAFG7frI7-g

## Sources and repositories

Every skill is first-party, written and maintained by Ghost
Developer Studio, and every listing's public record (file list,
bundle size, and full bundle hash) lives in `catalog.json` here.

- The free **Agent Seatbelt Guardrails** package is published in
  full in this repository, under `agent-seatbelt-guardrails/`.
- The two Seatbelt skills come from the **Agent Seatbelt** project:
  https://github.com/littlestjames82-sys/agent-seatbelt
- The paid packages are not published. Their full text is delivered
  with purchase, and this catalog is their public record, so what
  was vetted and listed can still be checked from the outside.

## Verify a bundle yourself

Every bundle hash in `catalog.json` is computed the same way, and
anyone can reproduce it from a package's files:

1. Take the package's text files (the ones listed for the skill in
   `catalog.json`), sorted by their relative paths.
2. Feed a sha256 hasher, for each file in that order: the relative
   path as UTF-8, one zero byte, the file's bytes, one zero byte.
3. The hex digest is the bundle hash. If it matches the catalog,
   the bundle is exactly what was vetted and listed.

## About

GhostBus Hub is Ghost Developer Studio's curated marketplace for
agent skills: 110 skills, all first-party for now, every package
scanned before listing with a human reading the findings, and terms
published at a 20% commission (the seller receives 80%).
