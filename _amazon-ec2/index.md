---
layout: document
title: Amazon EC2/S3の使い方(EC2)
next_section: administrator-console
---
Amazon EC2を使ってWebサーバを公開しちゃいました。
これまで、まったく知らなかった「クラウドコンピューティング」の初心者が、試行錯誤をしながら、何とか動くまでに行なったさまざまな作業と、気づいた点をまとめていきます。

![screenshot]({{ site.images_path }}amazon-ec2.gif)

ちょっとづつ書きますので、当分は書きかけ状態ですのでご了承ください。

Amazon EC2によるWebサーバ公開までの大まか流れは以下のような感じです。

- 初めての起動
  - [管理者PC環境の準備](administrator-console.html)
  - [料金とサイトの確認](pricing.html)
  - [アカウント登録](registration.html)
  - [セキュリティ証明書の理解](security-credentials.html)
  - [OSイメージ選び](choose-os-image.html)
  - [Key pair生成](generate-key-pair.html)
  - [インスタンス作成](create-instance.html)
- OSイメージのバックアップ
  - [Amazon S3とは](whats-amazon-s3.html)
  - [AWSアクセス証明書の取得](get-access-keys.html)
  - [管理クライアントの用意](management-client.html)
  - [バックアップ場所の登録](backup-destination.html)
  - [OSイメージバックアップ](backup-os-image.html)
  - [バックアップイメージからの起動](launch-os-image.html)
- データ領域のバックアップ
  - S3コピーツール
  - EBSの場合
