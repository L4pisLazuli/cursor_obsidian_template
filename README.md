# cursor_obsidian_template

An Obsidian vault template wired up with Cursor Agent rules, for capturing AI chats
(Cursor / Gemini / ChatGPT) as notes and growing them into a searchable knowledge base.

Cursor is the front end (conversation, editing, analysis). The Obsidian vault is the
back end (storage, links, cross-note analysis).

This repo is **meant to be used as a private repository**. After cloning, copy
`profile/user.example.md` to `profile/user.md` and fill in your own context.

## Folder layout

```
cursor_obsidian_template/
├── .cursor/rules/          # Rules for Cursor Agent
├── .obsidian/              # Obsidian settings (workspace/plugins are git-ignored)
├── templates/              # Templater templates
├── profile/                # Your own context (tracked, private-repo assumption)
├── chats/
│   ├── temp/               # Scratch chats — disposable, deleted unless promoted
│   └── persistent/         # Kept long term, linked and analyzed
│       └── _archive/       # Archived persistent notes
├── analysis/               # Cross-note analysis of persistent notes
└── assets/                 # Attachments (images, etc.)
```

## Quick start

1. Clone the repo
2. In Obsidian, "Open folder as vault" and select this folder
3. Copy `profile/user.example.md` to `profile/user.md` and fill it in
   - `profile/user.md` is **not** git-ignored, since this repo assumes private use
