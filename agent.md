# AGENTS.md

This file provides guidance to AI coding agents (Claude Code, Cursor, Copilot, etc.) when working with code in this repository.

## Repository Overview

SUZURI API特化スキル集。グッズ作成、商品管理、認証設定をサポート。

## Language

常に日本語で返答してください。

## API Map

**[suzuri-api-map.md](suzuri-api-map.md)** - 全エンドポイントとスキルの対応表

## Available Skills

| スキル | 機能 |
|--------|------|
| [suzuri-auth](skills/suzuri-auth/SKILL.md) | 認証設定（APIキー、OAuth） |
| [suzuri-activity](skills/suzuri-activity/SKILL.md) | アクティビティ（通知） |
| [suzuri-choice](skills/suzuri-choice/SKILL.md) | オモイデ（コレクション） |
| [suzuri-favorite](skills/suzuri-favorite/SKILL.md) | ズッキュン（お気に入り） |
| [suzuri-item](skills/suzuri-item/SKILL.md) | アイテム情報 |
| [suzuri-material](skills/suzuri-material/SKILL.md) | 素材管理 |
| [suzuri-product-create](skills/suzuri-product-create/SKILL.md) | 商品作成 |
| [suzuri-product-manage](skills/suzuri-product-manage/SKILL.md) | 商品管理 |
| [suzuri-user](skills/suzuri-user/SKILL.md) | ユーザー管理 |

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
