---
layout: document
title: 開発環境(自作OS)
upper_section: index
previous_section: index
next_section: history
---
基本の開発環境はMinGW/msysです。開発環境の作成手順をまとめます。

## MinGW/msysのインストール
基本の環境はMinGW/msysです。

以下のサイトからダウンロードできます。

> [Minimalist GNU for Windows](http://www.mingw.org/)

MinGWとmsysでコンパイルできるオプションでインストールしてください。
これを書いている時点のオプションでは以下を入れました。

- C Compiler
- MSYS Basic System
- MinGW Developer ToolKit

インストールされた主なソフトウェアのバージョンは以下のとおりでした。

- gcc version 4.7.0 (GCC)
- GNU assembler (GNU Binutils) 2.22
- GNU ld(GNU Binutils) 2.22
- GNU Make 3.81
- GNU bash 3.1.17(1)

## GCCののBuild
HelloOSのコンパイルには別途コンパイラーを用意します。

MinGWではWindows上で動作する実行形式が出来上がるため、Windowsがない環境では動作しません。

HelloOSをコンパイルするにはクロスコンパイラーが必要になるためです。

コンパイラーにはgcc 4.7.1を使います。

gccのソースをダウンロードし、MinGWを使ってターゲットi386-elfでgccをコンパイルします。

これまでgcc 4.2.2という古いバージョンを使っていたのですが、
一気にバージョンを上げました。

HelloOSの新しいソースコードでは最適化のpragma指定を出来るバージョンのgcc(ver 4.4以上)が必要です。

クロスコンパイル環境構築のために今回ダウンロードしたのは以下のとおりです。

- binutils-2.22 [GNU Binutils](http://www.gnu.org/software/binutils/)
- gmp-5.0.5 (GCCに必要) [The GNU Multiple Precision Arithmetic Library](http://gmplib.org/)
- mpfr-3.1.1 (GCCに必要) [The GNU MPFR Library](http://www.mpfr.org/)
- mpc-1.0.1 (GCCに必要) [Gnu Mpc](http://www.multiprecision.org/)
- gcc-4.7.1 [GCC, the GNU Compiler Collection](http://gcc.gnu.org/)


ターゲットはi386-elfと指定します。

486以上の勉強をしていないので386で。また、elfは使っていませんがaout形式で作れるのでelfを指定しています。ホストタイプはあるわけではないのでunknownとでもすればいいのかも知れないのですがなくてもbuild出来ました。

実際の操作は次のようになりました。

## Binutils
ターゲットを指定し、またパス設定が面倒にならないように/usr/localを指定しています。

    cd ~
    tar zxf binutils-2.22.tar.gz
    cd binutils-2.22
    mkdir cross
    cd cross
    ../configure --target=i386-elf --prefix=/usr/local
    make all
    make install

## GMP
最近のGCCではこれらのライブラリが必要になります。

ターゲットを指定していないのは、このライブラリが動作するのはmsys環境だからです。

コンパイルできたら、必ずmake checkをしてください。

    cd ~
    tar jxf gmp-5.0.5.tar.bz2
    cd gmp-5.0.5
    ./configure
    make all
    make check
    make install

## MPFR
ビルドしたGMPを指定してビルドします。

    cd ~
    tar jxf mpfr-3.1.1.tar.bz2
    cd mpfr-3.1.1
    ./configure --with-gmp=/usr/local
    make all
    make check
    make install

## MPC
同様にビルドしたGMPとMPFRを指定してビルドします。

    cd ~
    tar xf mpc-1.0.1.tar.gz
    cd mpc-1.0.1
    ./configure --disable-shared --with-gmp=/usr/local --with-mpfr=/usr/local
    make all
    make check
    make install

## GCC
再びターゲットをクロスコンパイル先のi386-elfに指定しています。

C言語のみと指定しないと他の言語もすべて作ろうとしてしまい大変時間がかかります。--enable-languages=cを指定してください。

Newlibは使いませんが、GCC内で用意されたライブラリを作り始めないように --with-newlib指定します。

完成したクロスコンパイラーがクロスコンパイルするときにgccの標準ヘッダーを参照しないように --without-headersを指定します。

MinGWなので--disable-sharedにします。

クロスコンパイラーをビルドする場合は --disable-libssp --disable-libquadmathをつけないとコンパイルエラーになるようです。

GMP/MPFR/MPCを指定します。

クロスコンパイラーが日本語表示するとmsys環境では見にくいので --disable-nlsを指定します。

    cd ~/gcc-4.7.1
    mkdir cross
    cd cross
    ../configure --target=i386-elf --enable-languages=c --with-newlib --without-headers --disable-shared --disable-libssp --disable-libquadmath --with-gmp=/usr/local --with-mpfr=/usr/local --with-mpc=/usr/local --prefix=/usr/local --disable-nls
    make all
    make install
