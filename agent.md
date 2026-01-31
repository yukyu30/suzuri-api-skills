# AGENTS.md

This file provides guidance to AI coding agents (Claude Code, Cursor, Copilot, etc.) when working with code in this repository.

## Repository Overview

SUZURI API完全ガイド。認証設定、グッズ作成、商品管理、素材管理など全APIエンドポイントをサポート。

## Language

常に日本語で返答してください。

## Skill

| スキル | 機能 |
|--------|------|
| [suzuri-api-guide](skills/suzuri-api-guide/SKILL.md) | SUZURI API完全ガイド（認証、グッズ作成、商品管理、素材管理、ユーザー管理） |

## API Reference

- [SUZURI API公式ドキュメント](https://suzuri.jp/developer/documentation/v1)
- [SUZURI API Tips集（Zenn）](https://zenn.dev/yu_9/articles/7d372e293b260e)

## Authentication

```bash
# 環境変数に設定
export SUZURI_API_TOKEN="your_token_here"

# リクエスト例
curl -H "Authorization: Bearer $SUZURI_API_TOKEN" \
  https://suzuri.jp/api/v1/user
```

## Rate Limits

レスポンスヘッダーで確認：
- `X-Ratelimit-Limit`: 上限
- `X-Ratelimit-Remaining`: 残り
- `X-Ratelimit-Reset`: リセット時刻
