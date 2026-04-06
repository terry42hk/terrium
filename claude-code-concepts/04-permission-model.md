The model understanding your intent does not mean it has authorization to act. Permission is the gatekeeper that enforces this separation.

## Table of Contents

- [Three Permission Results (Not Boolean)](#three-permission-results-not-boolean)
- [Five-Layer Priority (Organization to Individual)](#five-layer-priority-organization--individual)
- [Four Permission Modes (Switch with Shift+Tab)](#four-permission-modes-switch-with-shifttab)
- [Allowlist / Denylist: Fine-Grained Control](#allowlist--denylist-fine-grained-control)
- [How Permission Relates to Hook](#how-permission-relates-to-hook)
- [The Full Four-Layer Control Stack](#the-full-four-layer-control-stack)
- [Practical Scenarios](#practical-scenarios)
- [Connection to Previous Concepts](#connection-to-previous-concepts)

## Three Permission Results (Not Boolean)

Every tool call goes through the harness, which returns one of three results:

| Result | Meaning | What happens |
|--------|---------|-------------|
| **allow** | Approved automatically | You never see it -- tool executes silently |
| **deny** | Forbidden | Tool blocked, model receives synthetic error |
| **ask** | System won't decide for you | Prompt appears: "Claude wants to run X. Allow?" |

The existence of `ask` is important -- it acknowledges that sometimes the system itself should not decide on your behalf.

> *"Understanding intent does not equal authorization. The system must separate 'can do' from 'may do'."*

## Five-Layer Priority (Organization to Individual)

```
1. Organization policy    <- Highest priority (company-wide)
2. CLI flags              <- Launch-time overrides
3. Local settings         <- Personal (.claude/settings.local.json)
4. Project settings       <- Team-shared (.claude/settings.json)
5. User interaction       <- Per-request approval (lowest)
```

Higher layers override lower layers. If org policy denies `rm -rf`, your personal allow has no effect.

## Four Permission Modes (Switch with Shift+Tab)

| Mode | Behavior | Best for |
|------|----------|----------|
| **Default** | Asks before edits and shell commands | Beginners, sensitive operations |
| **Auto-accept edits** | File changes auto-approved, Bash still asks | Daily development |
| **Auto mode** | Classifier model assesses risk, only blocks high-risk | Experienced users |
| **Plan mode** | Read-only -- no edits, no commands | Research, zero side-effects |

## Allowlist / Denylist: Fine-Grained Control

```json
{
  "permissions": {
    "allow": [
      "Read", "Glob", "Grep",
      "Bash(npm test)", "Bash(git status)", "Bash(git diff)"
    ],
    "deny": [
      "Bash(rm -rf *)", "Bash(git push --force)", "Bash(git reset --hard)"
    ]
  }
}
```

**Principle:**
- All **read-only** operations -> allow
- **Safe, frequent** commands -> allow
- **Irreversible, dangerous** operations -> deny
- **Everything else** -> handled by current mode (ask or auto-classify)

## How Permission Relates to Hook

| | Permission | Hook |
|--|--|--|
| Role | Gatekeeper -- **may you do this?** | Automated guard -- **what happens before/after?** |
| Decision | allow / deny / ask | pass / block (exit code 2) |
| Who runs it | Harness rule engine | Your shell scripts |
| Claude aware? | Yes -- receives "denied" message | No -- completely transparent |
| Analogy | Security guard: can you enter this room? | Auto-sanitizer at the door: sprays on entry, you don't control it |

**They work together:**
```
Permission: allow Bash(npm test)       -> no need to ask
Hook: PostToolUse(Bash) -> log command  -> runs automatically after
```

Permission decides "may you". Hook decides "what else happens when you do".

> **Note for non-technical users:** You need to *use* Permission (it's your trust boundary). You only need to *know Hook exists* -- setup typically requires engineering help. Hook is a deterministic automation layer; the key insight is that it always runs, unlike CLAUDE.md instructions which the model may occasionally skip.

## The Full Four-Layer Control Stack

```
Permission  -> May you do this?           <- Hardest, deterministic
Hook        -> What else happens?          <- Hard, deterministic
CLAUDE.md   -> How should you do it?       <- Soft, persistent but probabilistic
Conversation-> Do it this way this time    <- Softest, lost on compaction
```

## Practical Scenarios

### Scenario 1: The Blind Approve Trap
You're debugging, Claude keeps asking to run `npm test`. After approving 20 times, you stop reading and approve `rm -rf node_modules && npm install` by accident. **Fix:** Allowlist `npm test`, keep other Bash commands on ask.

### Scenario 2: Safe Exploration with Plan Mode
You inherit a new codebase. Switch to Plan mode -- Claude reads dozens of files, searches patterns, explains architecture. Zero risk of accidental changes. **Plan mode = "look but don't touch".**

### Scenario 3: Auto Mode Is Not "Allow Everything"
Auto mode uses a classifier model to assess risk. `npm test` -> auto-allow. `git push origin main` -> still asks you. **But the classifier is probabilistic** -- always set a denylist as hard boundary.

## Connection to Previous Concepts

| Concept | Relationship |
|---------|-------------|
| **01 Harness vs Model** | Permission is a core harness responsibility -- model proposes, harness authorizes |
| **02 Agent Loop** | Every tool call in every loop turn passes through permission check |
| **03 Tools** | Two sides of one coin -- Tools = "what can be done", Permission = "what may be done" |

## For Non-Technical Users

Permissions are your safety net. Even though Claude is smart enough to understand what you want, it still needs your approval for risky actions -- like how a bank teller understands your request to withdraw money but still checks your ID. You control the trust level: ask Claude to get approval for everything (cautious), let it handle safe stuff automatically (balanced), or let it make most decisions on its own (autonomous). Start cautious and loosen as you build confidence.

## Reflection Questions

1. You're debugging and Claude keeps asking permission for `npm test`. You approve it 30 times. What's the risk, and how would you solve it with allowlists?
2. Why does the permission system have three results (allow/deny/ask) instead of just two (allow/deny)? What does the existence of "ask" acknowledge?
3. When would you choose Plan mode over Default mode? Give a concrete scenario.
