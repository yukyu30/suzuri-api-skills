# AGENTS.md

This file provides guidance to AI coding agents (Claude Code, Cursor, Copilot, etc.) when working with code in this repository.

## Repository Overview

SUZURI API特化スキル集。グッズ作成、商品管理、認証設定をサポート。

## Language

常に日本語で返答してください。

## Available Skills

### suzuri-auth
SUZURI APIの認証設定。APIキー取得、OAuth設定、トークン管理。

### suzuri-product-create
画像からSUZURIグッズを作成。Tシャツ、パーカー、マグカップなど。

### suzuri-product-manage
商品の一覧取得、検索、更新、削除。

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
