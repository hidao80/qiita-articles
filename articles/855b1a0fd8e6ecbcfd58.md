---
title: 秘密鍵接続を使ってWindowsのGit Bashでcloneする【conoha ssh】
tags: Linux SSH Windows Git
author: hidao
slide: false
---
## 前提

1. WindowsにGitとGit bashがインストールされていること。
2. conohaにLinuxサーバを立てていること。
3. Linuxサーバにgitがインストールされており、アクセス可能な場所にリポジトリがあること。

## 手順

1. conohaの管理ページにログインする。
2. conohaの管理ページからsshポートを開放する。
3. conohaのゲストOS側のsshポートを開放する（firewall-cmdなどを使う）。
3. conohaで公開鍵（パブリックキー）を作成する。同時に作成される秘密鍵をダウンロードしておく。
4. conohaのゲストOSにログインする。gitリポジトリにログインするユーザの`~/.ssh/authorized_keys`を vi などで開き、先ほどconohaで作成したパブリックキーの長い文字列を転記する。
5. conohaのゲストOSの設定ファイルを管理者権限（sudoなど）で編集する。
  これでルートログインとパスワード入力を不可にする。

    ```conf:/etc/ssh/sshd_config
    #PermitRootLogim yes
    PermitRootLogim no
    #PasswordAuthentication yes
    PasswordAuthentication no
    ```
6. sshd を再起動し、設定を有効にする。

    ```
    sudo systemctl restart sshd
    ```
7. conohaからダウンロードした秘密鍵ファイルを、Windowsの`C:\Users\<ユーザ名>\.ssh\id_rsa`ファイルとして名前変更し、移動する。

以上の手続きでGit Bashから`git clone`できました。
PuTTYのplink.exeを使おうとしてハマった…

