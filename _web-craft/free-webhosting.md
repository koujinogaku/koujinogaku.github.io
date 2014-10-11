---
layout: document
title: 無料サーバー
upper_section: index
previous_section: plain-css
next_section: wiki
---
作ったホームページを公開するのに、やっぱり無料がいいですよね。僕が使った気軽に使える無料レンタルサーバーをまとめました。

## チェックポイント
作ったホームページで何をするか？によって必要な機能が違ってきます。
CGIやPHPなどのプログラムを使えるか否かで大きく分かれますが、マッシュアップという技術を使おうとすると重要なのがHTTPの送信ができるかどうかです。
またメール送信が出来るか否かも、ホームページで提供するサービスの内容に影響を与えます。

英語に抵抗がない人は、海外サーバーもあります。米国では無料なのに機能豊富なサーバーを借りることが出来ます。

## 無料レンタルサーバー(または無料アカウント)比較

| サイト名 | 容量 | 広告| 商用  |
|--------|-----|-----|------|
|Github Pages|?|無|オープンソース限定|
|FC2WEB|1GB|無|商用可|
|Google App Engine|?|無|商用可|
|xrea.com|50MB|有|商用可|
|Infoseek isweb(終了)|50MB|有|楽天アフィリエイト|
|@PAGES|1GB|有|アフィリエイト|
|land.to|100MB|有|商用可|
|000webhost.com|1.5GB|初回アクセス|商用可|
|zymic.com *注|5GB|無|不可|

| サイト名 | CGI | PHP | DB | HTTP送信 | メール送信 | バッチjob |
|--------|-----|-----|----|---------|----------|---------|
|Github Pages|x|x|x|-|-|-|
|FC2WEB|x|x|x|-|-|-|
|Google App Engine|Java,Python,Go|プレビュー版|Google Cloud Datastore|?|可|有|
|xrea.com|Perl,Ruby,Python,PHP|有|MySQL,PostgreSQL|?|可|x|
|Infoseek isweb(終了)|Perl/SSI|x|x|x|x|x|
|@PAGES|Perl,Ruby,Python,C,C++|有|MySQL,SQLite|可|x|x|
|land.to|Perl,Ruby,Python,PHP,C,C++|有|MySQL,PostgreSQL|可|x|x|
|000webhost.com|x|有|MySQL|可|可|有|
|zymic.com *注|x|有|MySQL|x|x|x|

注)zymic.comという所は、レンタルするドメイン名で調べると日本語のサイトのお隣さんが好みの合いそうに無い厳つい方たちだったのでサイトを立てる直前で作るのを諦めました。
