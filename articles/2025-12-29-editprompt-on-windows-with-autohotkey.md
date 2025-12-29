---
title: Windows でも手軽に editprompt したい！ AutoHotkey で自作する
emoji: "\U0001F4BB"
type: tech
topics:
  - windows
  - autohotkey
  - cli
  - gemini
published: true
---

## はじめに

最近、Gemini CLI や Claude Code など、ターミナルで AI と対話する機会が増えてきました。
でも、ターミナルの入力欄って長文を書くのには向いてないですよね...。改行やカーソル移動が面倒だし、全体を見渡しにくい！

そんな時に見つけたのが [editprompt](https://github.com/eetann/editprompt) というツール。
CLI への入力を、一時的に `$EDITOR` (Vim など) で編集できるようにしてくれます。

ただこのツール、Windows で快適に（特にエディタ終了後の自動送信まで含めて）使おうとすると、WezTerm や Bash などの環境依存が強めで、導入のハードルがちょっと高そうでした。
(実際はちゃんと調べられてない)
サクッと使いたいだけなのに、環境構築でハマるのは避けたいところ。

というわけで、**AutoHotkey** を使って同じような機能を自作してみました！

## AutoHotkey スクリプト

スクリプトは GitHub で管理している dotfiles に置いてます。
これを AutoHotkey の設定ファイルに追記するだけです。

https://github.com/yukimemi/dotfiles/blob/9f6004a50d1edc7394a635c428ff03c4b1fed94c/win/AutoHotkey/AutoHotkey.ahk#L289-L310

### やってること

1.  ターミナル（Windows Terminal とか）で Gemini CLI や Claude Code などを起動して、プロンプトで `F7` 。
2.  一時ファイルが作られて、設定したエディタ（自分は Neovim）が起動。
3.  好きなエディタでプロンプトを書く。保存して閉じる。
4.  書いた内容が勝手にターミナルにペーストされる！

これだけです。シンプル！

### カスタマイズ

リンク先のコードは `nvim` 固定にしちゃってますが、`RunWait` の部分を書き換えれば VS Code (`code --wait`) とかでも動くはず。

これで Windows でも快適に AI 利用してコードがかける！
