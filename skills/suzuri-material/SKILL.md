---
name: suzuri-material
description: SUZURIの素材（デザイン画像）を管理。アップロード、更新、削除、テキストから作成。「素材アップロード」「デザイン登録」などのリクエスト時に使用。
allowed-tools: Bash
---

# SUZURI Material

SUZURIの素材（デザイン画像）を管理するスキル。

## Endpoints

| メソッド | エンドポイント | 機能 |
|----------|----------------|------|
| GET | `/api/v1/materials` | 素材リスト取得 |
| POST | `/api/v1/materials` | 素材作成 |
| PUT | `/api/v1/materials/{material_id}` | 素材更新 |
| DELETE | `/api/v1/materials/{material_id}` | 素材削除 |
| POST | `/api/v1/materials/text` | テキストから素材作成 |

## Instructions

### 素材一覧を取得

```bash
curl -s -H "Authorization: Bearer $SUZURI_API_TOKEN" \
  https://suzuri.jp/api/v1/materials | jq '.materials[]'
```

### 画像から素材を作成

```bash
curl -X POST \
  -H "Authorization: Bearer $SUZURI_API_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "texture": "https://example.com/image.png",
    "title": "素材タイトル",
    "description": "素材の説明",
    "products": [
      {"itemId": 1, "published": true, "resizeMode": "contain"}
    ]
  }' \
  https://suzuri.jp/api/v1/materials
```

### Data URIで画像を送信

```bash
# Base64エンコードした画像
BASE64_IMAGE=$(base64 -i image.png)

curl -X POST \
  -H "Authorization: Bearer $SUZURI_API_TOKEN" \
  -H "Content-Type: application/json" \
  -d "{
    \"texture\": \"data:image/png;base64,$BASE64_IMAGE\",
    \"title\": \"素材タイトル\"
  }" \
  https://suzuri.jp/api/v1/materials
```

### テキストから素材を作成

```bash
curl -X POST \
  -H "Authorization: Bearer $SUZURI_API_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "text": "Hello World",
    "title": "テキスト素材"
  }' \
  https://suzuri.jp/api/v1/materials/text
```

### 素材を更新

```bash
curl -X PUT \
  -H "Authorization: Bearer $SUZURI_API_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "title": "新しいタイトル",
    "description": "新しい説明"
  }' \
  https://suzuri.jp/api/v1/materials/{material_id}
```

### 素材を削除

```bash
curl -X DELETE \
  -H "Authorization: Bearer $SUZURI_API_TOKEN" \
  https://suzuri.jp/api/v1/materials/{material_id}
```

## Response Example

```json
{
  "material": {
    "id": 12345,
    "title": "素材タイトル",
    "texture": "https://...",
    "sampleImageUrl": "https://...",
    "products": [...]
  }
}
```

## Tips

- `sampleImageUrl`: WebP形式のプレビュー画像（推奨）
- `pngSampleImageUrl`: PNG形式のプレビュー画像
- `resizeMode`: `contain`（収める）, `cover`（埋める）