4. Install the Templater plugin (see [Obsidian setup](#obsidian-setup))
5. Open the same folder in Cursor — the Agent picks up `.cursor/rules/` automatically

### Setup checklist

- [ ] The folder opens as an Obsidian **vault**
- [ ] `profile/user.md` exists (copied from `user.example.md`) and is filled in
- [ ] Templater's "Template folder location" is set to `templates`
- [ ] Cursor is open on this folder and new chat notes land in `chats/temp/`

## Speaker attribution in chat logs

Raw conversation is stored under a `## ログ` (log) heading, and **every utterance is
labeled with its speaker**, so you can still tell who said what months later.

```markdown
## ログ

**user:**
Why split temp and persistent?

**assistant:**
Different deletion policies. temp is disposable;
persistent is kept, linked and analyzed.
```

Rules of the format:

- A turn starts with a line containing only `**<speaker>:**`, and runs until the next
  such line or the next `##` heading
- The label is bold, at the start of the line, with a colon — so turns stay greppable
- Labels: `user`, `assistant`, `system`, `note` (something you added later, not said
  during the conversation) — and only those. Any other `**bold:**` is body text, so a
  `**Note:**` inside an answer is not mistaken for a speaker
- **Fenced code blocks are opaque**: a `**user:**` or a `## heading` inside a fence is
  quoted material, not a delimiter. When the quoted content itself contains a fence,
  wrap it in four or more backticks
- New turns are appended to the end of `## ログ`; earlier turns are never rewritten,
  reordered or merged, so a long back-and-forth stays parseable
- Multiple humans: `user (name)`. Multiple AI tools in one note: `assistant (gemini)`,
  `assistant (chatgpt)` — otherwise the `source` field already says which tool it was
- Optional timestamps: `**user (18:20):**`
- Text in `## ログ` is **never** summarized, reworded or merged. Summaries live in
  `## 概要` / `## 要点` / `## メモ`; fragments with no known speaker do not go in the log

Every note under `chats/` also lists its speakers in frontmatter via `participants`
(actual speakers only — `system` and `note` are annotations, not participants).
The full spec lives in `.cursor/rules/chat-log-format.mdc`.

## Working with Cursor Agent

### New chat (temp)

The Agent creates a temp note **automatically and silently** when a chat starts — you
do not have to ask for it.

- Location: always `chats/temp/`
- Filename: `YYYY-MM-DD-HHmmss.md`, renamed to `YYYY-MM-DD-topic.md` on promote
- Frontmatter is applied from the template automatically
- Conversation is appended to `## ログ` with speaker labels

Temp notes are **disposable**: anything not promoted before the end of a session is
deleted. Only promoted notes survive.

### Promote temp → persistent

To keep a chat, say something like:

> このノートを蓄積して / persistent に移して / 保存して

The Agent then:

1. Moves the file from `chats/temp/` to `chats/persistent/`
2. Changes the `temp` tag to `persistent`
3. Sets `status` to `active`
4. Renames it to a topic-based filename
5. Tidies up `## 概要` / `## 要点` — the raw `## ログ` is carried over untouched

### Cross-note analysis

> persistent の ○○ について分析して

The Agent searches `chats/persistent/` and writes the result to `analysis/`.

### Cleanup and archiving

| Goal | Example instruction |
|------|--------------------|
| Tidy up temp | 「temp の古いノートを整理して」 |
| Archive a note | 「この persistent をアーカイブして」 |

Archiving sets `status: archived` and moves the file to `chats/persistent/_archive/`.
Notes are never deleted from persistent — they are kept as history.

## Sample notes

`chats/persistent/` and `analysis/` ship with worked examples. Delete them after
cloning if you would rather start from an empty vault.

| File | Shows |
|------|-------|
| `chats/persistent/2026-07-01-vault-design.md` | A persistent note with a labeled `## ログ` |
| `chats/persistent/2026-07-01-vault-setup-test.md` | The same, as it looks right after a promote |
| `analysis/2026-07-01-analysis-vault-setup.md` | A cross-note analysis with `sources_updated` |

## Obsidian setup

1. "Open folder as vault" and select this `cursor_obsidian_template` folder
2. Settings → Community plugins → install and enable **Templater**
   - `.obsidian/plugins/` is git-ignored, so this is done once per machine
3. In Templater settings, set "Template folder location" to `templates`
4. New notes default to `chats/temp/` (already set in `.obsidian/app.json`)

### Templates

| Template | Used for |
|----------|----------|
| `templates/chat-temp.md` | Scratch chat |
| `templates/chat-persistent.md` | Persistent note |
| `templates/analysis.md` | Analysis note |

Insert them with `Ctrl/Cmd + P` → Templater.

## Frontmatter schema

```yaml
---
created: YYYY-MM-DD
updated: YYYY-MM-DD
tags:
  - chat
  - temp          # or persistent / analysis
source: cursor    # cursor | gemini | chatgpt | manual
status: draft     # draft | active | archived
aliases: []
related: []
participants:     # chats/ only — speaker labels used in the note
  - user
  - assistant
sources_updated: YYYY-MM-DD  # analysis only — when sources were last checked
---
```

## Naming

- General: `YYYY-MM-DD-short-topic.md`
- Analysis: `YYYY-MM-DD-analysis-topic.md`
- Topics: English kebab-case preferred (e.g. `vault-design`); Japanese is allowed, but
  avoid mixing both within one vault
- Assets: `YYYY-MM-DD-description.ext`, or prefixed with the source note's name

## Cursor rules

`.cursor/rules/` contains seven rules:

| Rule | Applies to |
|------|-----------|
| `vault-core.mdc` | Always (overall vault policy) |
| `chat-log-format.mdc` | `chats/**/*.md` (speaker labels, raw log handling) |
| `chat-temp.mdc` | `chats/temp/**/*.md` |
| `chat-persistent.mdc` | `chats/persistent/**/*.md` |
| `analysis-workflow.mdc` | `analysis/**/*.md` |
| `obsidian-frontmatter.mdc` | All `**/*.md` |
| `git-workflow.mdc` | Git config files, README, rule changes |

## Git workflow (private-repo assumption)

| Change | How |
|--------|-----|
| Adding notes, promoting, adding analysis | Commit straight to `main` |
| Changing `.cursor/rules/` | Branch + PR recommended |
| Restructuring folders | Branch + PR required |
| Changing the privacy setup (incl. `.gitignore`) | Branch + PR recommended |

- `.gitignore` excludes Obsidian workspace/graph state, `.obsidian/plugins/`, `.trash/`,
  and OS/editor junk files
- `.gitattributes` normalizes line endings to LF (Windows-friendly)
- `profile/user.md` is tracked. **Under private use, committing it is fine.** If you ever
  make the repo public, add it back to `.gitignore` first
- Before committing, check that no secrets (passwords, API keys) and no
  `.obsidian/workspace*.json` slipped in

## Trigger phrases

| Goal | Example instruction |
|------|--------------------|
| Save to temp | 「この会話を temp に保存して」 |
| Promote | 「蓄積して」「persistent に移して」 |
| Analyze | 「persistent の API 設計について分析して」 |
| Tidy up | 「temp の古いノートを整理して」 |
| Archive | 「このノートをアーカイブして」 |

The Agent replies in Japanese by default (set in `vault-core.mdc`).

## License

[CC0 1.0 Universal](https://creativecommons.org/publicdomain/zero/1.0/) — see
[LICENSE](LICENSE). Copy, modify and redistribute freely.
