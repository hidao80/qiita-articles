---
title: ローカルで開発中のページをhttpsでスマホからも見る
tags: Laragon HTTPS ローカル開発環境 スマホ PHP
author: hidao
slide: false
---
やり方が分かったので、とりあえず忘れないうちにメモ。

## サーバの設定

laragon をインストール。下記は chocolatey を使った場合。

```ps:chocolatey
cinst -y laragon
```

Laraon を起動し、Preferences の General より`Document Root`を設定し、`Auto Virtual hosts`のチェックをオンにする。
<img width="935" alt="image.png (110.5 kB)" src="https://img.esa.io/uploads/production/attachments/16142/2021/06/30/62386/3c9e4614-664a-497c-b51e-5ec67b0ac26d.png">
Servises & Ports の https にチェックを入れる。SSL 証明書発行のために管理者権限を求めてくるので（Windows10 の場合）、随時昇格ダイアログの OK ボタンを押す。

### プロキシサーバの設定
ローカル開発環境の`hosts`に書かれた内容をスマホやタブレットからも参照するため、laragon の中にプロキシサーバも立てる。

laragon の httpd.conf を開く。
<img width="735" alt="image.png (94.8 kB)" src="https://img.esa.io/uploads/production/attachments/16142/2021/06/30/62386/42aab0e3-0732-458c-804f-e830f46ae818.png">

次の3つのモジュールのコメントアウト（先頭の`#`記号）を外す。

```conf:httpd.conf
LoadModule proxy_module modules/mod_proxy.so
LoadModule proxy_http_module modules/mod_proxy_http.so
LoadModule rewrite_module modules/mod_rewrite.so
```

`httpd.conf`ファイルの最後に以下の内容を追加する。

```conf:httpd.conf
<IfModule mod_proxy.c>
  ProxyRequests On
  ProxyVia On
  Listen 8080
  <Proxy *>
    Order deny,allow
    Deny from all
    Allow from all
  </Proxy>
</IfModule>
```

httpd.conf ファイルの編集を保存し、laragon の apache を再起動する。Reload ではなく、一度停止（Stop）から起動させた方ががよさそう。

## スマホ・タブレット側のプロキシ設定
スマホ・タブレット側にプロキシ設定を行うことで、テスト用 PC の`hosts`に書かれているドメイン名で開発中ページを表示することができるようになる。

スマホやタブレットのプロキシ設定を行う。以下は iPhone の例。
サーバ欄にはプロキシサーバにしている PC の IP アドレスを入力し、ポート欄には上記設定で`Listen`にある通り`8080`と入力する。

<img width="375" alt="IMG_0043.PNG (72.5 kB)" src="https://img.esa.io/uploads/production/attachments/16142/2021/06/30/62386/6c1e7662-c27a-4691-974c-c2e7d05635f8.PNG">

以上でプロキシ設定を行ったスマホやタブレットや別 PC などからも`https://プロジェクトディレクトリ名.test/`でアクセスできるようになる。

## 注意
プロキシ設定中はインターネットやほかの PC へアクセスできなくなるかもしれない。（未検証）
また、プロキシサーバ設定はセキュリティを何も考えていないので、プロキシサーバを立てたまま公共的なネットワークに接続しないようにすること。

iPhone や iPad でアクセスするとオレオレ証明書に反応して警告画面が表示されるが、何度か「表示」や「進む」などのリンクやボタンを押せば開発中ページが表示されるようになる。

