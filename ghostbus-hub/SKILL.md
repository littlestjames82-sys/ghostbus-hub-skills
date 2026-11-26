---
name: ghostbus-hub
description: The GhostBus Hub catalog index by Ghost Developer Studio — 200 vetted agent skills (1 free, 199 paid), every skill scanned twice with a dated vetting record and a published bundle hash. Use when looking for a vetted agent skill, checking what the Hub carries, or verifying a Hub listing against the public catalog record before buying.
version: "1.0.0"
license: MIT
platforms: [openclaw, mcp, claude, claudecode, openai, cursor, codex, opencode]
metadata:
  author: Ghost Developer Studio
  source: https://ghostdeveloperstudio.com/hub/
---

# GhostBus Hub — catalog index

GhostBus Hub is the skills marketplace of Ghost Developer Studio.
It carries 200 vetted agent skills: 1 free, 199 paid. Every skill is
scanned twice before listing, carries a dated vetting record, and is
published with a bundle hash so a buyer can verify that what they
downloaded is exactly what was vetted.

**Storefront:** https://ghostdeveloperstudio.com/hub/

## What this skill is

This is the Hub's index skill. It points at the catalog; it does not
bundle the paid skills. Paid skill bodies are sold and delivered
through the storefront above and are not in this repository.

## The public record

This repository is the public record of the catalog:

- `catalog.json` — the whole catalog in machine-readable form: every
  listing's title, version, price, bundle hash, and vetting status,
  so anyone can check a listing from the outside.
- `agent-seatbelt-guardrails/` — the Hub's one free skill, published
  in full in this repository.

## How vetting works

A skill is scanned twice with independent passes before it lists.
The vetting record is dated, and the published bundle hash is fixed
at listing time. If a bundle changes, its hash changes, and the
record no longer matches — that mismatch is the signal not to run it.

## Using the Hub

1. Browse the catalog at https://ghostdeveloperstudio.com/hub/ or
   read `catalog.json` in this repository.
2. Start with the free skill (Agent Seatbelt Guardrails, in this
   repository) to see the format and the vetting record style.
3. For a paid skill, buy it at the storefront, then compare the
   downloaded bundle's hash against the hash in `catalog.json`
   before installing it.
