---
title: "Simple vim statusline plugin barow !"
emoji: "😺"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["vim", "neovim", "plugin"]
published: true
---

Vim の simple な statusline plugin があったので使用してみた。

[doums/barow: A minimalist statusline for n/vim](https://github.com/doums/barow)
![barow](https://github.com/doums/barow/blob/master/img/barow.png?raw=true)

<!-- more -->

僕は最近結局 [dein.vim](https://github.com/Shougo/dein.vim) を使用しているので以下のような設定。
[barow](https://github.com/doums/barow) に付随する、 [lsp](https://github.com/doums/barowLSP) と [git](https://github.com/doums/barowGit) の plugin も追加している。

設定内容はそれぞれの README にあったとおり。細かい内容はわかってません・・・。

- dein.toml

```toml
[[plugins]]
repo = "doums/barow"
depends = ["barowLSP", "barowGit"]
on_event = ['CursorHold', 'FocusLost']
hook_add = '''
source $VIM_PATH/rc/barow.vim
'''

[[plugins]]
repo = "doums/barowLSP"
lazy = 1
[[plugins]]
repo = "doums/barowGit"
lazy = 1
```

- $VIM_PATH/rc/barow.vim

```vim
let g:barow = {
      \  'modes': {
      \    'normal'  : [' ', 'BarowNormal'],
      \    'insert'  : ['i', 'BarowInsert'],
      \    'replace' : ['r', 'BarowReplace'],
      \    'visual'  : ['v', 'BarowVisual'],
      \    'v-line'  : ['l', 'BarowVisual'],
      \    'v-block' : ['b', 'BarowVisual'],
      \    'select'  : ['s', 'BarowVisual'],
      \    'command' : ['c', 'BarowCommand'],
      \    'shell-ex': ['!', 'BarowCommand'],
      \    'terminal': ['t', 'BarowTerminal'],
      \    'prompt'  : ['p', 'BarowNormal'],
      \    'inactive': [' ', 'BarowModeNC']
      \  },
      \  'statusline': ['Barow', 'BarowNC'],
      \  'tabline': ['BarowTab', 'BarowTabSel', 'BarowTabFill'],
      \  'buf_name': {
      \    'empty': '',
      \    'hi'   : ['BarowBufName', 'BarowBufNameNC']
      \  },
      \  'read_only': {
      \    'value': 'ro',
      \    'hi': ['BarowRO', 'BarowRONC']
      \  },
      \  'buf_changed': {
      \    'value': '*',
      \    'hi': ['BarowChange', 'BarowChangeNC']
      \  },
      \  'tab_changed': {
      \    'value': '*',
      \    'hi': ['BarowTChange', 'BarowTChangeNC']
      \  },
      \  'line_percent': {
      \    'hi': ['BarowLPercent', 'BarowLPercentNC']
      \  },
      \  'row_col': {
      \    'hi': ['BarowRowCol', 'BarowRowColNC']
      \  },
      \  'modules': [
      \    [ 'barowGit#branch', 'BarowHint' ],
      \    [ 'barowGit#branch', 'StatusLine' ],
      \    [ 'barowLSP#ale_status', 'StatusLine' ],
      \    [ 'barowLSP#coc_status', 'StatusLine' ],
      \    [ 'barowLSP#error', 'BarowError' ],
      \    [ 'barowLSP#hint', 'BarowHint' ],
      \    [ 'barowLSP#info', 'BarowInfo' ],
      \    [ 'barowLSP#warning', 'BarowWarning' ],
      \  ]
      \ }
```

うん、シンプルだし、ちょっと気分転換にはいいかも！

## 参考

---

- [doums/barow: A minimalist statusline for n/vim](https://github.com/doums/barow)
- [doums/barowLSP: n/vim plugin to display LSP info in barow statusline](https://github.com/doums/barowLSP)
- [doums/barowGit: A module to display the current git branch in barow statusline](https://github.com/doums/barowGit)
