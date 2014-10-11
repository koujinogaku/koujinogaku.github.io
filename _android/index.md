---
layout: document
title: Android SDK
---
## もくじ

- [このぺーじ](index.html)

-----------------------------

ちょっと触ってみたかっただけです。

## 開発環境

### Android SDKダウンロード
Android Developersのホームページからダウンロードします。

> [android Developers](http://developer.android.com/sdk/index.html)

### SDKインストール
ダウンロードしたものをインストールするとまずエラーがでます。

    Failed to fetch URL https://dl-ssl.google.com/android/repository/repository.xml, reason: HTTPS SSL error. You might want to force download through HTTP in the settings.

エラーを回避するためには、ホームディレクトリに設定ファイルを作成します。
以下のファイルを作成します。

    C:\Documents and Settings\ユーザー名\.android\androidtool.cfg

ファイルの中に一行以下のように書きます。

    sdkman.force.http=true

これでもう一度セットアップを実行するとインストールが始まります。
