---
name: suzuri-api-guide
description: SUZURI API完全ガイド。認証設定、グッズ作成、商品管理、素材管理、ユーザー管理など全APIエンドポイントをサポート。「SUZURI」「グッズ作成」「商品管理」などのリクエスト時に使用。
allowed-tools: Read, Write, Edit, Bash
---

# SUZURI API Guide

SUZURI APIの完全ガイド。全エンドポイントとその使い方を網羅。

## API Map（エンドポイント一覧）

### 認証 (Auth)

| メソッド | エンドポイント | 機能 |
|----------|----------------|------|
| GET | `/oauth/authorize` | 認可リクエスト |
| POST | `/oauth/token` | トークン取得 |

### アクティビティ (Activity)

| メソッド | エンドポイント | 機能 |
|----------|----------------|------|
| GET | `/api/v1/activities` | できごとリスト取得 |
| GET | `/api/v1/activities/unreads` | 未読数取得 |

### オモイデ (Choice)

| メソッド | エンドポイント | 機能 |
|----------|----------------|------|
| GET | `/api/v1/choices` | リスト取得 |
| GET | `/api/v1/choices/{id}` | 情報取得 |
| POST | `/api/v1/choices` | 作成 |
| PUT | `/api/v1/choices/{id}` | 更新 |
| DELETE | `/api/v1/choices/{id}` | 削除 |
| POST | `/api/v1/choices/{id}` | 商品追加 |
| POST | `/api/v1/choices/{id}/remove` | 商品削除 |

### ズッキュン (Favorite)

| メソッド | エンドポイント | 機能 |
|----------|----------------|------|
| GET | `/api/v1/products/{id}/favorites` | 商品へのズッキュン取得 |
| GET | `/api/v1/users/{id}/favorites` | ユーザのズッキュン取得 |
| POST | `/api/v1/products/{id}/favorites` | ズッキュン作成 |
| DELETE | `/api/v1/products/{id}/favorites` | ズッキュン削除 |

### アイテム (Item)

| メソッド | エンドポイント | 機能 |
|----------|----------------|------|
| GET | `/api/v1/items` | アイテムリスト取得 |

### 素材 (Material)

| メソッド | エンドポイント | 機能 |
|----------|----------------|------|
| GET | `/api/v1/materials` | リスト取得 |
| POST | `/api/v1/materials` | 作成 |
| PUT | `/api/v1/materials/{id}` | 更新 |
| DELETE | `/api/v1/materials/{id}` | 削除 |
| POST | `/api/v1/materials/text` | テキストから作成 |

### 商品 (Product)

| メソッド | エンドポイント | 機能 |
|----------|----------------|------|
| GET | `/api/v1/products` | リスト取得 |
| GET | `/api/v1/products/{id}` | 情報取得 |
| GET | `/api/v1/products/search` | 検索 |
| GET | `/api/v1/products/on_sale` | セール商品取得 |
| GET | `/api/v1/choices/{id}/products` | Choice内商品取得 |

### ユーザー (User)

| メソッド | エンドポイント | 機能 |
|----------|----------------|------|
| GET | `/api/v1/user` | 認証ユーザ情報取得 |
| PUT | `/api/v1/user` | 認証ユーザ情報更新 |
| GET | `/api/v1/users` | ユーザーリスト取得 |
| GET | `/api/v1/users/{id}` | ユーザー情報取得 |

---

## 認証 (Authentication)

SUZURI APIは2つの認証方式をサポート。

### APIキー認証（開発用）

個人用トークン。`read`と`write`スコープを持つ。

```bash
# 環境変数に設定
export SUZURI_API_TOKEN="your_token_here"

# リクエスト例
curl -H "Authorization: Bearer $SUZURI_API_TOKEN" \
  https://suzuri.jp/api/v1/user
```

### OAuth認証（アプリケーション用）

他ユーザーの代わりに操作する場合に使用。

```
1. GET /oauth/authorize でユーザーを認証ページへリダイレクト
2. ユーザーが許可
3. POST /oauth/token でアクセストークン取得
```

