---
layout: document
title: 開発環境(自作OS)
upper_section: index
previous_section: history
---
### メモリーマップ
<pre>
FFFF:FFFF ----------------------
           割り込みテーブル(未使用)
FFFF:FFF0 ----------------------
           PCI I/O(未使用)
$$$$:$$$$ ---------------------- 
          Virtual Address:
              VESA MODE VRAM
D000:0000-------D000:0000-----
          Virtual Address:
              User Argument 128B
CFFF:FF80 -------CFFF:FF80-----
          Virtual Address:
              User Stack   32KB
CE00:0000 -------A000:1000-----
          Virtual Address:
              User Heap 736MB
A000:0000 -------A000:0000-----
           Virtual Address:
             User Code & Data
                     512MB
8000:0000  -------8000:0000-----
            Virtual Address:
             Shared Memory 2GB
0080:0000 -------0080:0000-----
            8MBまで物理メモリに
            マッピングして
           Kernel Heap領域で
            使用。
           ページディレクトリ
            もここから確保
              memory alloc
0020:0000 -------0020:0000-----
                  kernel   1MB
0010:0000 =====================
           BIOS            64KB(未使用)
000f:0000 ----------------------
           拡張BIOS        64KB(未使用)
000e:0000 ----------------------
           各種カード      96KB(未使用)
000C:8000 ----------------------
           Video BIOS      16KB(未使用)
000C:0000 ----------------------
           Video Reserved  64KB(未使用)
000B:0000 ----------------------
           VGA             64KB 
000A:0000 =======640KB境界======
           Kernel Heap2  512KB
          ------0002:0000------
                   GDT    64KB
0001:0000 ------0001:0000------
                   IDT     2KB
          ------0000:F800------
          ------0000:F000------
           stack for      60KB
               setup or kernel
             boot以後下まで
0000:8000    つぶして使う
0000:7E00 ======================
             iplinfo      32B
               ipl        480B
0000:7C00 ------0000:7C00------
            stack for ipl 30KB

          ------0000:4020------
              tmp GDT   32B   
          ------0000:4000------

            Setup終了後
            kernelでは
            Page Managerの
            Page Windowとして
            使用
                   setup    4KB
          ------0000:1000------
               bios info    64B
0000:0500 ------0000:0500------
           BIOS用ワーク    256B(未使用)
0000:0400 ----------------------
           BIOS用スタック  256B(未使用)
0000:0300 ----------------------
           Real用INTベクタ 768B(未使用)
0000:0000 ----------------------
</pre>