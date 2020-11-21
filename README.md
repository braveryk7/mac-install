# mac-install

このリポジトリはCiexionの開発用機材(Mac)の再インストールに必要な手順を示したものです。

標準言語(Python)利用までの手順を遵守してください。

## Google Chromeのインストール
Safariから[Google Chrome](https://www.google.com/intl/ja_jp/chrome/) へアクセスし、ダウンロードしてインストールしてください。

ブラウザはGoogle Chromeをデフォルトブラウザとしてください。

## VSCodeのインストール
開発用エディタはVSCodeを採用します。

[VSCode](https://azure.microsoft.com/ja-jp/products/visual-studio-code/) へアクセスし、ダウンロードしてインストールしてください。


VSCodeインストール後は、日本語環境の利用のため以下の記事に従って日本語環境を構築してください。

記事タイトルにWindowsとなっていますがMacも同様の手順で設定できます。

[Visual Studio Codeで日本語化する方法[Windows] - Qiita](https://qiita.com/nanamesincos/items/5c48ff88a4eeef0a8631)

## Homebrewのインストール
Macのパッケージ管理ソフトウェアとしてHomebrewを採用します。

[Homebrew](https://brew.sh/index_ja) にアクセスします。

Homebrewインストールコマンドをコピーし、ターミナル上で実行してHomebrewをインストールしてください。

インストールが完了したら以下のコマンドで正常にインストールされた確認してください。

```
$ brew --version
```

## Gitのインストール
Macには標準でGitがインストールされていますが、バージョンが古いため最新のGitを利用します。

GitはHomebrew経由でインストールします。

現在のgitのバージョンを確認
```
$ git --version
git version 2.20.0 (Apple Git-117)
```

現在のgitのpathを確認

```
$ which git
/usr/bin/git
```

homebrewでgitをインストール

```
$ brew install git
```

インストール後、gitがインストールされたか確認

一覧にgitがあればOK

```
$ brew list
```

gitをMac標準のgitからHomebrewのgitにpathを向ける

viでzsh用の設定ファイルを開く

```
$ vi ~/.zshrc
```

以下の文言をzshに追記

iキーで編集モード、escキーでコマンドモードに切り替え

コマンドモードで:wqで上書き保存、:qで保存せずに終了、:q!で保存せずに強制終了

```
PATH=/usr/local/bin/git:$PATH
export PATH
```

zshの変更を再読み込み

```
source ~/.zshrc
```

gitのバージョンを確認

最初のバージョンから変わっていればOK

```
$ git --version
git version 2.21.0
```

pathも確認

```
$ which git
/usr/local/bin/git
```

gitのユーザー名とメールアドレスを設定

usernameはGitHubのIDでOK

メールアドレスは会社のメールアドレス、もしくはGitHubのダミーメールアドレス

```
$ git config --global user.name "username"
$ git config --global user.email "user@example.com"
```

## GitHubとの連携
GitHubと接続するためのSSHキーを生成、GitHubに公開鍵の登録を行います。

ホームディレクトリ直下に.sshディレクトリ作成
```
$ mkdir ~/.ssh
```

秘密鍵/公開鍵の作成

メールアドレスはgit configで設定したメールアドレスを入力
```
$ ssh-keygen -t ed25519 -N " -f ~/.ssh/github -C user@example.com
```

~/.ssh/github以下にgithub.pub（公開鍵）とgithub（秘密鍵）という2つのキーペアが生成されている

公開鍵をコピー
```
$ pbcopy < ~/.ssh/github.pub
```

GitHubにログインした状態でhttps://github.com/settings/keys にアクセスし、New SSH keyをクリック

Titleはわかりやすいもの（例：Mac）を入力し、Keyにペースト

Add SSH Keyで保存


configファイルに接続情報を入力する
```
$ vi ~/.ssh/config
```

以下を入力

```
Host github.com
    IdentityFile ~/.ssh/github
    User git
```

パーミッションを設定

```
$ chmod 700 ~/.ssh
$ chmod 600 ~/.ssh/*
```

GitHubへの接続を確認

```
$ ssh -T github.com
Warning: Permanently added 'github.com,52.69.186.44' (RSA) to the list of known hosts.
Hi USERNAME! You've successfully authenticated, but GitHub does not provide shell access.
```
