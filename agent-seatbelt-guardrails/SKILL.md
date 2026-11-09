---
name: agent-seatbelt-guardrails
description: Live safely with Agent Seatbelt guardrails on a coding agent. Use when Agent Seatbelt gates, denies, or asks about a tool call; when checking what Seatbelt would decide for a command before running it; when reading a Seatbelt denial, audit report, or flight plan; or when installing the free, open-source Agent Seatbelt plugin for Claude Code, Cursor, Copilot, Codex, and other agents.
license: MIT
compatibility: Works with any agent that supports the SKILL.md format. The full Agent Seatbelt gate requires Python 3 and the free plugin from the source repository linked in this skill.
metadata:
  author: Ghost Developer Studio
  version: "0.3.2"
  source: https://github.com/littlestjames82-sys/agent-seatbelt
---

# Living with Agent Seatbelt

Agent Seatbelt is a deterministic policy gate between an agent's
decision and the tool call. It classifies each Bash/PowerShell/file/MCP
call, decides it against locked rules plus the project overlay
(`.seatbelt/policy.json`), and writes an audit record BEFORE the call
runs. It is free and open source (MIT) on GitHub and PyPI as
`ghost-seatbelt`. This skill teaches the agent how to behave around
the gate — the gate itself is the plugin, not this file.

## Install the gate (free)

- Claude Code plugin:
  `claude plugin marketplace add littlestjames82-sys/agent-seatbelt`
  then `claude plugin install agent-seatbelt@agent-seatbelt`
- Or PyPI: `pip install ghost-seatbelt`
- Read the installer before running it. Installing a security tool
  you have not read is how incidents happen.

## When Seatbelt denies

- The denial is FINAL. Do not evade it: no splitting the command into
  pieces, no quoting/obfuscation tricks, no reaching the same effect
  through python/node instead of the shell, no editing Seatbelt's own
  policy, hook, or agent settings to loosen the gate.
- Read the named rule and the reason — every decision names its rule.
- Follow the remediation in the reason (the "Safer path:") or propose
  an equivalent safer alternative, and tell the user what was stopped
  and why.

## When Seatbelt asks

- The human is being asked because the action is consequential
  (deploy, publish, secret read, persistence change, deletion).
- Present the exact command/target and the blast radius if shown.
  Do not rephrase the command to make it sound smaller than it is.

## When Seatbelt allows or stays silent

- `allow` means the call is on the conservative safe list.
- No output means Seatbelt deferred to the native permission flow —
  that is not an approval; follow the normal permission process.

## First run

Before you rely on the gate, watch it decide once:

1. Take one real tool call you were about to make and let
   the gate see it the normal way. The gate writes its audit
   record before anything runs, so the call is safe to offer
   even if the answer is no. If you would rather stage it,
   preview a call you know should be stopped, like a force
   push, with the plugin's check command instead.
2. Read the verdict where it lands: the decision, the rule
   that made it, and the safer path if the reason offers one.
3. Open `.seatbelt/audit.jsonl` and find the record of that
   decision.

The first run is finished when you have seen one decision
end to end, allowed or denied with its reason, and found
its line in the audit log. From then on the gate is a tool
you have watched work, not a claim you are taking on trust.

## Checking and reporting

- Preview a decision without running anything: use the plugin's check
  command for the exact command string, and report the verdict, the
  rule name, and the safer path. NEVER run the checked command itself.
- For the user, run the report command to show verdict counts, top
  rules, and prompts saved. Records live in `.seatbelt/audit.jsonl`
  with secrets redacted to type+length. Do not edit the audit log.

## Flight plans

If the user has declared a flight plan for the session
(`.seatbelt/plan.json`, announced at SessionStart), work inside it:
the plan covers specific paths, verbs, and resources for a bounded
time, and ask-tier actions inside it proceed without a prompt.

- A plan NEVER covers denials or locked rules — if Seatbelt denies,
  the plan is irrelevant; follow the denial guidance above.
- If an action you need is off-plan, say so and let the human amend
  the plan (they edit `.seatbelt/plan.json`); do not route around
  the boundary.
- Off-plan consequential actions ask in solo mode and are denied
  under ci/paranoid packs — both are the plan working as intended.
