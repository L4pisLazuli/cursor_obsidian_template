---
created: 2026-07-01
updated: 2026-07-01
tags:
  - chat
  - persistent
  - vault-setup
source: cursor
status: active
aliases: []
related:
  - analysis/2026-07-01-analysis-vault-setup.md
  - chats/persistent/2026-07-01-vault-design.md
participants:
  - user
  - assistant
---

## 概要

Vault セットアップの動作確認用サンプルノート（promote 後の例）。

## 要点

- Cursor rules が 7 ファイル配置された
- Templater テンプレートが 3 種作成された
- frontmatter スキーマが正しく付与されている
- promote ワークフロー: `temp` → `persistent` タグ変更、`status: active`

## ログ

**user:**
セットアップ後の確認をしたい。何を見ればいい？

**assistant:**
4 点を確認する。
1. `.cursor/rules/` に 7 ファイルあるか
2. `templates/` に 3 テンプレートあるか
3. 新規ノートに frontmatter が付くか
4. promote でタグと `status` が書き換わるか

**user:**
promote したときログはどうなる？

**assistant:**
`## ログ` の原文はそのまま持ち越す。
promote で整理するのは `## 概要` と `## 要点` のみで、原文の要約や統合はしない。

## 関連リンク

- [[chats/persistent/2026-07-01-vault-design]]
- [[analysis/2026-07-01-analysis-vault-setup]]
