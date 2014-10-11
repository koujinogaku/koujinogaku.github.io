---
layout: document
title: オリジナルOS作り(自作OS)
next_section: dev-tools
---
## もくじ
- [このぺーじ](index.html)
- [開発環境](dev-tools.html)
- [現在までの開発状況](history.html)
- [メモリーマップ](memory-map.html)
- [ダウンロード][disk-image]


## オリジナルのOSをカーネルから作っています。
簡単に言えば、自作OSを作成中です。

開発の進行状況は[こちら](history.html)からご覧ください。

どちらかといえば、OSを使わず直接PCのハードウェアをいじってみたかったというのが近いです。

一部の人たちのあいだでは、自作OSが流行っているようです。自作OSの書籍が出版され、皆さんそれを元にしてOS作りを楽しまれています。僕もその一人ですが、プログラムの中身は、書籍のプログラムの派生ではなく、CORON-OSというOSに影響されて作り始めました。ですからだいぶ傾向が違います。プログラミングで最初に作るHello Worldの表示プログラムを作っているつもりで楽しんでおります。 新しいソースコードが読みたいという方には、楽しめると思います。 

[![Screenshot](img/thumb/helloscreenshot20120823.png)](img/helloscreenshot20120823.png)

スペックはこんな感じ。 

- プリエンティブ マルチタスク オペレーティングシステムです。
- Intel i386 プロテクトモードで動作します。 
- Intel i386 の機能でメモリーのページ制御をしています。セグメントは使いません。 
- タスク管理もi386にお任せです。自分でやってもいいのですが別のCPUに移植するつもりもないので。 
- 一応、マイクロカーネル風で、ほとんどのドライバーはユーザーモードで動作します。 
- 割り込みはすべてメッセージに変換されてドライバーへ到達します。 
- キャラクタベースのコマンドラインインターフェースが基本動作です。 
- ウィンドウマネージャーは開発途上で、コマンドラインから起動予定です。 

## 最新バージョンのダウンロード
取り合えず、ここまでできています。 

- [最新フロッピデスクイメージ][disk-image]
- [最新ソースコード][source-code]

## 実行環境
フロッピディスクイメージはエミュレーターを使って起動してください。VWMare Playerで動作します。VirtualBoxもしくはVirtual PCでも一応動いています。現在QEMUでの動作確認は中断しています。

- [VWMare Player](http://www.vmware.com/jp/products/player/)
- [VirtualBox Player](https://www.virtualbox.org/) ※2012-07-11より
- [Microsoft Virtual PC 2007 SP1](http://www.microsoft.com/downloads/details.aspx?displaylang=ja&FamilyID=28c97d22-6eb8-4a09-a7f7-f6c7a1f000b5)
- [QEMU on Windows](http://www.h7.dion.ne.jp/~qemu-win/index-ja.html)のサポートは現在していません。

## 仮想環境に必要な資源

メモリー

- Nano-Xを使用する場合は16MB以上必要です。
- CUIのみの使用であれば4MBあれば十分です。

ディスク

- 1.44MBのフロッピーディスクドライブ(仮想で可)

ユーザインターフェース

- PS2キーボード
- Nano-Xを使う場合はPS2マウス

## 使い方
ダウンロードしたフロッピーディスクイメージのzipを解凍し、出てきたhello.imgをフロッピーディスクイメージに指定して起動します。

Virtual PCやVMWare、VirtualBoxのフロッピーディスクイメージに指定して起動してください。

起動してコマンドラインが表示されたら、
「DIR」と入力するとファイル一覧が表示され
「TYPE README.TXT」と入力するとREADME.TXTファイルが表示されます。

「win」
　と入力するとNano-Xというウィンドウシステムが起動します。

※自作の旧ウィンドウシステムは開発中断してます。

## コマンド一覧
現状のOSコマンドです。

- WIN: Nano-X(MicroWindows)を起動します。
- DIR : ファイル一覧を表示します。
- TYPE ファイル名 : テキストファイルを表示します。
- FREE : メモリー状況を表示します。
- PS : 実行中のプロセスを表示します。
- QS : 作成されたQueueを表示します。
- DATE : 現在日時を表示します。
- CLS : 画面をクリアします(暫定版)

## 開発の進捗状況
現在の開発状況など詳しいことはこちらをご覧ください。

- [開発の履歴](history.html)


[disk-image]:   /files/helloimg20130129.zip
[source-code]:  https://github.com/koujinogaku/helloos
