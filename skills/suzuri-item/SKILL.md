---
name: suzuri-item
description: SUZURIで利用可能なアイテム（商品種類）を取得。Tシャツ、パーカー、マグカップなどのアイテムID確認。「アイテム一覧」「商品種類」などのリクエスト時に使用。
allowed-tools: Bash
---

# SUZURI Item

SUZURIで利用可能なアイテム（商品種類）を取得するスキル。

## Endpoints

| メソッド | エンドポイント | 機能 |
|----------|----------------|------|
| GET | `/api/v1/items` | アイテムリスト取得 |

## Instructions

### アイテム一覧を取得

```bash
curl -s -H "Authorization: Bearer $SUZURI_API_TOKEN" \
  https://suzuri.jp/api/v1/items | jq '.items[] | {id, name, humanizeName}'
```

### 主なアイテム例

| ID | 名前 | 表示名 |
|----|------|--------|
| 1 | t-shirt | Tシャツ |
| 2 | long-sleeve-t-shirt | ロングスリーブTシャツ |
| 3 | hoodie | パーカー |
| 4 | tote-bag | トートバッグ |
| 5 | mug | マグカップ |
| 6 | smartphone-case | スマホケース |
| 7 | sticker | ステッカー |
| 8 | notebook | ノート |

### アイテム詳細（カラー・サイズ情報）

```bash
curl -s -H "Authorization: Bearer $SUZURI_API_TOKEN" \
  https://suzuri.jp/api/v1/items | jq '.items[] | select(.id == 1)'
```

## Response Example

```json
{
  "items": [
    {
      "id": 1,
      "name": "t-shirt",
      "humanizeName": "Tシャツ",
      "colors": [...],
      "sizes": [...],
      "variants": [...]
    }
  ]
}
```

## Tips

- 商品作成時に`itemId`パラメータで使用するアイテムを指定
- カラーやサイズはアイテムによって異なる
- `variants`で各組み合わせの詳細を確認可能
