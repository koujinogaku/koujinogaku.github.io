---
layout: document
title: Amazon S3とは(EC2)
upper_section: index
previous_section: create-instance
next_section: get-access-keys
---
Amazonの提供するクラウドサービスで欠かせない物の一つにAmazon S3というのがあります。
一言で表せばクラウドストレージで、良くある解説の例ではPC上のファイルの保存先として書かれています。
確かに、インターネット上にあるファイルサーバー的な使い方もできるのですが、そんな事の為に使うのであれば、他にもっと使いやすいサービスがたくさんあります。
Amazonのクラウドサービスならではの使い方に意味があるわけです。

その使い方の中の一つがAmazon EC2のバックアップサーバーとして使い道です。
最初のほうに書いたようにAmazon EC2は（EBSから起動するのでなければ）RAM DISKのようにせっかく起動後に設定したりインストールしたソフトウェアなどは、シャットダウンとともに消えてしまいます。
これはOSの起動イメージをロードしてインスタンス（仮想マシンの実態）を作るのですが、このときインスタンスに割り当てられているディスクはInstance-Storeという場所に起動イメージをコピーして作られます。
起動後にDISKに書き込みをしても、元のOSの起動イメージに書き込まれるわけではないのです。
シャットダウンするとInstance-Storeは他のインスタンスの為に開放されてなくなります。

ということは、起動後にセットアップした内容をバックアップする手段がなければ、ほぼ運用は無理って言うことです。

この時のバックアップ先として使えるのがAmazon S3であり、実はOSの起動イメージであるAMIもこのAmazon S3に保存されているのです。
そしてさらに具体的に表現すると、起動後にセットアップしたOS環境のシステムバックアップするという作業は、新しいOS起動イメージであるAMIをAmazon S3に作ることを意味しています。

もちろん、起動ディスクイメージだけでなくユーザデータのディスクをバックアップすることもできます。
通常のインスタンスのディスク構成は起動パーティションと、データパーティションに分かれていますので、このデータパーティションのほうをAmazon S3にバックアップするということです。
ユーザーデータですからパーティション丸ごとというより、個別のファイルやディレクトリごとにバックアップしたいと思いますので、ここではディレクトリをバックアップする方法を解説していきます。

Amazon S3を使うにあたって、注意というか意識しておくべきことがあります。

1. EC2のインスタンス内部から利用するためには、アクセスキー取得が必要
2. Bucketという入れ物に、全世界で一意な名前を割り振らなければならない
3. データバックアップのツールは自分で用意する

## 認証用のアクセスキー
概念的なことは「セキュリティ証明書の理解」のところで書きましたが、EC2立ち上げまでに使ったアカウント類とは別に、またまた認証用のアクセスキーが必要になります。
面倒ですが仕方ありません。
詳しい手順は後述します。

## Bucketについて
BucketとはS3の中での自分用の入れ物です。

S3でとても意外だったのはBucketの名前が他人とかぶらない様にしなければならないという事で、名前空間の管理が甘いよなと思ってしまいました。
全世界であなただけの名前をいくつも作らなければならないので、最初によーく考えておいてください。
OS起動のイメージは基本的に、１つのイメージが一つのBucketに対応します。

S3の物理的な保存場所はこれまで良く出てきたリージョンごとに設けられています。
インスタンスの場所に、何かと便利でお得なアメリカ東海岸を使うとするのであれば、Bucketも同じくアメリカ東海岸に作ってください。日本大好き！な方はインスタンスもBucketも東京に作るのがいいと思います。
だれがどのリージョンになんと言う名前でBucketを作ったかはAmazon S3の中で一元管理されていて、Bucket名を指定しただけで、APIが物理的な場所へアクセスしてくれます。
アクセス権などもBucket単位で設定されていて、デフォルトでは作成者のみがアクセスできます。

## ツールの用意
S3の利用はもともの何もツールがなくAPIだけが用意されていたのですが、徐々にAmazon標準の画面が出来上がってきており、Bucketの管理はできるようになったのですが、
これを書いている時点でもユーザデータのバックアップについては配慮が無く、ツールは自前で探してくる必要があります。
後の章ではそれも含めて紹介します。

ちなみに、当初は無かったAmazon標準のS3画面に相当する、FireFoxのAmazon S3管理プラグインを紹介するはずでしたがやめました。
「管理者PC環境の準備」の章と矛盾するかもしれませんがご了承を。

## EBSがあるじゃないか
・・・・・と、ここまで読んで、知っている人であれば「それは嘘ですよ。だってシャットダウンしてもインスタンス消えないし、また起動すれば前の状態が残っているよ！実際にそうやって使っているもの」という方がいらっしゃると思います。

まさにそのとおり。私がこの情報の最初の章を書き始める前と比べるとAmazonのクラウドサービスはずいぶん進化しています。なんとEBSからインスタンスが起動できるようになってしまったんですね。
EBSは保存してあるそのままをディスクとしてマウントできる仕組みなので、Instance-Storeにロードする必要がないんです。
ですから、ここから先に書くことは、まったく意味がないことかもしれません。
EBSを使いたくない、ケチケチ大好きな方々がお楽しみください。

ちなみに私のシステムでは今でも主力マシンではEBSを使っていません。だって維持費がEC2のインスタンス以外に必要になり、Rootパーティションの為だけにEBSを使うのもなんだかもったいない気がします。
さらにデータディスクについてもバッチジョブが24時間フル稼働していてデータベースの大きさもアクセス量もかなり多いので、EBSだとなにかと課金がかさみ維持費かかりすぎなんです。
EC2のインスタンスタイプがSmallですからデータエリアとして100GBのデーターが軽々入る容量が最初からついてくるのを使わない手はないですよ。
自分でバックアップをきちんと出来るのであれば、EBSを使わないという選択肢もあるということを少しだけ考慮すべきと思います。
