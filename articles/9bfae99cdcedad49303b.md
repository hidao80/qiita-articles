---
title: windows で npm の shell 設定を変更したら別窓に開くようなってしまったのを直す
tags: Windows Node.js npm shell トラブルシューティング
author: hidao
slide: false
---
たちまちやり方がわからなかったので`npm`の設定ファイルのリセットで直しました。
`npm`の設定ファイルを修正すればリセットする必要はないと思われますが、方法がわかりません。

ユーザーのデフォルトをリセットするには
これをコマンドライン（またはWindowsではgit bash）で実行します。

```sh
echo "" > $(npm config get userconfig)
npm config edit
```

グローバルデフォルトをリセットするには
```sh
echo "" > $(npm config get globalconfig)
npm config --global edit
```

```sh
sudoが必要な場合は、代わりにこれを実行してください。
sudo sh -c 'echo "" > $(npm config get globalconfig)'
```

## 引用元
node.js - How to restore/reset npm configuration to default values? - Stack Overflow
https://stackoverflow.com/questions/20934343/how-to-restore-reset-npm-configuration-to-default-values

