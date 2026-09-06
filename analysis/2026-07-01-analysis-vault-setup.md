---
created: 2026-07-01
updated: 2026-07-01
tags:
  - analysis
  - vault-setup
source: cursor
status: active
aliases: []
related:
  - chats/persistent/2026-07-01-vault-setup-test.md
  - chats/persistent/2026-07-01-vault-design.md
sources_updated: 2026-07-01
---

# 2026-07-01-analysis-vault-setup

## 分析対象

`chats/persistent/` 内の vault セットアップ関連ノート 2 件。

## 要約

- フォルダ構成は temp / persistent / analysis の 3 層
- メタデータは Templater テンプレートと Cursor rules で統一
- promote ワークフローで一時チャットを蓄積ノートに昇格可能

## 洞察

- Cloud Agent との連携は `.cursor/rules/` の glob 設定でフォルダ別に振る舞いを制御できる
- `related` フィールドにより persistent 間のリンクと分析の参照元を一元管理できる
- `## ログ` の話者ラベル（`**user:**` / `**assistant:**`）により、蓄積後も発言の帰属が追える。要約と原文が分離されているため、分析時に原文へ遡れる

## 未解決事項

- Templater プラグインの手動インストールが必要
- Dataview プラグイン導入でタグ横断検索を強化できる（任意）

## 参照元

- [[chats/persistent/2026-07-01-vault-setup-test]]
- [[chats/persistent/2026-07-01-vault-design]]
