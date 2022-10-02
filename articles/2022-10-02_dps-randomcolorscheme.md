---
title: "denops.vim で random colorscheme !"
emoji: "🐜"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["vim", "deno", "denops", "neovim"]
published: true
---

またまた [denops.vim](https://github.com/vim-denops/denops.vim) で `plugin` 作成しました！

`denops random colorscheme !`

https://github.com/yukimemi/dps-randomcolorscheme


<!-- more -->

## 機能概要

`colorscheme` をランダムに変更するやつです。
デフォルトだと起動時、及び 3,600 秒 (1時間) ごとに変更を行います。


## インストールと設定

インストールは [dein.vim](https://github.com/Shougo/dein.vim) だとこんな感じ。

- dein.toml

```toml
[[plugins]]
repo = 'vim-denops/denops.vim'

[[plugins]]
repo = 'yukimemi/dps-randomcolorscheme'
depends = 'denops.vim'
hook_add = '''
" 特に設定は不要ですが、以下のオプションが指定可能
" デバッグオプション (v:true にするといっぱいログ出る。Default: v:false)
let g:randomcolorscheme_debug = v:false
" colorscheme 変更時に colorscheme 名を echo するかどうか (Default: v:true)
let g:randomcolorscheme_echo = v:true

" 使用する colorscheme (Default: [] (全て))
let g:randomcolorscheme_enables = ["morning", "ron"]
" 使用しない colorscheme (Default: [])
let g:randomcolorscheme_disables = ["evening", "default"]

" colorscheme 変更するインターバル (秒) (Default: 3600)
let g:randomcolorscheme_interval = 600
" colorscheme 変更するイベント (Default: [])
let g:randomcolorscheme_events = ["CursorHold", "FocusLost", "BufWritePost"]
'''
```

`g:randomcolorscheme_enables` を設定すると指定された colorscheme の中からのみ選ばれます。
`g:randomcolorscheme_disables` を設定すると指定された colorscheme は選択の対象外になります。

変更のトリガーは時間 (デフォルト 3600秒 [1時間]) とイベントの指定 (デフォルトはなし) ができます。

`let g:randomcolorscheme_interval = 1` とかにすれば・・・

ユナイトビューティフルアタッ・・・・


