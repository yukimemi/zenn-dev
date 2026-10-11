# Zenn Contents

* [✍️ How to use](https://zenn.dev/zenn/articles/zenn-cli-guide)
* [✍️ Markdown guide](https://zenn.dev/zenn/articles/markdown-guide)

クリーン環境で依存関係の再現性と CLI の起動を確認するには、リポジトリのルートで次を実行します。
既存の `node_modules` を使わず、`bun.lock` を固定してインストールし、一時ディレクトリは終了時に削除します。

```sh
(
  set -eu
  verify_dir=$(mktemp -d)
  trap 'rm -rf "$verify_dir"' EXIT
  cp package.json bun.lock "$verify_dir/"
  cd "$verify_dir"
  bun install --frozen-lockfile
  bunx zenn --version
)
```
