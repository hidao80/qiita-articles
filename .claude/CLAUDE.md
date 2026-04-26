# CLAUDE.md

このファイルは、このリポジトリで作業するAIコーディングエージェント（claude.ai/code・Codex・Gemini・Cursor・Windsurf・Antigravityなど）向けのガイダンスを提供します。

## プロジェクト概要

hidaoによるQiita記事の管理リポジトリ。`articles/`配下にMarkdownファイルとして記事を格納し、Qiita CLIでアップロード・管理する。記事のテーマはClaude Code・MCP・ローカルLLM・PHP・Composer・VS Code・Linux・Git（Jujutsu含む）など幅広い技術トピックを扱う。

## ワークフロー

タスクを開始する際は、そのタスクに関連する`everything-claude-code`の機能を活用すること。

- コードレビューを行う際は、重大度順（CRITICAL → HIGH → MEDIUM → LOW）に問題を修正し、次に進む前に各修正後にテストを実行すること。大規模なリファクタリング変更をまとめて行わないこと。

## 一般ルール

プロジェクト構造を調査する際は、`agents/`・`skills/`・プラグイン/設定ディレクトリを含むすべてのディレクトリを確認すること。非標準のディレクトリをスキップしないこと。
