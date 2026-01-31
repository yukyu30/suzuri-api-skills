---
name: suzuri-product-create
description: SUZURI APIで画像からグッズを作成する。「SUZURIで商品作成」「グッズを作って」「Tシャツ作成」などのリクエスト時に使用。
allowed-tools: Read, Write, Edit, Bash
---

# SUZURI Product Create

画像からSUZURIグッズを作成するスキル。

## Instructions

### 1. 利用可能なアイテム一覧を取得

```bash
curl -s -H "Authorization: Bearer $SUZURI_API_TOKEN" \
  https://suzuri.jp/api/v1/items | jq '.items[] | {id, name, humanizeName}'
```

主なアイテム:
- Tシャツ (t-shirt)
- パーカー (hoodie)
- トートバッグ (tote-bag)
- マグカップ (mug)
- スマホケース (smartphone-case)
- ステッカー (sticker)

### 2. 商品プレビューを作成

```bash
curl -X POST \
  -H "Authorization: Bearer $SUZURI_API_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "texture": "https://example.com/image.png",
    "title": "商品タイトル",
    "description": "商品説明",
    "products": [
      {"itemId": 1, "published": true}
    ]
  }' \
  https://suzuri.jp/api/v1/materials
```

**textureパラメータ**:
- 画像URL: `https://example.com/image.png`
- Data URI: `data:image/png;base64,...`

### 3. プレビュー画像を取得

レスポンスから以下を使用：
- `sampleImageUrl`: WebP形式（推奨）
- `pngSampleImageUrl`: PNG形式

### 4. 購入ページへ誘導

レスポンスの`sampleUrl`フィールドを使用してユーザーを商品ページへ誘導。

### 5. プレビューをキャンセル（任意）

ユーザーがキャンセルした場合：

```bash
curl -X DELETE \
  -H "Authorization: Bearer $SUZURI_API_TOKEN" \
  https://suzuri.jp/api/v1/materials/{material_id}
```

## ワークフロー例

```markdown
1. ユーザーが画像をアップロード
2. POST /api/v1/materials で仮商品作成
3. sampleImageUrl でプレビュー表示
4. ユーザーが確認
   - OK → sampleUrl で購入ページへ
   - NG → DELETE で削除して再作成
```

## エラーハンドリング

| ステータス | 意味 | 対処 |
|-----------|------|------|
| 401 | 認証エラー | トークンを確認 |
| 422 | バリデーションエラー | パラメータを確認 |
| 429 | レートリミット | 時間をおいて再試行 |

## References

- [SUZURI API公式ドキュメント](https://suzuri.jp/developer/documentation/v1)
- [実装Tips](https://zenn.dev/yu_9/articles/7d372e293b260e)
