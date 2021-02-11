---
title: "【環境構築】anyenv + nodenv をインストールしてみた"
emoji: "🕌"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["anyenv", "nodenv"]
published: true
---

# Homebrewが入ってるか確認
```shell-session
$ brew -v
Homebrew 2.7.3
Homebrew/homebrew-core (git revision 438d0; last commit 2021-01-12)
Homebrew/homebrew-cask (git revision 0f2b1; last commit 2021-01-12)
```

# nodebrew用に環境変数を設定していないか確認
[とその前にnodebrew用に設定していた環境変数をコメントアウトします。](https://qiita.com/kyosuke5_20/items/eece817eb283fc9d214f#%E3%81%A8%E3%81%9D%E3%81%AE%E5%89%8D%E3%81%ABnodebrew%E7%94%A8%E3%81%AB%E8%A8%AD%E5%AE%9A%E3%81%97%E3%81%A6%E3%81%84%E3%81%9F%E7%92%B0%E5%A2%83%E5%A4%89%E6%95%B0%E3%82%92%E3%82%B3%E3%83%A1%E3%83%B3%E3%83%88%E3%82%A2%E3%82%A6%E3%83%88%E3%81%97%E3%81%BE%E3%81%99)
```shell-session
$ find ~/.zprofile
find: /Users/xxxx/.zprofile: No such file or directory
```
そもそもnodebrewを使ったことがなかったので.zprofileが存在しなかった。
.zprofileが存在する場合は以下をコメントアウトする。
```
export PATH=$HOME/.nodebrew/current/bin:$PATH
```

# anyenvをインストールし初期化する
```shell-session
$ brew install anyenv
```
```shell-session
$ anyenv init
# Load anyenv automatically by adding
# the following to ~/.zshrc:

eval "$(anyenv init -)"
```
~/.zshrcにコマンドを追記すれば自動で読み込まれる、と言っているのでやってみる。^[$ echo 追記する文字列 >> ファイル名]^[$ eval 'hoge hoge' 指定した文字列を連結してシェルに実行させる]
```shell-session
$ echo 'eval "$(anyenv init -)"' >> ~/.zshrc
```
もう一度 $ anyenv init してみるも出力は変わらない。
ターミナルを終了。

# ターミナルを再起動
なんか怒られている。
```shell-session
ANYENV_DEFINITION_ROOT(/Users/xxxx/.config/anyenv/anyenv-install) doesn't exist. You can initialize it by:
> anyenv install --init
```
上記に従ってコマンドを入力する。
[anyenvのGitHub](https://github.com/anyenv/anyenv)には
> You'll see a warning if you don't have manifest directory.

とあるため、途中の質問にはyesで回答。
```shell-session
$ anyenv install --init
Manifest directory doesn't exist: /Users/xxxx/.config/anyenv/anyenv-install
Do you want to checkout ? [y/N]: y
# 中略
Completed!
```
やったぜ。

# anyenvでインストールできる**env系のリストを表示
```shell-session
$ anyenv install -l
```
nodenvやrbenvなどズラッと表示された。

# nodenvをインストール
```shell-session
$ anyenv install nodenv
# 中略
Install nodenv succeeded!
Please reload your profile (exec $SHELL -l) or open a new session.
```
上記に従ってシェルを再起動する。
```shell-session
$ exec $SHELL -l
```

# nodenvでインストールできるバージョンの確認
```shell-session
$ nodenv install -l
```
ズラズラズラ〜っとたくさん表示された。
目当てのバージョンが存在することを確認。

# バージョンを指定してインストール
```shell-session
$ nodenv install 14.15.4
```
[新しい**env系をインストールをした後はbash（shell）の再起動を忘れずに行っておきましょう](https://qiita.com/rinpa/items/81766cd6a7b23dea9f3c#env%E7%B3%BB%E3%81%AB%E7%95%B0%E3%81%AA%E3%82%8B%E3%83%90%E3%83%BC%E3%82%B8%E3%83%A7%E3%83%B3%E3%81%AE%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB)
```shell-session
$ exec $SHELL -l
```

# インストールされているバージョンを確認
```shell-session
$ anyenv versions
nodenv:
* system (set by /Users/xxxx/.anyenv/envs/nodenv/version)
  14.15.4
```

# バージョン指定(グローバル)
:::message
バージョン名が ~/.nodenv/version ファイルに書き込まれる。
:::
```shell-session
$ nodenv global 14.15.4
$ nodenv global
14.15.4
$ node -v
v14.15.4
```

# バージョン指定(ローカル)
:::message
カレントディレクトリ内の .node-version ファイルにバージョン名が書き込まれる。
ローカルバージョンはグローバルバージョンを上書きする。
環境変数 NODENV_VERSION を設定するか、 nodenv コマンドを使用して上書きできる。
:::
```shell-session
$ cd test-directory # プロジェクトのディレクトリに移動
$ nodenv install 12.20.1
$ nodenv local 12.20.1
$ nodenv local
12.20.1
$ find .node-version # .node-versionが作成されている
```

```shell-session
$ anyenv versions
nodenv:
  system
* 12.20.1 (set by /Users/xxxx/workspace/test-directory/.node-version)
  14.15.4
```

# まとめ
備忘録的にインストール方法を書き残しました。
これで開発プロジェクトごとに、バージョンを気軽に変更できるようになりそうです。

# 参考文献
[GitHub - anyenv/anyenv](https://github.com/anyenv/anyenv)
[GitHub - nodenv/nodenv](https://github.com/nodenv/nodenv)
[anyenv + macOS環境構築 - Qiita](https://qiita.com/rinpa/items/81766cd6a7b23dea9f3c)
[MacにNode.jsをインストール（anyenv + nodenv編） - Qiita](https://qiita.com/kyosuke5_20/items/eece817eb283fc9d214f)