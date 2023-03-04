---
title: "denops.vim で neovim-remote 代替 !"
emoji: "🐜"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["vim", "deno", "denops", "neovim"]
published: false
---

またまた [denops.vim](https://github.com/vim-denops/denops.vim) で `plugin` 作成しました！

`denops hitori !`

https://github.com/yukimemi/dps-hitori

![denops-hitori](https://github.com/yukimemi/zenn-dev/blob/master/articles/img/dps-hitori_001.gif?raw=true)

<!-- more -->

## 機能概要

`Neovim` が起動中の場合は新しくインスタンスを立ち上げるのではなく、既に起動中の `Neovim` を使用する、というものです。
[denops.vim](https://github.com/vim-denops/denops.vim) で Websocket を立ち上げて起動済みの場合はパスを送信しています。

`Vim` だと、 `--remote` とかオプションがあるやつです。
また、 `Vim` の場合は thinca さん作成の素晴らしいプラグイン [vim-singleton](https://github.com/thinca/vim-singleton) があります。
(僕も Vim を使用していた時はずっとお世話になっていました。)

`Neovim` も [neovim-remote](https://github.com/mhinz/neovim-remote) とかあるにはあるのですが、 `Windows` に優しくなかったので作っちゃいました。

## インストールと設定

インストールは [folke/lazy.nvim](https://github.com/folke/lazy.nvim) だとこんな感じです。

- dps-hitori.lua

```lua
return {
  "yukimemi/dps-hitori",

  lazy = false,

  dependencies = {
    "vim-denops/denops.vim",
  },

  init = function()
    " デバッグモード (設定すると echo いっぱい出る)
    vim.g.hitori_debug = false
    " 有効 / 無効
    vim.g.hitori_enable = true
    " 既に起動中の Neovim にパスを送信した後に Neovim を終了するかどうか
    vim.g.hitori_quit = true
    " Websocket サーバの port
    vim.g.hitori_port = 7070

    " 除外するファイルパターン。デフォルトは [] です。
    vim.g.hitori_blacklist_patterns = { "\\.tmp$", "\\.diff$", "(COMMIT_EDIT|TAG_EDIT|MERGE_|SQUASH_)MSG$" }
  end,
}
```

設定部分は特に設定しなくてもデフォルト設定があるので不要です。
`g:hitori_blacklist_patterns` だけは設定したほうがいいかもです。


## ラップコマンド `hitori` ！

[denops.vim](https://github.com/vim-denops/denops.vim) のプラグインである以上、一度 `neovim` が立ち上がって、 `denops` が load されてからしか動作しません。
`Linux` や `Mac` だと十分速いのであまり気にならないのですが、 `Windows` だとそうもいきません・・・。

なので、 `Neovim` 起動前に `Websocket` でサーバが起動していたら `Neovim` は起動せずにパスだけ `Websocket` で送信する、というラップコマンドを [Deno](https://deno.land/) で書きました。

以下のコマンドでインストールできます。


- `nvim` を起動する場合

```sh
deno install --allow-net --allow-run --allow-read --name hitori https://raw.githubusercontent.com/yukimemi/dps-hitori/main/cmd/hitori.ts
```

- `nvim-qt` を起動する場合

```sh
deno install --allow-net --allow-run --allow-read --name hitori https://raw.githubusercontent.com/yukimemi/dps-hitori/main/cmd/hitori-qt.ts
```

`hitori` コマンドがインストールされるので、以降は `hitori` を実行すれば複数の `Neovim` が起動されるのを防ぐことができます。



