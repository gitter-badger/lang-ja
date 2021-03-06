# vim-jp/lang-ja

Vimに付属する日本語翻訳ファイルを管理するリポジトリ

## ディレクトリ/ファイル 解説

パス |説明
-----|-----
src/po/, runtime/lang |Vimに付属の日本語翻訳ファイルが置いてあります。
src/po/ja.po |Vimのメッセージ翻訳ファイルのマスター(UTF-8)
runtime/lang/menu\_ja\_jp.utf-8.vim |Vimの日本語メニューファイルのマスター(UTF-8)
runtime/doc/\*-ja.UTF-8.1 |日本語manファイル(UTF-8)
runtime/doc/\*.1 |原文manファイル
runtime/tutor/tutor.ja.utf-8 |日本語チュートリアルファイル(UTF-8)
runtime/tutor/tutor |原文チュートリアルファイル

## ja.po 更新手順

1.  vim.pot を作成(要xgettext)

    Vimのソースで以下を実行して、生成される vim.pot を src/po へコピー

        $ cd src/po
        $ make vim.pot

2.  ja.po に vim.pot をマージ (古いのは ja.po.old へ退避します)

        $ make merge

3.  ja.po のコピーライトやヘッダーを適宜修正

    これはPRを作るだけの場合は、やらないほうが良いかも。

4.  翻訳する

    Vimを使って下記の検索コマンドで翻訳すべき場所を探すと良い。

        /fuzzy\|^msgstr ""\(\n"\)\@!

5.  不要な情報の削除

    Vim で以下のようにする。

        :source cleanup.vim

    cleanup.vim は Vim 本体からのコピー

6.  チェック

        $ vim -S check.vim ja.po

7.  もう1回マージして、整形と消しすぎたコメントの復活

        $ make merge-force
        $ vim ja.po
        :source cleanup.vim
        :wq

## manファイル更新手順

1.  原文manファイルの更新

    Vimのソースファイルの runtime/doc/ ディレクトリから、原文manファイルを本リポジトリにコピー。

        $ cd /path/to/vim/runtime/doc
        $ cp evim.1 vim.1 vimdiff.1 vimtutor.1 xxd.1 /path/to/lang-ja/runtime/doc

2.  翻訳

    原文の差分を見つつ翻訳ファイルを更新する。

        $ git diff | gvim -R -

3.  表示確認

    以下のコマンドで表示を確認できる。

        $ groff -Tutf8 -Dutf8 -mandoc -mja vim-ja.UTF-8.1 | less -R

4.  コミット

    原文と日本語訳は常に同じバージョンがコミットされているように注意すること。

## チュートリアルファイル更新手順

1.  原文チュートリアルファイルの更新

    Vimのソースファイルの runtime/tutor/ ディレクトリから、原文チュートリアルファイルを本リポジトリにコピー。

        $ cd /path/to/vim/runtime/tutor
        $ cp tutor /path/to/lang-ja/runtime/tutor

2.  翻訳

    原文の差分を見つつ翻訳ファイルを更新する。

        $ git diff | gvim -R -

3.  コミット

    原文と日本語訳は常に同じバージョンがコミットされているように注意すること。

## リリース手順

以下を実行してください。

    $ make release-today

`vim-lang-ja-20160131.tar.xz` のようなアーカイブファイルができます。
`20160131` の部分は実行した日付に置き換わります。
あとはこのアーカイブファイルを vim-dev へ更新依頼とともに送信します。
