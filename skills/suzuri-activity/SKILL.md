---
name: suzuri-activity
description: SUZURIのアクティビティ（できごと）を取得。通知確認、未読数取得。「SUZURIの通知」「アクティビティ確認」などのリクエスト時に使用。
allowed-tools: Bash
---

# SUZURI Activity

SUZURIのアクティビティ（できごと）を管理するスキル。

## Endpoints

| メソッド | エンドポイント | 機能 |
|----------|----------------|------|
| GET | `/api/v1/activities` | できごとのリスト取得 |
| GET | `/api/v1/activities/unreads` | 未読できごと数取得 |

## Instructions

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

## Response Example

```json
{
  "activities": [
    {
      "id": 12345,
      "type": "favorite",
      "message": "〇〇さんがズッキュンしました",
      "createdAt": "2024-01-01T12:00:00Z",
      "read": false
    }
  ]
}
```
