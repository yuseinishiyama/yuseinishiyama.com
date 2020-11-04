---
title: "HugoでGoogle Analyticsを開発中に無効にする正しい方法"
date: 2020-11-04T01:17:30+01:00
---

まず、Google Analyticsの埋め込みは[ビルトインのテンプレート](https://gohugo.io/templates/internal/#google-analytics)を使う。`<head>`のどこかに次のコードを追加する。

```go
{{ template "_internal/google_analytics.html" . }}
```

この前後でローカルで実行されているかのチェックを自前でしてしまいそうになるが、このテンプレートのコードを良く読むと[先頭](https://github.com/gohugoio/hugo/blob/cf6131dc18e5e833b021724a0d9bcdaa6827b8dd/tpl/tplimpl/embedded/templates/google_analytics.html#L1-L2)に次のようにある。

```go
{{- $pc := .Site.Config.Privacy.GoogleAnalytics -}}
{{- if not $pc.Disable -}}
```

どうやら設定ファイルで`privacy.googleAnalytics.disable = true`とすればスクリプトを無効にできそうだ。後はlocalhostで実行時だけ別の設定を持たせる方法が分かればよい。[ドキュメント](https://gohugo.io/getting-started/configuration/#configuration-directory)によれば、そもそもサーバーの実行時には自動的に環境が`development`になるらしい。

> Default environments are development with hugo server and production with hugo.

設定ファイルの配置を以下のようにしてやれば、環境によらず`_default`以下の設定が読み込まれ、`development`のときだけ同名のディレクトリ以下の設定も追加で読み込まれる。

```
config
├── _default
│   └── config.toml
└── development
    └── privacy.toml
```

最後に`privacy.toml`の中身に以下の行を追加する。

```toml
[googleAnalytics]
disable = true
```

これで、通常通り`hugo server`で起動するとGAが無効になる。コードや実行コマンドの変更一切なしで切り替えることができるので、一番美しい方法だと思っている。