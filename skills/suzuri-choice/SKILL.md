---
name: suzuri-choice
description: SUZURIのオモイデ（Choices/コレクション）を管理。作成、更新、削除、商品追加・削除。「コレクション作成」「オモイデ管理」などのリクエスト時に使用。
allowed-tools: Bash
---

# SUZURI Choice

SUZURIのオモイデ（Choices/キュレーションリスト）を管理するスキル。

## Endpoints

| メソッド | エンドポイント | 機能 |
|----------|----------------|------|
| GET | `/api/v1/choices` | オモイデリスト取得 |
| GET | `/api/v1/choices/{choice_id}` | オモイデ情報取得 |
| POST | `/api/v1/choices` | オモイデ作成 |
| PUT | `/api/v1/choices/{choice_id}` | オモイデ更新 |
| DELETE | `/api/v1/choices/{choice_id}` | オモイデ削除 |
| POST | `/api/v1/choices/{choice_id}` | 商品追加 |
| POST | `/api/v1/choices/{choice_id}/remove` | 商品削除 |

## Instructions

### オモイデ一覧を取得

```bash
curl -s -H "Authorization: Bearer $SUZURI_API_TOKEN" \
  https://suzuri.jp/api/v1/choices | jq '.choices[]'
```

### オモイデを作成

```bash
curl -X POST \
  -H "Authorization: Bearer $SUZURI_API_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "title": "お気に入りコレクション",
    "description": "私のお気に入り商品"
  }' \
  https://suzuri.jp/api/v1/choices
```

### オモイデに商品を追加

```bash
curl -X POST \
  -H "Authorization: Bearer $SUZURI_API_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "productId": 12345
  }' \
  https://suzuri.jp/api/v1/choices/{choice_id}
```

### オモイデから商品を削除

```bash
curl -X POST \
  -H "Authorization: Bearer $SUZURI_API_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "productId": 12345
  }' \
  https://suzuri.jp/api/v1/choices/{choice_id}/remove
```

### オモイデを更新

```bash
curl -X PUT \
  -H "Authorization: Bearer $SUZURI_API_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "title": "新しいタイトル",
    "description": "新しい説明"
  }' \
  https://suzuri.jp/api/v1/choices/{choice_id}
```

### オモイデを削除

```bash
curl -X DELETE \
  -H "Authorization: Bearer $SUZURI_API_TOKEN" \
  https://suzuri.jp/api/v1/choices/{choice_id}
```

## Response Example

```json
{
  "choice": {
    "id": 123,
    "title": "お気に入りコレクション",
    "description": "私のお気に入り商品",
    "products": [...]
  }
}
```
