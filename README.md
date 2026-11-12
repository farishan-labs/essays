# Essays — Faris Han Labs

Long-form, narrative, evidence-backed pieces by Faris Han. The narrative counterpart to
[`farishan-labs/research`](https://github.com/farishan-labs/research) (structured,
verified topics) and to [`farishan-labs/user-md`](https://github.com/farishan-labs/user-md)
(operator profile).

> **Essays vs. articles:** an *essay* is the source of truth in this repo (voice,
> argument, evidence). When syndicated to dev.to / Hashnode / Medium / DZone, the
> published unit is an *article* (platform-native). The essay stays canonical; the
> article is the rendering.

## What counts as an essay here

- 500–3000 words, first-person, conversational but evidence-backed.
- Has a thesis or a question it works toward; not a scratch note, not a research
  topic (those live in `research/`).
- Cites sources where load-bearing (link or footnote).
- One idea per essay; cut scope creep into a separate essay.

## What does *not* count

- Scratch notes → `research/notes/` or `research/topics/<slug>/`.
- Verified, structured findings → `research/topics/`.
- Raw observations → tag as a **field note** inside `essays/` (frontmatter
  `series: field-notes`) rather than a new repo.

## Layout

```
essays/          # published or publish-ready essays (one .md per essay)
drafts/          # work in progress
```

Frontmatter (YAML) on every essay:

```yaml
---
title: "Title"
date: 2026-11-12
status: draft | published
series: essay | field-notes
source: farishan-labs/research/topics/<slug>   # if promoted from research
tags: [ai, agents, ...]
---
```

## Promotion process (research → essay)

1. In the research repo: `bun run promote <topic-slug>` (script:
   `research/scripts/promote-to-essay.py`) scaffolds an essay here with frontmatter
   pre-filled (title, source path, date, tags).
2. Edit in `drafts/`, move to `essays/` when ready.
3. Publish to dev.to (article) and/or Gumroad (ebook) / landing page.
4. Update essay frontmatter `status: published` + add published URL.

## Style guide

- Voice: first-person, direct, no hype. Say what you mean, briefly (Paul Graham).
- Structure: hook → 2–4 sections → takeaway. No filler.
- Length: aim for the shortest version that carries the argument.
- Evidence: link load-bearing claims; mark self-reported vs verified.
- Avoid: listicles, "ultimate guide", "in this article we will".

## License

Content © Faris Han. Code/scripts MIT. Essays may be shared with attribution.