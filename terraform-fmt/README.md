# terraform-fmt

`terraform fmt` を自動実行し、フォーマットの差分があれば自動コミット・プッシュする composite action です。

## 使い方

```yaml
- uses: maooz4426/maoz-workflows/terraform-fmt@main
  with:
    head_ref: ${{ github.head_ref }}
```

## Inputs

| 名前 | 必須 | 説明 |
|------|------|------|
| `head_ref` | ✓ | プッシュ先のブランチ名（例: `github.head_ref`） |


## Example

```yaml
name: Terraform fmt

on:
  pull_request:

jobs:
  terraform-fmt:
    runs-on: ubuntu-latest
    permissions:
      contents: write
    steps:
      - uses: maooz4426/maoz-workflows/terraform-fmt@main
        with:
          head_ref: ${{ github.head_ref }}
```
