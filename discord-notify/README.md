# discord-notify

ジョブの結果を Discord の Webhook に embed で通知する composite action です。

- すべてのジョブが `success` のとき `success`、いずれかが `cancelled` のとき `cancelled`、それ以外は `failure` として通知します。
- `needs` に渡した各ジョブの結果を embed のフィールドに表示します。
- `mention-user-id` を指定すると、`success` と `failure` のときにそのユーザーをメンションします。

## 使い方

```yaml
- uses: maooz4426/maoz-workflows/discord-notify@main
  with:
    webhook-url: ${{ secrets.DISCORD_WEBHOOK_URL }}
    title: my-app deploy
    needs: ${{ toJSON(needs) }}
```

## Inputs

| 名前 | 必須 | 説明 |
|------|------|------|
| `webhook-url` | | Discord の Webhook URL。空のときは警告を出して通知をスキップする |
| `title` | ✓ | 通知のタイトル。末尾に `: <status>` を付ける |
| `needs` | ✓ | 呼び出し元の `${{ toJSON(needs) }}` |
| `mention-user-id` | | `success` と `failure` のときにメンションする Discord のユーザー ID |

## Example

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - run: echo build

  deploy:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - run: echo deploy

  notify:
    needs: [build, deploy]
    if: always()
    runs-on: ubuntu-latest
    steps:
      - uses: maooz4426/maoz-workflows/discord-notify@main
        with:
          webhook-url: ${{ secrets.DISCORD_WEBHOOK_URL }}
          title: my-app deploy
          needs: ${{ toJSON(needs) }}
          mention-user-id: ${{ vars.DISCORD_MENTION_USER_ID }}
```
