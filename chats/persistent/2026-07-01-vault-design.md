---
created: 2026-07-01
updated: 2026-07-01
tags:
  - chat
  - persistent
  - vault-design
source: cursor
status: active
aliases: []
related:
  - analysis/2026-07-01-analysis-vault-setup.md
  - chats/persistent/2026-07-01-vault-setup-test.md
participants:
  - user
  - assistant
---

## 概要

Obsidian Vault + Cursor Rules の設計方針（サンプルノート）。

## 要点

- 一時チャット（`chats/temp/`）と継続蓄積（`chats/persistent/`）を分離
- Templater + Cursor rules で frontmatter を統一管理
- 分析は `analysis/` フォルダで横断的に実施

## ログ

**user:**
チャットの保存先を 1 フォルダにまとめるか、temp と persistent で分けるか迷っている。

**assistant:**
分けたほうがよい。削除ポリシーが違うため。
temp は使い捨てで、保存指示がなければセッション終了時に消す。
persistent は長期保持してリンク・分析の対象にする。
1 フォルダに混ぜると「消していいノート」の判断が毎回必要になる。

**user:**
frontmatter は Templater と Cursor rules のどちらで管理する？

**assistant:**
両方に同じスキーマを持たせる。
手動作成は Templater テンプレート、Agent 作成は `obsidian-frontmatter` ルールが担当し、
スキーマ定義は 1 か所（ルール側）を正とする。

**note:**
この記法（`**話者:**` で 1 発言を区切る）は `chat-log-format` ルールで定義している。

## 関連リンク

- [[chats/persistent/2026-07-01-vault-setup-test]]
- [[analysis/2026-07-01-analysis-vault-setup]]
