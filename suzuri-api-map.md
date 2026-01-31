# SUZURI API Map

SUZURI APIエンドポイントとスキルの対応表。

## 認証 (Auth)

| メソッド | エンドポイント | スキル | 機能 |
|----------|----------------|--------|------|
| GET | `/oauth/authorize` | [suzuri-auth](skills/suzuri-auth/SKILL.md) | 認可リクエスト |
| POST | `/oauth/token` | [suzuri-auth](skills/suzuri-auth/SKILL.md) | トークン取得 |

## アクティビティ (Activity)

| メソッド | エンドポイント | スキル | 機能 |
|----------|----------------|--------|------|
| GET | `/api/v1/activities` | [suzuri-activity](skills/suzuri-activity/SKILL.md) | できごとリスト取得 |
| GET | `/api/v1/activities/unreads` | [suzuri-activity](skills/suzuri-activity/SKILL.md) | 未読数取得 |

## オモイデ (Choice)

| メソッド | エンドポイント | スキル | 機能 |
|----------|----------------|--------|------|
| GET | `/api/v1/choices` | [suzuri-choice](skills/suzuri-choice/SKILL.md) | リスト取得 |
| GET | `/api/v1/choices/{id}` | [suzuri-choice](skills/suzuri-choice/SKILL.md) | 情報取得 |
| POST | `/api/v1/choices` | [suzuri-choice](skills/suzuri-choice/SKILL.md) | 作成 |
| PUT | `/api/v1/choices/{id}` | [suzuri-choice](skills/suzuri-choice/SKILL.md) | 更新 |
| DELETE | `/api/v1/choices/{id}` | [suzuri-choice](skills/suzuri-choice/SKILL.md) | 削除 |
| POST | `/api/v1/choices/{id}` | [suzuri-choice](skills/suzuri-choice/SKILL.md) | 商品追加 |
| POST | `/api/v1/choices/{id}/remove` | [suzuri-choice](skills/suzuri-choice/SKILL.md) | 商品削除 |

## ズッキュン (Favorite)

| メソッド | エンドポイント | スキル | 機能 |
|----------|----------------|--------|------|
| GET | `/api/v1/products/{id}/favorites` | [suzuri-favorite](skills/suzuri-favorite/SKILL.md) | 商品へのズッキュン取得 |
| GET | `/api/v1/users/{id}/favorites` | [suzuri-favorite](skills/suzuri-favorite/SKILL.md) | ユーザのズッキュン取得 |
| POST | `/api/v1/products/{id}/favorites` | [suzuri-favorite](skills/suzuri-favorite/SKILL.md) | ズッキュン作成 |
| DELETE | `/api/v1/products/{id}/favorites` | [suzuri-favorite](skills/suzuri-favorite/SKILL.md) | ズッキュン削除 |

## アイテム (Item)

| メソッド | エンドポイント | スキル | 機能 |
|----------|----------------|--------|------|
| GET | `/api/v1/items` | [suzuri-item](skills/suzuri-item/SKILL.md) | アイテムリスト取得 |

## 素材 (Material)

| メソッド | エンドポイント | スキル | 機能 |
|----------|----------------|--------|------|
| GET | `/api/v1/materials` | [suzuri-material](skills/suzuri-material/SKILL.md) | リスト取得 |
| POST | `/api/v1/materials` | [suzuri-material](skills/suzuri-material/SKILL.md) | 作成 |
| PUT | `/api/v1/materials/{id}` | [suzuri-material](skills/suzuri-material/SKILL.md) | 更新 |
| DELETE | `/api/v1/materials/{id}` | [suzuri-material](skills/suzuri-material/SKILL.md) | 削除 |
| POST | `/api/v1/materials/text` | [suzuri-material](skills/suzuri-material/SKILL.md) | テキストから作成 |

## 商品 (Product)

| メソッド | エンドポイント | スキル | 機能 |
|----------|----------------|--------|------|
| GET | `/api/v1/products` | [suzuri-product-manage](skills/suzuri-product-manage/SKILL.md) | リスト取得 |
| GET | `/api/v1/products/{id}` | [suzuri-product-manage](skills/suzuri-product-manage/SKILL.md) | 情報取得 |
| GET | `/api/v1/products/search` | [suzuri-product-manage](skills/suzuri-product-manage/SKILL.md) | 検索 |
| GET | `/api/v1/products/on_sale` | [suzuri-product-manage](skills/suzuri-product-manage/SKILL.md) | セール商品取得 |
| GET | `/api/v1/choices/{id}/products` | [suzuri-product-manage](skills/suzuri-product-manage/SKILL.md) | Choice内商品取得 |
| POST | `/api/v1/materials` | [suzuri-product-create](skills/suzuri-product-create/SKILL.md) | 商品作成（素材経由） |

## ユーザー (User)

| メソッド | エンドポイント | スキル | 機能 |
|----------|----------------|--------|------|
| GET | `/api/v1/user` | [suzuri-user](skills/suzuri-user/SKILL.md) | 認証ユーザ情報取得 |
| PUT | `/api/v1/user` | [suzuri-user](skills/suzuri-user/SKILL.md) | 認証ユーザ情報更新 |
| GET | `/api/v1/users` | [suzuri-user](skills/suzuri-user/SKILL.md) | ユーザーリスト取得 |
| GET | `/api/v1/users/{id}` | [suzuri-user](skills/suzuri-user/SKILL.md) | ユーザー情報取得 |

## スキル一覧

| スキル名 | 機能 | トリガーキーワード |
|----------|------|-------------------|
| suzuri-auth | 認証設定 | SUZURI認証, セットアップ |
| suzuri-activity | アクティビティ | 通知, できごと |
| suzuri-choice | オモイデ管理 | コレクション, オモイデ |
| suzuri-favorite | ズッキュン | お気に入り, いいね |
| suzuri-item | アイテム情報 | 商品種類, アイテム一覧 |
| suzuri-material | 素材管理 | デザイン, 素材アップロード |
| suzuri-product-create | 商品作成 | グッズ作成, Tシャツ作成 |
| suzuri-product-manage | 商品管理 | 商品一覧, 検索 |
| suzuri-user | ユーザー管理 | プロフィール, ユーザー情報 |

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
