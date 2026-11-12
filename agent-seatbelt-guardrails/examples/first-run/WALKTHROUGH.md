# Worked example: the first run

This is the first run the skill itself prescribes, done for
real on Nov 11, 2026 against the real gate (ghost-seatbelt
0.3.2, `seatbelt_hook.py`): one dangerous call staged, its
verdict read, and its record found in the audit log. The
project is a scratch demo created for this run, with no real
code, no remote, and nothing at stake. Nothing below was
executed: the gate only decided. The force push at the
center of this file never ran.

## The setup

The scratch project carries one overlay, `.seatbelt/policy.json`:

```json
{"allow_patterns":["git status"],"ask_patterns":["npm run deploy"],"deny_patterns":[],"description":"Overlay for a scratch demo project used for one Seatbelt first run: git status is routine and allowed, a deploy asks, and the locked rules (including the force-push denial) sit underneath, unreachable by this file.","name":"first-run-demo","rules":[]}
```

The overlay allows one routine command outright and asks
before a deploy. Underneath it sit the locked rules, where
no overlay can loosen anything. That last part is what the
first run is for.

## Step 1: preview the call, run nothing

The skill's staging route is the plugin's check command,
which decides a command string without running it. Three
previews, quoted verbatim with their exit codes:

```text
$ seatbelt_hook.py --check "git push --force origin main"
DENY  rule=seatbelt-locked-git-force-push
  reason: force push rewrites shared history Safer path: Use --force-with-lease, and never force-push a shared branch without telling its other users.
  command: git push --force origin main
exit code: 2
```

The denial names its rule, and the rule is a locked one:
the overlay above could not have waved this through, and
the safer path arrives in the same breath as the stop.

```text
$ seatbelt_hook.py --check "git status"
ALLOW  rule=seatbelt-overlay-allow-pattern
  reason: project overlay allows this command
  command: git status
exit code: 0

$ seatbelt_hook.py --check "npm publish"
ASK  rule=seatbelt-locked-deploy-publish
  reason: deploy / publish / push changes what the world runs Safer path: Deploy a preview/staging build first and confirm the target project before production. Last checkpoint: none this session.
  command: npm publish
exit code: 1
```

One of each tier, all decided by the same gate in the same
project: the overlay's allow for the routine call, and two
locked decisions, an ask and a denial, on the calls that
change what the world runs or rewrites its history. The
overlay never got a vote on either.

One honest limitation of the preview route: check mode
decides and reports, but it writes no audit record. The
record in step 3 exists because the same call was then
offered to the gate the normal way.

## Step 2: offer the same call the normal way

The force push was presented to the hook as an ordinary
PreToolUse event for a Bash call, exactly what a coding
agent's harness sends before a tool runs. The gate writes
its audit record before anything runs, so the call is safe
to offer even when the answer is no. The gate answered:

```json
{"hookSpecificOutput": {"hookEventName": "PreToolUse", "permissionDecision": "deny", "permissionDecisionReason": "Seatbelt: force push rewrites shared history Safer path: Use --force-with-lease, and never force-push a shared branch without telling its other users. (rule: seatbelt-locked-git-force-push). Do not evade this denial; propose a safer alternative."}}
```

Denied, with the rule named and an instruction not to evade
it. The command was never executed; the hook is a decider,
and its decision is the whole event.

## Step 3: find the record

`.seatbelt/audit.jsonl` in the scratch project holds one
line, written by the step 2 decision:

```json
{"ts": "2026-11-12T01:44:20.573487+00:00", "event": "PreToolUse", "session_id": "first-run-2026-11-11", "cwd": "/home/hatch/workspace/scratch-guardrails-first-run", "mode": "enforce", "version": "0.3.2", "agent": "claude", "tool_name": "Bash", "target": {"command": "git push --force origin main"}, "decision": "deny", "rule_id": "seatbelt-locked-git-force-push", "reason": "force push rewrites shared history Safer path: Use --force-with-lease, and never force-push a shared branch without telling its other users.", "remediation": "Use --force-with-lease, and never force-push a shared branch without telling its other users.", "asi": "ASI02", "prev_hash": "GENESIS", "hash": "83173af415c293b1077a586d2cc89c57c7714174031111ed0b3a54616caf4051"}
```

Every field the skill tells you to look for is on that line:
the decision, the deciding rule, the reason, the safer path
as its own remediation field, the gate version, and a hash
chained to the record before it (GENESIS, because this log
starts here). The plugin's report command reads the same
log back: 1 gated call, 1 deny, top rule
seatbelt-locked-git-force-push.

## What this proves, and what it does not

Proven: on Nov 11, 2026, ghost-seatbelt 0.3.2 denied a
force push under a locked rule, asked on a publish under
another locked rule, allowed the routine call the project
overlay names, put the rule and the safer path on every
decision, and left a complete audit record of the one
decision offered through the normal path, all without
executing anything.

Not proven: anything about a real project. The scratch
overlay is three lines of intent, the target repository is
a demo with no remote, and one denial is a first run, not a
track record. The skill asks for exactly this much before
you rely on the gate: one decision, seen end to end, and
found in the log. That is what this file is.
