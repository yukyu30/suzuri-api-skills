---
name: suzuri-user
description: SUZURIのユーザー情報を管理。自分の情報取得・更新、他ユーザー情報取得。「ユーザー情報」「プロフィール」などのリクエスト時に使用。
allowed-tools: Bash
---

# SUZURI User

SUZURIのユーザー情報を管理するスキル。

## Endpoints

| メソッド | エンドポイント | 機能 |
|----------|----------------|------|
| GET | `/api/v1/user` | 認証済みユーザ情報取得 |
| PUT | `/api/v1/user` | 認証済みユーザ情報更新 |
| GET | `/api/v1/users` | ユーザーリスト取得 |
| GET | `/api/v1/users/{user_id}` | ユーザー情報取得 |

## Instructions

### 自分の情報を取得

```bash
curl -s -H "Authorization: Bearer $SUZURI_API_TOKEN" \
  https://suzuri.jp/api/v1/user | jq .
```

### 自分の情報を更新

```bash
curl -X PUT \
  -H "Authorization: Bearer $SUZURI_API_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "displayName": "新しい表示名",
    "description": "新しい自己紹介"
  }' \
  https://suzuri.jp/api/v1/user
```

### ユーザー一覧を取得

```bash
curl -s -H "Authorization: Bearer $SUZURI_API_TOKEN" \
  https://suzuri.jp/api/v1/users | jq '.users[]'
```

### 特定ユーザーの情報を取得

```bash
curl -s -H "Authorization: Bearer $SUZURI_API_TOKEN" \
  https://suzuri.jp/api/v1/users/{user_id} | jq .
```

### ユーザーの商品一覧を取得

```bash
curl -s -H "Authorization: Bearer $SUZURI_API_TOKEN" \
  https://suzuri.jp/api/v1/users/{username}/products | jq '.products[]'
```

## Response Example

```json
{
  "user": {
    "id": 12345,
    "name": "username",
    "displayName": "表示名",
    "avatarUrl": "https://...",
    "description": "自己紹介",
    "productsCount": 10,
    "followersCount": 100,
    "followingCount": 50
  }
}
```

## Tips

- `name`: ユーザー名（URLに使用）
- `displayName`: 表示名（ニックネーム）
- ユーザーIDまたはユーザー名でアクセス可能
