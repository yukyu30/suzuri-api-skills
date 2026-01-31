---
name: suzuri-auth
description: SUZURI APIの認証設定をサポート。APIキー取得、OAuth設定、トークン管理。「SUZURI認証」「SUZURIセットアップ」などのリクエスト時に使用。
allowed-tools: Read, Write, Edit, Bash
---

# SUZURI Auth Setup

SUZURI APIの認証設定をサポートするスキル。

## 認証方式

SUZURI APIは2つの認証方式をサポート：

### 1. APIキー認証（開発用）

個人用トークン。`read`と`write`スコープを持つ。

```bash
# 環境変数に設定
export SUZURI_API_TOKEN="your_token_here"

# リクエスト例
curl -H "Authorization: Bearer $SUZURI_API_TOKEN" \
  https://suzuri.jp/api/v1/user
```

### 2. OAuth認証（アプリケーション用）

他ユーザーの代わりに操作する場合に使用。

```
1. GET /oauth/authorize でユーザーを認証ページへリダイレクト
2. ユーザーが許可
3. POST /oauth/token でアクセストークン取得
```

**注意**: コールバックURLはHTTPS必須。ローカル開発にはOrbstackなどを使用。

## Instructions

### 環境変数の設定

```bash
# .envファイルを作成
cat > .env << 'EOF'
SUZURI_API_TOKEN=your_token_here
SUZURI_CLIENT_ID=your_client_id
SUZURI_CLIENT_SECRET=your_client_secret
EOF
```

### トークンの取得方法

1. [SUZURI開発者ページ](https://suzuri.jp/developer/documentation/v1) にアクセス
2. 「アプリケーションを登録」または「個人用トークンを発行」
3. トークンを安全に保存

### 接続テスト

```bash
curl -s -H "Authorization: Bearer $SUZURI_API_TOKEN" \
  https://suzuri.jp/api/v1/user | jq .
```

## レートリミット

レスポンスヘッダーで確認：
- `X-Ratelimit-Limit`: 上限
- `X-Ratelimit-Remaining`: 残り
- `X-Ratelimit-Reset`: リセット時刻

## References

- [SUZURI API公式ドキュメント](https://suzuri.jp/developer/documentation/v1)
- [SUZURI API Tips集](https://zenn.dev/yu_9/articles/7d372e293b260e)
