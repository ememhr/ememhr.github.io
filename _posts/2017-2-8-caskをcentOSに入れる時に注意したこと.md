---
layout: post
title: emacsのパッケージをcaskで管理する上で気をつけていること
---

# emacs のパッケージ管理

- emacs 25.1
- cask
を使ってパッケージの管理をしていたりします  

一応githubにemacsのパッケージリストは公開中  
<a href="https://github.com/ememhr/my_emacs_conf" target="_blank">my emacs conf</a>


# えぐさま

1. Vagrantで立ち上げた仮想環境でcaskが動作しない
2. Macでcaskを入れる場合とcentOSでcaskを入れる際の手順が違っていた
3. centOSでcaskを入れる
4. 作成済み.emacs.dのディレクトリ内にあるCaskを使ってパッケージをインストールする

# 手順とハマったところ

## 手順

1. centOSにyumでemacsを入れる
2. centOSにcaskを入れる
3. caskでパッケージをインストールする

### centOSにyumでemacsを入れる

まず最初にここでハマりました。

```bash
$ sudo yum install -y emacs
# emacs 23.* がインストールされてしまう
```

yumで管理されているemacsのパッケージが古く、自分の用意したemacs のcaskで管理しているパッケージが25.1用だったので、cask登録時にバージョン違いのエラーが起こりました。

そこでcentOSにemacs 25.1 を入れます。  
yumで入れられないので、ソースからコンパイルします。

このあたり参考にしました。  
<a href='http://qiita.com/maangie/items/2c8c4123b00c6fb6c8d5' target='_blank'>Installing Emacs 25.1.1 on CentOS 6.8</a>


```bash
$ sudo yum install -y xz
$ curl http://core.ring.gr.jp/pub/GNU/emacs/emacs-25.1.tar.xz | tar Jxf -
$ cd emacs-25.1/
$ ./configure --without-x
$ make
$ sudo make install
$ emacs --version
GNU Emacs 25.1.1
Copyright (C) 2016 Free Software Foundation, Inc.
GNU Emacs comes with ABSOLUTELY NO WARRANTY.
You may redistribute copies of GNU Emacs
under the terms of the GNU General Public License.
For more information about these matters, see the file named COPYING.
```

### caskをインストールする

Macではbrewで提供されているので、brew入れた後にパス通してくれますが、centOSはソースからビルドするので、パスを通す必要あり。

```bash
$ curl -fsSL https://raw.githubusercontent.com/cask/cask/master/go | python
$ export PATH="$HOME/.cask/bin:$PATH"
```

ここでcaskのファイルはリポジトリの中にある  
caskのコマンドを実行した時に、すでにインストールしていた.emacs.d のディレクトリ配下の.cask以下にコマンドを移動する

## caskでパッケージ入れる

```bash
# コマンドは以下
$ /.cask/.cask/bin/cask
$ cd .emacs.d/
$ cask
# cask-bootstrap cask-cli cask がなくcaskが実行できない
# この状態でemacsを起動するとcaskがないと言われて起動できない

```

Macでbrew経由でcaskを入れた場合はこのあたりをよしなにしてくれていたけど、CentOSでソースから入れた場合はcaskの立ち上げ*.elファイルを.emacs.d/.cask配下に置かないとcaskが起動しない。

```bash
$ cd
$ cp .cask/cask-bootstrap.el ../.emacs.d/.cask/
$ cp .cask/cask.el ../.emacs.d/.cask/
$ cp .cask/cask-cli.el ../.emacs.d/.cask
$ cd ../.emacs.d/
$ cask
# これでcaskが実行されてパッケージが入る
```