**注意**: コールバックURLはHTTPS必須。ローカル開発にはOrbstackなどを使用。

### トークンの取得方法

1. [SUZURI開発者ページ](https://suzuri.jp/developer/documentation/v1) にアクセス
2. 「アプリケーションを登録」または「個人用トークンを発行」
3. トークンを安全に保存

### 接続テスト

```bash
curl -s -H "Authorization: Bearer $SUZURI_API_TOKEN" \
  https://suzuri.jp/api/v1/user | jq .
```

### レートリミット

レスポンスヘッダーで確認：
- `X-Ratelimit-Limit`: 上限
- `X-Ratelimit-Remaining`: 残り
- `X-Ratelimit-Reset`: リセット時刻

---

## アクティビティ (Activity)

### アクティビティ一覧を取得

```bash
curl -s -H "Authorization: Bearer $SUZURI_API_TOKEN" \
  https://suzuri.jp/api/v1/activities | jq '.activities[]'
```

### 未読数を取得

```bash
curl -s -H "Authorization: Bearer $SUZURI_API_TOKEN" \
  https://suzuri.jp/api/v1/activities/unreads | jq '.count'
```

---

## オモイデ (Choice)

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

---

## ズッキュン (Favorite)

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

---

## アイテム (Item)

### アイテム一覧を取得

```bash
curl -s -H "Authorization: Bearer $SUZURI_API_TOKEN" \
  https://suzuri.jp/api/v1/items | jq '.items[] | {id, name, humanizeName}'
```

### 主なアイテム

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

---

## 素材 (Material)

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

---

## 商品作成 (Product Create)

### ワークフロー

1. ユーザーが画像をアップロード
2. POST /api/v1/materials で仮商品作成
3. sampleImageUrl でプレビュー表示
4. ユーザーが確認
   - OK → sampleUrl で購入ページへ
   - NG → DELETE で削除して再作成

### 商品プレビューを作成

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

### プレビュー画像

レスポンスから以下を使用：
- `sampleImageUrl`: WebP形式（推奨）
- `pngSampleImageUrl`: PNG形式

### プレビューをキャンセル

```bash
curl -X DELETE \
  -H "Authorization: Bearer $SUZURI_API_TOKEN" \
  https://suzuri.jp/api/v1/materials/{material_id}
```

---

## 商品管理 (Product Manage)

### 商品一覧を取得

```bash
curl -s -H "Authorization: Bearer $SUZURI_API_TOKEN" \
  https://suzuri.jp/api/v1/products | jq '.products[] | {id, title, sampleUrl}'
```

### 商品を検索

```bash
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

### 便利なjqフィルタ

```bash
# 商品タイトルと価格のみ
jq '.products[] | {title, price}'

# 公開中の商品のみ
jq '.products[] | select(.published == true)'

# 価格順でソート
jq '.products | sort_by(.price)'
```

---

## ユーザー (User)

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

---

## エラーハンドリング

| ステータス | 意味 | 対処 |
|-----------|------|------|
| 401 | 認証エラー | トークンを確認 |
| 422 | バリデーションエラー | パラメータを確認 |
| 429 | レートリミット | 時間をおいて再試行 |

---

## クイックリファレンス

```bash
# 認証テスト
curl -H "Authorization: Bearer $SUZURI_API_TOKEN" https://suzuri.jp/api/v1/user

# アイテム一覧
curl -H "Authorization: Bearer $SUZURI_API_TOKEN" https://suzuri.jp/api/v1/items

# 商品検索
curl -H "Authorization: Bearer $SUZURI_API_TOKEN" "https://suzuri.jp/api/v1/products/search?q=猫"

# 商品作成
curl -X POST -H "Authorization: Bearer $SUZURI_API_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"texture":"https://example.com/image.png","title":"タイトル"}' \
  https://suzuri.jp/api/v1/materials
```

## References

- [SUZURI API公式ドキュメント](https://suzuri.jp/developer/documentation/v1)
- [SUZURI API Tips集](https://zenn.dev/yu_9/articles/7d372e293b260e)
