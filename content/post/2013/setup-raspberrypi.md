+++
title = "Raspberry Piのセットアップ(NOOBS)。"
date = "2013-08-31"
+++

購入してからしばらく放置していたRaspberry Piをセットアップしてみる。

今のところRaspberry
Piでやりたいことはないから、セットアップするに足る目的もないけどなぁ、などと思いつつ作業を開始する。すると、どうしたことか全然上手くいかないので、セットアップすることが目的になって、Raspberry
Piが起動した時には、なんとも言えない達成感が・・・。セットアップとは得てして、このような性質を持った作業である。

さて、私の「セットアップ観」など何の役にも立たないので、私がセットアップの際に困った事を展開したい。

まずインストールするOSをダウンロードするために、下記サイトを見る。

<http://www.raspberrypi.org/downloads>

「初心者はNOOBSを利用してOSをインストールするのが、おすすめ」ということなので、それに従う(ちなみにnoobは「新参者」という意味らしい)。「俺は利口だから中級者以上の扱いで頼むぜ」という感じで臨んで、マニュアルで色々やろうとしたら散々な結果になった、などということはぜひとも避けたい。

さて、Transcendの8GB(Class10)のSDカードをフォーマットし、そこにNOOBSのファイルを突っ込み、Raspberry
Piを起動した。

しかし、画面に何も表示されない。よくみると「ACT」のLEDが消灯しているので、それを手がかりに調べてみる。

<http://elinux.org/R-Pi_Troubleshooting>

上記のサイトによると、ACTはSDカードのステータスらしい。これは、所謂「[SDカードの相性問題][]」かと思い、別のSDカード(TOSHIBA)のもので試す。が、これも同じ結果。無念、Raspberry
Piは屋根裏行きかなと思ったものの、さすがに本体・入門用書籍・SDカードx2の代金を捨てるのはあまりに勿体ないので、もうちょっと調べてみる。すると、気になる投稿を見つけた。

<http://www.raspberrypi.org/phpBB3/viewtopic.php?t=48389&p=378706>

* * * * *

by **[mahjongg][]** » Fri Jun 28, 2013 12:26 am

Okay, my bad, I suddenly realized you didn't make any install yet, and
config.txt is (only) installed after you choose to install a linux
variant. config.txt doesn't exist on the card until then.

the NOOBS menu itself shouldn't need config.txt, as it uses the most
elementary VGA format that any (VGA) monitor should be able to display.

By default, NOOBS 1.2 will try to output over HDMI at your display's
preferred resolution. If you do not see any output on your HDMI display
or are using the composite output, you can manually select a different
output mode by pressing one of the following number keys on your
keyboard;

​1. HDMI preferred mode - this is the default display mode for NOOBS.  
2. HDMI safe mode - select this mode if your display is connected to
the Pi's HDMI connector and you cannot see anything on screen after the
Pi has booted.  
3. Composite PAL mode - select either this mode or composite NTSC mode
if you are using the composite RCA video connector on the Pi  
4. Composite NTSC mode

So in your case you will need to boot the Pi, wait for a second or so,
then press either 3 or 4 until you get a display on your monitor.

You can tell that your keypress has been registered as the green ACT LED
will turn on after the key has been pressed before switching off once
the display mode has been changed. If you still do no see a display
after the LED goes off then try another mode.

* * * * *

私も同じくVGAのディスプレイに出力していた。どうやらNOOBSをVGA出力で起動する場合にはキーボードのキーを押して、出力モードを選択しないといけないようだ。試しにキーボードの1を押してみる。すると、NOOBSのセットアップ画面が表示されたではないか！ということで、SDカードの問題ではなく、VGA出力が問題であったことが発覚した。こうして、無事にNOOBSからRaspbian
“wheezy”をインストールすることができたのだが、ここで再度つまずいた。OS起動後、またもや画面が表示されないのである。だが、VGAが問題であることは分かっているので、再度調べてみる。すると、こういう記事を発見した。

<http://nomolk.hatenablog.com/entry/2013/03/28/234247>

この記事と、記事のリンクにあるtwitterの投稿を参考にして、config.txtを編集し、最終的に、

hdmi\_safe=1  
hdmi\_drive=2

という設定にすることで、画面が出力された。こうして、ゆとりプログラマー(noob)はハードウェアの手痛い洗礼を受けながらも、無事にRaspberry
Piをセットアップすることができたのである。

VGA出力にしている人は参考にしていただきたい。

  [SDカードの相性問題]: http://elinux.org/RPi_SD_cards
  [mahjongg]: http://www.raspberrypi.org/phpBB3/memberlist.php?mode=viewprofile&u=14062
