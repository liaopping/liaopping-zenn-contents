---
title: "【Flutter】Android Studioのdevice selectionでiosシミュレーターが選択できない問題"
emoji: "💭"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["Flutter", "AndroidStudio"]
published: true
---

# 経緯
[【初心者向け超入門】MacでFlutterの環境構築（iOS/Androidアプリ開発,プログラミング）](https://youtu.be/kpvVENfDCRc)
上記動画に従って環境構築を行っていたところ以下の箇所(iPhone 12 Pro ~)が"<no devices>"となり、
Flutter Device Selectionでiosシミュレーターを選択できない問題が発生。
Androidデバイスは選択できるのに。。。
![](https://storage.googleapis.com/zenn-user-upload/kj7unu41jwsj9efl2lchaghkopxw)
![](https://storage.googleapis.com/zenn-user-upload/m5hclg53xihw74kjb362jfhh50qd)

# 原因を探る
```
$ flutter doctor
```
```
✗ Flutter plugin not installed; this adds Flutter specific functionality.
✗ Dart plugin not installed; this adds Dart specific functionality.
```
Preferencesからプラグインを入れているはずなのにインストールされてないことになってる。。。

# 解決法
```
$ ln -s ~/Library/Application\ Support/Google/AndroidStudio4.1/plugins ~/Library/Application\ Support/AndroidStudio4.1
```
[Flutter plugin not installed error;. When running flutter doctor](https://stackoverflow.com/questions/51860845/flutter-plugin-not-installed-error-when-running-flutter-doctor)
上記記事に記載のコマンドを実行し解決。
[【 ln 】コマンド――ファイルのハードリンクとシンボリックリンクを作る](https://www.atmarkit.co.jp/ait/articles/1605/30/news022.html)
lnコマンドでシンボリックリンクとやらを作成することによって解決したみたい。
パスを通すみたいな話か。

-----
##### 編集後記
Stack Overflowはすごい。