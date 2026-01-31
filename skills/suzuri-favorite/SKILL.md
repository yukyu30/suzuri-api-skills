---
name: suzuri-favorite
description: SUZURIのズッキュン（お気に入り/いいね）を管理。追加、削除、一覧取得。「ズッキュン」「お気に入り」「いいね」などのリクエスト時に使用。
allowed-tools: Bash
---

# SUZURI Favorite

SUZURIのズッキュン（お気に入り）を管理するスキル。

## Endpoints

| メソッド | エンドポイント | 機能 |
|----------|----------------|------|
| GET | `/api/v1/products/{product_id}/favorites` | 商品へのズッキュン取得 |
| GET | `/api/v1/users/{user_id}/favorites` | ユーザのズッキュン取得 |
| POST | `/api/v1/products/{product_id}/favorites` | ズッキュン作成 |
| DELETE | `/api/v1/products/{product_id}/favorites` | ズッキュン削除 |

## Instructions

### 商品へのズッキュン一覧を取得

```bash
curl -s -H "Authorization: Bearer $SUZURI_API_TOKEN" \
  https://suzuri.jp/api/v1/products/{product_id}/favorites | jq '.favorites[]'
```

### ユーザーのズッキュン一覧を取得

```bash
curl -s -H "Authorization: Bearer $SUZURI_API_TOKEN" \
  https://suzuri.jp/api/v1/users/{user_id}/favorites | jq '.favorites[]'
```

### ズッキュンする（お気に入りに追加）

```bash
curl -X POST \
  -H "Authorization: Bearer $SUZURI_API_TOKEN" \
  https://suzuri.jp/api/v1/products/{product_id}/favorites
```

### ズッキュン解除

```bash
curl -X DELETE \
  -H "Authorization: Bearer $SUZURI_API_TOKEN" \
  https://suzuri.jp/api/v1/products/{product_id}/favorites
```

## Response Example

```json
{
  "favorites": [
    {
      "id": 12345,
      "user": {
        "id": 1,
        "name": "username"
      },
      "createdAt": "2024-01-01T12:00:00Z"
    }
  ]
}
```
