## これは何  
mariones スーパーマリオ専用エミュレータのCソースコード一式  
Vivado HLS 2015.1で合成するなら修正したmariones_C_source_for_VivadoHLS
を使うことをお勧めします.
なお, 対応カートリッジはスーパーマリオブラザーズのみ。。  

Developed by Hiroki Nakahara
 31, July, 2015

## ライセンス  
~~このコード一式はみきゃんウェアです。ゆるキャラグランプリ２０１５で
みきゃんに投票して頂ければ自由に配布・改変・公開してかまいません。~~
MITライセンス

## 内容物  
makefile        … メイクファイル. プロファイル解析も行います。  
main.c          … テストベンチを含んだメイン関数。  
mariones_top.c  … NESのトップ関数。これ以下をVivado HLSで高位合成にかける。  
mbc.c           … メモリ部分をエミュレーション. 別配布のpartition_ROM.exe で分割した  
　　　　　　　      カートリッジイメージをここにコピペしてください。  
ppu.c           … PPU(描画ユニット)エミュレーション部  
registers.c     … レジスタエミュレーション部  
reset_nes.c     … NESのソフトウェアリセットを行う。起動時に呼び出し必須。  
RICHO2A03.c     … NESのCPUをエミュレーションする部分  
mariones.h      … ヘッダファイル。CPUの命令セットマクロとレジスタ・メモリの宣言  
partition_ROM.c … カートリッジイメージを分割するツール。Vivado HLSでは  
　　　　　　　　　　読み込まないように！！  
log.txt         … 試しにスーパーマリオを数十フレーム動作したときのCPUのログ出力。  
　　　　　　　　　  デバッグに使ってください。  
mov_mario.txt   … 別のエミュレータを使って記録したマリオのジョイパッド入力データ。  
　　　　　　　　　　Vivado HLSでCシミュレーションに使用。とりあえず、スタートを押すので  
　　　　　　　　　　別のカートリッジのデバッグにも使えるかもしれない。。  
screen_???.bmp  … 試しにスーパーマリオを数十フレーム動作したときのBMP画像の出力。  
　　　　　　　　　　Vivado HLSでCシミュレーションした結果と突き合わせるのに使う。  

## 環境  
Ubuntsu 14.04 LTS, Cygwin 64bit版を想定しています。
gcc を入れてください。

まず、NESのカートリッジイメージを用意します。私はArduinoを使って吸い出しました。

ネットからダウンロードしたら違法だよ！カードリッジ買おうね！！  
ネットからダウンロードしたら違法だよ！カードリッジ買おうね！！（大事なことなので２回言った）  

現時点ではマッパー０、ミラーリング１のみ対応しています。つまり、スーパーマリオブラザーズ専用。  
他のカートリッジはまだ試していません。誰か人柱プリーズ。

## 実行方法  
別配布の partition_ROM.c をコンパイルしてバイナリを作ってください。

$ gcc -O3 -o partition_ROM partition_ROM.c  

で、カートリッジイメージ（ここではsuper_mario.nes）を分割して埋め込み用のテンプレを生成します。  

$ ./partition_ROM supar_mario.nes  

実行すると  

[ROM INFO]  
ROM_SIZE: 32 KB, VROM_SIZE: 8 KB  
PROGRAM #PAGE: 2, CHARACTER #PAGE: 1  
MAPPER #0  
MIRRORING: 1  
IS_SRAM: 0  
IS_TRAINER: 0  
IS_FOUR_SCREEN: 0  

別のカートリッジでも同じ情報が出ると、そのまま使えるかも。  
同時に、ファイル(super_mario.nes_template.txt)を出力します。  
エディタでmbc.cを開いて、冒頭の配列にコピペしてください。

最後に、  

$ make  

で一発でコンパイルできます。  

$ mariones  

で、４００フレームエミュレーションを行います。フレーム数を変えたかったら main.c をいじってください。  
一応、プロファイルフラグも入れています。　gmon.outも出てきますので gprofを使って  
プロファイル解析ができます。画像が出てきますので画面のデバッグができます。  
RICOH2A03.c のコメントアウトを外すとCPUのレジスタとかデバッグできます。  

## 問い合わせ先  
Hiroki Nakahara, ~~@oboe7man (twitter)~~
忙しいので対応できないかもしれません、すみません…
