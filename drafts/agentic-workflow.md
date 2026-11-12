---
title: "Agentic workflow, or: how I actually work with agents"
date: 2026-11-12
status: draft
series: essay
tags: [agents, workflow, cockpit, user-md, openspec, optmem]
---

# Agentic workflow, or: how I actually work with agents

I'm not sure "agentic workflow" is the right name for what I'm about to
describe. It might be too grand, or too narrow, or already taken by something
else. But I need a word for the whole thing — the way I use tools, harnesses,
models, memory, and conventions to get work done with AI — because I keep
referring to it in pieces ("the cockpit", "the operator profile", "the spec
engine") and losing the shape of the whole. This essay is the whole.

## The shape of the stack

The stack has six layers. Each one is a separate repo or tool, and that's
deliberate: no single system owns the workflow, so no single system can break it.

**1. Harness.** OpenCode on both machines, pi as a second harness. The harness
is where the session lives: it loads `AGENTS.md`, skills, commands, plugins,
and the global instructions. It does not think; it routes.

**2. Providers.** Beam (reflection) is the daily driver. The provider is
swappable — the harness doesn't care which model answers, only that the model
can call tools and follow instructions. I keep a fallback list (free models,
other providers) for when a provider is down or out of credits.

**3. Memory.** OptMem. A small append-only memory tree that outlives every
session, compaction, model, and vendor change. Every session starts with
`memory wake`. Every durable fact gets a one-line note. This is the
through-line: without it, each session starts from zero and re-derives
everything.

**4. Identity.** Two files, kept strictly separate (the OpenClaw split):
`SOUL.md` is the agent's persona; `USER.md` is mine. `AGENTS.md` holds
project instructions. Machine facts live in a machine profile. Project facts
live in the project. Mixing these is how preferences leak into infra rules and
get re-derived on every new machine.

**5. Spec engine.** OpenSpec. Changes are proposals → specs → design → tasks,
validated before implementation. Cockpit was the first repo to adopt it; the
intent is that every repo in the lab uses the same spec engine, so an agent
moving between repos sees the same shape of work.

**6. Control plane.** Cockpit. The thing that launches agents in terminal
sessions, tracks their status, holds a tool registry, and (eventually) injects
the operator profile into every session it starts. It is an aggregator, not a
replacement: it reuses tmux, herdr, pi, opencode, SQLite.

## Work vs. personal

One machine, two modes. On the Mac I have both a work overlay (Hubton cockpit,
hucli, research-dev skill, MCP servers) and a personal core (`farishan/agent`).
On the ThinkPad it's personal only. The rule: personal assets live in the
personal core and are installed via profile steps; work assets stay in the work
overlay and are never pushed to the personal core. Memory is per-machine but
shares a generic seed.

This split is the reason the workflow can't be owned by a single repo. The
personal core is one consumer; the work overlay is another; cockpit is a third.
They share conventions, not code.

## Why it's not a repo

I tried, briefly, to make `farishan/agent` the single source of truth for the
whole workflow. It doesn't work, because the workflow spans machines, modes,
and products that have different owners and lifecycles. What *is* shareable is
the contract: a manifest of paths (where is `USER.md`, where is the machine
profile, where is OptMem, which spec engine, which agent set, which mode) plus
the conventions (OpenClaw split, P-001..P-024, search-guard, OptMem notes).
The manifest is the system; the repos are consumers.

That manifest is what I'm calling the Operator Context in the cockpit ↔ user-md
integration doc. It is the thing both cockpit (runtime) and user-md (static
template + validator) point at. Neither owns it.

## What cockpit adds

Cockpit is the runtime half of the workflow: it launches agents, tracks them,
and (in progress) injects `USER.md` into every session via `USER_MD_PATH` +
a symlink into the working directory. `cockpit doctor` validates the operator
profile with `usermd-check`. The web UI gets an "Operator" panel. In other
words: cockpit makes the manifest *executable* — it turns the Operator Context
from a document into something every launched agent actually sees.

## What user-md adds

user-md is the static half: the template, the guide, the worksheet, and the
validator (`usermd-check`). It defines what a valid operator profile looks
like and enforces the cap, secrets, contradictions, scope, and compatibility
checks. Cockpit consumes it; it does not reimplement it (P-001).

## What's missing

- A shared manifest that both tools read. Now exists: `farishan/agent/workflow.toml`
  (installed at `~/.config/fhl/workflow.toml`), consumed by cockpit and
  `usermd-check --workflow`. Remaining: export/diff/share tooling and a shared
  reader in every consumer.
- A spec engine adopted across all repos. Cockpit, user-md, and farishan/agent
  now use OpenSpec; user-md-ops is still pending.
- The Operator Context as a first-class, versioned artifact rather than a
  convention. Done (workflow.toml). Still to add: exportable, diffable, shareable.
- A name. "Agentic workflow" is fine as a working label, but it leans on
  "agentic" which already means too many things. If a better term shows up,
  rename freely — the manifest doesn't care what we call it.

## Takeaway

The workflow is not a tool and not a repo. It is a set of conventions plus a
manifest of paths, consumed by harnesses, control planes, validators, and
memory. The job of each new tool (cockpit, user-md, OpenSpec, the essays repo)
is to make one part of that manifest concrete and checkable, without owning
the whole thing. That is the line I'm trying to hold.