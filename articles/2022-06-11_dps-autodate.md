---
title: "denops.vim で autodate"
emoji: "🐜"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["vim", "deno", "denops"]
published: true
---

またまた [denops.vim](https://github.com/vim-denops/denops.vim) で `plugin` 作成しました！

`denops auto date !`

https://github.com/yukimemi/dps-autodate


<!-- more -->

## 機能概要

決められた書式で記載している文字列を置換するプラグインです。

特に設定をしないデフォルトだと、ファイル保存時に以下のように置換を行います。

- Before.

```markdown
Last Change: .
```

- After.

```markdown
Last Change: 2022/06/09 16:59:43.
```

日時は保存を行った時のものになります。


## インストールと設定

インストールは [dein.vim](https://github.com/Shougo/dein.vim) だとこんな感じ。

- dein.toml

```toml
[[plugins]]
repo = 'vim-denops/denops.vim'

[[plugins]]
repo = 'yukimemi/dps-autodate'
depends = 'denops.vim'
hook_add = '''
" デバッグオプション (v:true にするといっぱいログ出る。)
let g:autodate_debug = v:false
" 置換パターン (詳細は後述。以下はデフォルト設定)
let g:autodate_config = {
\ "*": {
\   "replace": [
\     ['/(.*Last Change.*: ).*\.$/i',
\     '$1${format(now, "yyyy/MM/dd HH:mm:ss")}.']
\   ],
\   "events": ["FileWritePre", "BufWritePre"],
\   "pat": "*",
\   "head": 13,
\   "tail": 13,
\ }
\ }
'''
```

## `g:autodate_config` 設定について

以下のように `filetype` 毎に指定が可能。
( `*` は全ファイルタイプで動作)

```vim
let g:autodate_config = {
 \ "filetype": {                            # vim のファイルタイプを指定
 \   "replace": [['before', 'after'], ...], # 置換パターン。 deno で line.replace(before, after) として実行される
                                            # 以下の変数、関数が使用できる
                                            #   now   : new Date()
                                            #   format: Deno format function (https://deno.land/std/datetime#format)
 \   "events": "event",                     # 置換動作発動条件。Vim の autocmd events. String or List が指定できる
 \   "pat": "aupat",                        # 置換動作発動条件。Vim の autocmd pattern. String or List が指定できる
 \   "head": 13,                            # 置換対象とする top からの行数
 \   "tail": 13,                            # 置換対象とする bottom からの行数
 \ },
 \ "filetype": ...
 \ }
```

### 例

```vim
let g:autodate_config = {
 \ "markdown": {
 \   "replace": [
 \     ['/(Last update\s*:).*/', '$1 ${format(now, "yyyy/MM/dd HH:mm")}'],
 \     ['/(date\s*:).*/', '$1 ${format(now, "yyyy/MM/dd")}'],
 \   ],
 \   "event": "BufWritePre",
 \   "pat": ["*.md", "*.mkd"],
 \   "head": 30,
 \   "tail": 10,
 \ },
 \ "txt": {
 \   "replace": [
 \     ['/(時計\s*:).*/', '$1 ${format(now, "yyyy/MM/dd HH:mm:ss.SSS")}']
 \   ],
 \   "event": ["BufWritePre", "CursorHold", "CursorMoved", "InsertLeave"],
 \   "pat": "*.txt",
 \   "head": 1,
 \   "tail": 0,
 \ }
 \ }
```

例えば上記のような設定の場合、 `markdown` ではファイル保存時に以下の動作となります。

#### markdown

- Before.

```markdown
- Last update:
- date:
```

- After.

```markdown
- Last update: 2022/06/11
- date: 2022/06/11
```

`txt` では以下のようになり、カーソル移動すると逐次更新されます。
処理が連続しすぎないように 1 秒未満の更新の場合は更新されないようにしています。
(こんな使い方はしないだろうけど・・・・)

- Before.

```txt
- 時計:
```

- After.

```txt
- 時計: 2022/06/11 22:44:14.253
```

## コマンド

コマンドで有効化 / 無効化ができる。

```vim
" 無効化
:DisableAutodate

" 有効化
:EnableAutodate
```

## インスパイア元

以下です。 ずっと使ってお世話になってました！ありがとうございました！

[vim-scripts/autodate.vim: A customizable plugin to update time stamps automatically.](https://github.com/vim-scripts/autodate.vim)
