---
name: suzuri-product-manage
description: SUZURI商品の一覧取得、検索、更新、削除。「SUZURI商品一覧」「商品を検索」「商品を更新」などのリクエスト時に使用。
allowed-tools: Read, Write, Edit, Bash
---

# SUZURI Product Manage

SUZURI商品の管理（一覧・検索・更新・削除）スキル。

## Instructions

### 商品一覧を取得

```bash
# 自分の商品一覧
curl -s -H "Authorization: Bearer $SUZURI_API_TOKEN" \
  https://suzuri.jp/api/v1/products | jq '.products[] | {id, title, sampleUrl}'
```

### 商品を検索

```bash
# キーワードで検索
curl -s -H "Authorization: Bearer $SUZURI_API_TOKEN" \
  "https://suzuri.jp/api/v1/products/search?q=猫" | jq '.products[]'
```

### 商品詳細を取得

```bash
curl -s -H "Authorization: Bearer $SUZURI_API_TOKEN" \
  https://suzuri.jp/api/v1/products/{product_id} | jq .
```

### 商品を更新

```bash
curl -X PUT \
  -H "Authorization: Bearer $SUZURI_API_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "title": "新しいタイトル",
    "description": "新しい説明",
    "published": true
  }' \
  https://suzuri.jp/api/v1/products/{product_id}
```

### 商品を削除

```bash
curl -X DELETE \
  -H "Authorization: Bearer $SUZURI_API_TOKEN" \
  https://suzuri.jp/api/v1/products/{product_id}
```

## ユーザー情報

### 自分の情報

```bash
curl -s -H "Authorization: Bearer $SUZURI_API_TOKEN" \
  https://suzuri.jp/api/v1/user | jq .
```

### 他ユーザーの商品

```bash
curl -s -H "Authorization: Bearer $SUZURI_API_TOKEN" \
  https://suzuri.jp/api/v1/users/{username}/products | jq '.products[]'
```

## Choices（キュレーション）管理

### Choices一覧

```bash
curl -s -H "Authorization: Bearer $SUZURI_API_TOKEN" \
  https://suzuri.jp/api/v1/choices | jq '.choices[]'
```

### Choicesに商品を追加

```bash
curl -X POST \
  -H "Authorization: Bearer $SUZURI_API_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "productId": 12345
  }' \
  https://suzuri.jp/api/v1/choices/{choice_id}/products
```

## レスポンス例

```json
{
  "product": {
    "id": 12345,
    "title": "かわいい猫Tシャツ",
    "description": "猫好きのためのTシャツ",
    "sampleImageUrl": "https://...",
    "sampleUrl": "https://suzuri.jp/...",
    "published": true,
    "price": 2500
  }
}
```

## 便利なjqフィルタ

```bash
# 商品タイトルと価格のみ
jq '.products[] | {title, price}'

# 公開中の商品のみ
jq '.products[] | select(.published == true)'

# 価格順でソート
jq '.products | sort_by(.price)'
```

## References

- [SUZURI API公式ドキュメント](https://suzuri.jp/developer/documentation/v1)
