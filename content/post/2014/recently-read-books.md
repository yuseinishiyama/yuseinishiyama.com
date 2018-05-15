+++
title = "プログラマ歴2年の文系出身プログラマがこの1年で読んだ本"
date = "2014-07-07"
+++

# はじめに

私は文系出身のプログラマであるが、今年の4月でようやくプログラマ歴2年になった。
その私がこの1年間で読んだ本を、整理も兼ねてまとめてみる。

# 読んだ本一覧

## コンピュータサイエンス全般

「文系出身の」と見出しをつけたものの、文系・理系がどうとかあまりそういうことにはこだわらないし、また、こだわるのは愚かだと思っている。
かの[ハイゼンベルク][]も

> 科学は人間によってつくられるものであります。これはもともと自明のことですが、簡単に忘れてしまいがちです。
> このことをもう一度思いかえすならば、、しばしば嘆かれるような人文科学-芸術と、
> 技術-自然科学という二つの文化の間にある断然を少なくすることに役立つのではないでしょうか。\<W.ハイゼンベルク(1974)『部分と全体』みすず書房 p.vii\>

と文理の壁を超えることの重要性を説いている。こうした哲学的基礎があったからこそ、
[不確定性原理][]という非常に深遠な考えを導き出すことが出来たのであろう(私程度の知性では全く理解できない原理ではあるが)。
とはいえ、文系出身の私にとってはアカデミックな教養がないというのは大変なコンプレックスであり、

「「コンピュータサイエンス」」

とか、ましてや

「「計算機科学」」

などと唱えられると、「ははぁ、参りました」となってしまう。
というわけで、そういうアカデミックな雰囲気を醸し出す本にはめっぽう弱い。

### コンピュータの構成と設計〜ハードウエアとソフトウエアのインタフェース

<iframe src="http://rcm-fe.amazon-adsystem.com/e/cm?lt1=_blank&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=usay0b-22&o=9&p=8&l=as4&m=amazon&f=ifr&ref=ss_til&asins=4822284786" style="width:120px;height:240px;" scrolling="no" marginwidth="0" marginheight="0" frameborder="0"></iframe>

通称パタヘネ。
3章までしか読めていない。これを読んだからといって、自分の関わっているレイヤには殆ど役に立たない気もするが、
自分の書いたプログラムがどういう形で実際に実行されるに至るのかをイメージできるようになった。
レジスタ云々とかCPUアーキテクチャがどうの、とか言われてもなんとか理解できるようになった気がする。
そのうち効いてくる知識なのではないかと思っている。

### 計算機プログラムの構造と解釈

<iframe src="http://rcm-fe.amazon-adsystem.com/e/cm?lt1=_blank&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=usay0b-22&o=9&p=8&l=as4&m=amazon&f=ifr&ref=ss_til&asins=489471163X" style="width:120px;height:240px;" scrolling="no" marginwidth="0" marginheight="0" frameborder="0"></iframe>

通称SICP。HTML版を原文で読んでいるが、最近日本語訳が公開されたので併用している。
4章の途中。5章でレジスタマシンを実装しないと読んだとは言えないのだろうが、それでもコンピュータサイエンスの様々な領域へアクセスし易くなったと思う。
特に遅延ストリームの章で、プログラムで無限を扱う方法について知ることが出来たのが印象的だった。
これはなんとしても最後まで終わらせたいのだが、あまりに練習問題が多いのと、本当に理解できているのかが不安になってモヤモヤしてくるのが難点か。

### 人工知能概論

<iframe src="http://rcm-fe.amazon-adsystem.com/e/cm?lt1=_blank&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=usay0b-22&o=9&p=8&l=as4&m=amazon&f=ifr&ref=ss_til&asins=4320121163" style="width:120px;height:240px;" scrolling="no" marginwidth="0" marginheight="0" frameborder="0"></iframe>

人工知能に関する様々な概念のイントロダクションを集めた本。他のより難解な文章を読んだ時に既視の単語に出会う確率が増え、
脳の負荷が下がるというメリットはあるが、あくまでそこまでという感じ。

## コンパイラ/プログラミング言語/形式言語

### すごいHaskell楽しく学ぼう!

<iframe src="http://rcm-fe.amazon-adsystem.com/e/cm?lt1=_blank&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=usay0b-22&o=9&p=8&l=as4&m=amazon&f=ifr&ref=ss_til&asins=4274068854" style="width:120px;height:240px;" scrolling="no" marginwidth="0" marginheight="0" frameborder="0"></iframe>

HTML版を原文で読んだ。途中までは関数型言語一般の知識という感じでサクサク読めたが、Applicativeあたりからは初見ではなかなか辛い。
map,reduce,filterなどの味をしめ、「写像」とかなんとかが頭にチラつくようになると、通常の制御文でプログラムする能力が著しく低下するのが最近の悩み。
もちろん圏論が分かるようになったなどということは口が裂けても言えない。

### プログラミング言語を作る

<iframe src="http://rcm-fe.amazon-adsystem.com/e/cm?lt1=_blank&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=usay0b-22&o=9&p=8&l=as4&m=amazon&f=ifr&ref=ss_til&asins=4774138959" style="width:120px;height:240px;" scrolling="no" marginwidth="0" marginheight="0" frameborder="0"></iframe>

図書館で[ドラゴンブック][]に目を通して儚くも散った後に読んだ本。yaccとかlexとかの説明がある。

### 白と黒のとびら

<iframe src="http://rcm-fe.amazon-adsystem.com/e/cm?lt1=_blank&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=usay0b-22&o=9&p=8&l=as4&m=amazon&f=ifr&ref=ss_til&asins=4130633570" style="width:120px;height:240px;" scrolling="no" marginwidth="0" marginheight="0" frameborder="0"></iframe>

構造主義の先駆者、[ウラジミール・プロップ][]が一瞬で分類しそうなベタベタなビルドゥングスロマンを読んでいる内に、オートマトンの基礎が分かった気になる奇書。

### きつねさんでも分かるLLVM〜コンパイラを自作するためのガイドブック〜

<iframe src="http://rcm-fe.amazon-adsystem.com/e/cm?lt1=_blank&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=usay0b-22&o=9&p=8&l=as4&m=amazon&f=ifr&ref=ss_til&asins=4844334158" style="width:120px;height:240px;" scrolling="no" marginwidth="0" marginheight="0" frameborder="0"></iframe>

自分がきつねさん以下であることが分かる本。というのは冗談で、これを読めばLLVMで何かできそうな気がするレベルになれる。
主にフロントエンドに興味があるので、5章までしか読んでいない。LLVMのドキュメントを読んだ後に読むと一気に理解が深まる。

### プログラミング言語の基礎概念

<iframe src="http://rcm-fe.amazon-adsystem.com/e/cm?lt1=_blank&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=usay0b-22&o=9&p=8&l=as4&m=amazon&f=ifr&ref=ss_til&asins=4781912850" style="width:120px;height:240px;" scrolling="no" marginwidth="0" marginheight="0" frameborder="0"></iframe>

型理論の文脈で出てくる記号操作に慣れることができる。もっと賢ければこの後TAPLを読んだりするのだろうが自分には難しい。

<iframe src="http://rcm-fe.amazon-adsystem.com/e/cm?lt1=_blank&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=usay0b-22&o=9&p=8&l=as4&m=amazon&f=ifr&ref=ss_til&asins=4274069117" style="width:120px;height:240px;" scrolling="no" marginwidth="0" marginheight="0" frameborder="0"></iframe>

## iOS/Objective-C関連

これはもっぱら業務でやるので必須であった。私は人がなんと言おうと、Objective-Cという言語でキャリアをスタートさせることが出来てよかったと思っている。
Smalltalk由来の由緒正しいオブジェクト指向言語でありながら、一方でC言語のスーパーセットでしかないという特徴を持つこの言語のお陰で、
プログラミングを様々なレイヤーで考えることができるようになった。これから[Swiftを書いていくようになる][]のかもしれないが、この言語で得た知識や関心は無駄にはならないと思っている。
初心者向けの本から、そうでないものまで幅広く読んだが、特に印象に残った本を挙げる。

### Dynamic Objective-C

<iframe src="http://rcm-fe.amazon-adsystem.com/e/cm?lt1=_blank&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=usay0b-22&o=9&p=8&l=as4&m=amazon&f=ifr&ref=ss_til&asins=4861006414" style="width:120px;height:240px;" scrolling="no" marginwidth="0" marginheight="0" frameborder="0"></iframe>

Objective-C関連の本では最も好きな本だ。Objective-Cの思想とそれをどのように活かすかということを分かりやすく解説している。
これを読んで、如何にしてObjective-Cがその動的特性を実現しているかを理解することができた。
また、この本のお陰で（せいで？)コンピュータ言語そのものにも興味を持つようになった。

### エキスパートObjective-Cプログラミング

<iframe src="http://rcm-fe.amazon-adsystem.com/e/cm?lt1=_blank&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=usay0b-22&o=9&p=8&l=as4&m=amazon&f=ifr&ref=ss_til&asins=4844331094" style="width:120px;height:240px;" scrolling="no" marginwidth="0" marginheight="0" frameborder="0"></iframe>

Objective-C中級者以上を名乗りたいのなら必須か。多分にマニアックであるが、GCDの章は誰が読んでも役に立ちそうである。

### Effective Objective-C 2.0

<iframe src="http://rcm-fe.amazon-adsystem.com/e/cm?lt1=_blank&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=usay0b-22&o=9&p=8&l=as4&m=amazon&f=ifr&ref=ss_til&asins=4798134198" style="width:120px;height:240px;" scrolling="no" marginwidth="0" marginheight="0" frameborder="0"></iframe>

Objective-Cでコードを書く上のベストプラクティスが良くまとまっている。
とりあえずこれを読んで理解しておけば、Objective-C開発で無作法なことをしないですむだろう。
それにしても「ぶらぶらポインタ」という訳は如何なものだろうか...。

### iOS Core Data 徹底入門

<iframe src="http://rcm-fe.amazon-adsystem.com/e/cm?lt1=_blank&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=usay0b-22&o=9&p=8&l=as4&m=amazon&f=ifr&ref=ss_til&asins=4798039799" style="width:120px;height:240px;" scrolling="no" marginwidth="0" marginheight="0" frameborder="0"></iframe>

賛否両論あるCoreDataだが、これを読んでおけばどんな機能があるか一通りさらえると思われる。
Cのライブラリ関数を使用してデータを永続化するところから始まり、NSUserDefaultやSQLiteも解説した上で、
そこからCoreDataについて解説するスタイルも好印象。

## ツール関連

### 入門UNIXシェルプログラミング

<iframe src="http://rcm-fe.amazon-adsystem.com/e/cm?lt1=_blank&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=usay0b-22&o=9&p=8&l=as4&m=amazon&f=ifr&ref=ss_til&asins=4797321946" style="width:120px;height:240px;" scrolling="no" marginwidth="0" marginheight="0" frameborder="0"></iframe>

とりあえずUNIXと仲良くなりたいので。
でも何回書いても忘れるし、そのうちシェルの文法にイライラしてくる。
もちろん本自体は素晴らしい。

### Emacs実践入門

<iframe src="http://rcm-fe.amazon-adsystem.com/e/cm?lt1=_blank&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=usay0b-22&o=9&p=8&l=as4&m=amazon&f=ifr&ref=ss_til&asins=4774150029" style="width:120px;height:240px;" scrolling="no" marginwidth="0" marginheight="0" frameborder="0"></iframe>

とりあえずここに書かれている設定とパッケージを導入しておけば、エディタとしては十分でないかと思う。
るびきち本より読みやすい印象。ただし、少し古い情報なので、これからのモダンなEmacs環境には必須と思われる[Cask][]や[helm][]の解説がないので注意。

## ゲームプログラミング関連

一時期業務でUnity3Dを利用していた時に読んだ本たち。ちなみに今ではほとんど触っていない。
ゲームならではの最適化手法やシェーダの基礎などを学ぶ上で、ハードウェアを意識できるようになったことは大きい。
特に一般的なシステム開発では考える機会が殆ど無い[(かわいそうな)GPU][]の恩恵が理解できるようになった。

### ゲームの作り方 Unityで覚える遊びのアルゴリズム

<iframe src="http://rcm-fe.amazon-adsystem.com/e/cm?lt1=_blank&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=usay0b-22&o=9&p=8&l=as4&m=amazon&f=ifr&ref=ss_til&asins=4797370084" style="width:120px;height:240px;" scrolling="no" marginwidth="0" marginheight="0" frameborder="0"></iframe>

ライトな見た目だが、ゲーム開発の門外漢がゲーム特有のロジックを知る上で役に立つテクニックが詰まっていて、
さすがゲーム開発のプロが書く本は違うなと感動した記憶がある。
とりあえず、地形を作って、オブジェクトおいて、物理演算つけて...みたいな書籍とは一線を画す。
プロシージャルメッシュなどは意外とすぐに必要になるが、解説している本・Webサイトがあまりなかったのでとても有難かった。

### ゲームプログラマになる前に覚えておきたい技術

<iframe src="http://rcm-fe.amazon-adsystem.com/e/cm?lt1=_blank&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=usay0b-22&o=9&p=8&l=as4&m=amazon&f=ifr&ref=ss_til&asins=4798021180" style="width:120px;height:240px;" scrolling="no" marginwidth="0" marginheight="0" frameborder="0"></iframe>

とても分厚いので流し読みになったが、基礎知識が網羅されていると感じた。
Z-Bufferとか衝突判定にまつわる問題などは知らないと、トラブった時に詰む。

## 数学関連

これもコンプレックスがある。中高の時は、意味論抜きで記号操作だけをやらされるのが無理すぎて完全に放棄していた。
親や教師からも数学落第生みたいなレッテルを貼られて、数学ができないという自己暗示にかかっていたと思う。

中高で数学ができる人は一体何者だったのだろうと思うが勝手に推測してみた。

1. 意味がなくとも、解を得るという作業自体にモチベーションを見いだせた者。
1. 早熟で幼い時から思考の抽象度が高く、数学と日常生活に関連性を見出すことができた者。
1. 数学自体は特に好きではないが、医者などの理系の仕事に就きたいという意思を強くもった者

ともかく、プログラマになった今、ある程度数学の恩恵を受けれるようになった訳だから初歩からやり直そうと思い、色々手にとってみた。

### 数学ガールシリーズ

<iframe src="http://rcm-fe.amazon-adsystem.com/e/cm?lt1=_blank&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=usay0b-22&o=9&p=8&l=as4&m=amazon&f=ifr&ref=ss_til&asins=4797341378" style="width:120px;height:240px;" scrolling="no" marginwidth="0" marginheight="0" frameborder="0"></iframe>

### オイラーの贈物

<iframe src="http://rcm-fe.amazon-adsystem.com/e/cm?lt1=_blank&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=usay0b-22&o=9&p=8&l=as4&m=amazon&f=ifr&ref=ss_til&asins=448601863X" style="width:120px;height:240px;" scrolling="no" marginwidth="0" marginheight="0" frameborder="0"></iframe>

### 素数夜曲

<iframe src="http://rcm-fe.amazon-adsystem.com/e/cm?lt1=_blank&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=usay0b-22&o=9&p=8&l=as4&m=amazon&f=ifr&ref=ss_til&asins=4486019245" style="width:120px;height:240px;" scrolling="no" marginwidth="0" marginheight="0" frameborder="0"></iframe>

### フーリエの冒険

<iframe src="http://rcm-fe.amazon-adsystem.com/e/cm?lt1=_blank&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=usay0b-22&o=9&p=8&l=as4&m=amazon&f=ifr&ref=ss_til&asins=4906519008" style="width:120px;height:240px;" scrolling="no" marginwidth="0" marginheight="0" frameborder="0"></iframe>

### プログラマのための線形代数

<iframe src="http://rcm-fe.amazon-adsystem.com/e/cm?lt1=_blank&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=usay0b-22&o=9&p=8&l=as4&m=amazon&f=ifr&ref=ss_til&asins=4274065782" style="width:120px;height:240px;" scrolling="no" marginwidth="0" marginheight="0" frameborder="0"></iframe>


やはり苦手意識があるものを克服することは難しく、上記のような内容は高校生の内に理解できてしまう者も沢山いるのだろうが、私には少々苦痛であった。
そもそも数学自体が好きなわけではないので、なんらかのプログラミングの問題とセットになっていないとすぐに飽きてしまう。
とはいえ、目が慣れてくるもので、画像処理や機械学習のごく初歩的な文章を読むときなどに、なんとか数式が理解できるという程度にはリカバーできた。

## プログラマ論

### 闘うプログラマー

<iframe src="http://rcm-fe.amazon-adsystem.com/e/cm?lt1=_blank&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=usay0b-22&o=9&p=8&l=as4&m=amazon&f=ifr&ref=ss_til&asins=4822740161" style="width:120px;height:240px;" scrolling="no" marginwidth="0" marginheight="0" frameborder="0"></iframe>

Windows NTの開発設計者であり、怒ると壁を殴って穴をあけるプログラマ、[デビッド・カトラー][]について知ることができる。
プログラマとしてプロダクトを完成に近づけることを徹底するその様は見習わなくてはいけないと考えさせられた。

### 情熱プログラマー

<iframe src="http://rcm-fe.amazon-adsystem.com/e/cm?lt1=_blank&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=usay0b-22&o=9&p=8&l=as4&m=amazon&f=ifr&ref=ss_til&asins=4274067939" style="width:120px;height:240px;" scrolling="no" marginwidth="0" marginheight="0" frameborder="0"></iframe>

プログラマの処世術。

### Team Geek

<iframe src="http://rcm-fe.amazon-adsystem.com/e/cm?lt1=_blank&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=usay0b-22&o=9&p=8&l=as4&m=amazon&f=ifr&ref=ss_til&asins=4873116309" style="width:120px;height:240px;" scrolling="no" marginwidth="0" marginheight="0" frameborder="0"></iframe>

自己修正的なプログラマ達はゼネラリストになり、やがて人格者に。
少なくとも、「謙虚、尊敬、信頼」の意識は記憶に留めておく必要があると感じた。
[Larry Wall][]の唱えた「怠慢、短気、傲慢」はあくまでコンピュータに対して推奨される態度であり、これを人間にやると[モヒカン][]と呼ばれるのでは。

## ハッカー文化

### ハッカーズ大辞典

<iframe src="http://rcm-fe.amazon-adsystem.com/e/cm?lt1=_blank&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=usay0b-22&o=9&p=8&l=as4&m=amazon&f=ifr&ref=ss_til&asins=475614084X" style="width:120px;height:240px;" scrolling="no" marginwidth="0" marginheight="0" frameborder="0"></iframe>

ハッカー文化に関する、パロディ辞書。中古で買った。
なぜか表紙がベトベトしているのだが、元の所有者である[J.Random Hacker][]の手汗だろうか。
たまにパラパラめくると面白い。例えば、こんな調子だ。

> program
> n.1. コンピュータが人の入力をエラーメッセージに変えられるようにするために、コンピュータにかける呪文。
> 2. 実験認識論の演習。
> 3. 表向きはコンピュータに指示するためのものだが、ほかのプログラマがそれを理解できなければ間違いなく失敗に終わる芸術の一形態。

よく出来ている。

### ゲーデル・エッシャー・バッハ(3周目)

<iframe src="http://rcm-fe.amazon-adsystem.com/e/cm?lt1=_blank&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=usay0b-22&o=9&p=8&l=as4&m=amazon&f=ifr&ref=ss_til&asins=4826901259" style="width:120px;height:240px;" scrolling="no" marginwidth="0" marginheight="0" frameborder="0"></iframe>

これを読んでプログラマを志したという、私のバイブル。また、読みなおしてみた。
何回読んでもやっぱり混乱してくるが、気がつけば物の考え方がやたらに影響されている。
"Think Once, Run Everywhere"なLispと自己言及。疑似科学の典型という人も多い。

どんな本と言われてもなかなか言語化しにくいので、著者の言葉を引用する。

> 知的な行動をプログラム化することがどうして可能なのだろうか？これは最も見えすいた用語の矛盾ではなかろうか？
> 本書の主要な目的のひとつは、読者の一人一人がこの外見上の矛盾にまともに対面するように仕向け、これをよく味わい、考え、分解し、そこで転げまわって、
> そして最後には、形式性と非形式性、生物と無生物、柔軟と硬直の間の超えがたく見える隔たりについて、新しい洞察を得られるようにすることである。

### 禅とオートバイ修理技術

<iframe src="http://rcm-fe.amazon-adsystem.com/e/cm?lt1=_blank&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=usay0b-22&o=9&p=8&l=as4&m=amazon&f=ifr&ref=ss_til&asins=415050332X" style="width:120px;height:240px;" scrolling="no" marginwidth="0" marginheight="0" frameborder="0"></iframe>

絶対主義者からの暴力を受け続け、相対主義というオアシスを見つける。
しかし、相対主義者であるという理由から相対主義の絶対性も主張できなくて、[アァァァァ][]な人には至高。
ソクラテス・プラトン・アリストテレスらのギリシア哲学の争点が、今なお意味を成す理由が実感できる。

## その他

### 論理学

<iframe src="http://rcm-fe.amazon-adsystem.com/e/cm?lt1=_blank&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=usay0b-22&o=9&p=8&l=as4&m=amazon&f=ifr&ref=ss_til&asins=4130120530" style="width:120px;height:240px;" scrolling="no" marginwidth="0" marginheight="0" frameborder="0"></iframe>

型理論とかの話が分かるようになりたいなぁ、などと思っていたら存在量化子程度で吐き気を催したので、基礎から勉強しなおそうと読んでみた。
初歩の初歩から始まる上にそれほど文量があるわけでもないのに、アリストテレスの[古典論理][]から[フレーゲ][]の[述語論理][]、そして[ゲーデル数][]までを平易に解説する何気にすごい本。
やはりゲーデルは難しいので十分に理解していないが、[述語論理][]までの論理記号がまともに読めるようになったのは大きい。

### 悪魔の辞典

<iframe src="http://rcm-fe.amazon-adsystem.com/e/cm?lt1=_blank&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=usay0b-22&o=9&p=8&l=as4&m=amazon&f=ifr&ref=ss_til&asins=4003231228" style="width:120px;height:240px;" scrolling="no" marginwidth="0" marginheight="0" frameborder="0"></iframe>

これも、パロディ辞書であるが、上述のものよりもっと普遍的である。
以下の様なものが並んでいる。

> 学識 (learning n.)
> 学問に勤勉な者の特色である一種の無知。

かなり過激で人に言えないようなものも多いが、思い詰めてる時に読むと良い意味で力が抜ける本。

### 圏論による論理学―高階論理とトポス

<iframe src="http://rcm-fe.amazon-adsystem.com/e/cm?lt1=_blank&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=usay0b-22&o=9&p=8&l=as4&m=amazon&f=ifr&ref=ss_til&asins=4130120573" style="width:120px;height:240px;" scrolling="no" marginwidth="0" marginheight="0" frameborder="0"></iframe>

ハハッ。余裕余裕。

### 哲学入門

<iframe src="http://rcm-fe.amazon-adsystem.com/e/cm?lt1=_blank&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=usay0b-22&o=9&p=8&l=as4&m=amazon&f=ifr&ref=ss_til&asins=448006768X" style="width:120px;height:240px;" scrolling="no" marginwidth="0" marginheight="0" frameborder="0"></iframe>

待望されていた、[唯物論者][]のための哲学入門。
この本を手に取り、哲学を虚学といってくる蛮人を殴ろう。

***

この倍ぐらいの数は読んだ気がするが、思い出せない本は読んでいないのと同じだろう。あ、これも読んだ。

### 読んでいない本について堂々と語る本

<iframe src="http://rcm-fe.amazon-adsystem.com/e/cm?lt1=_blank&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=usay0b-22&o=9&p=8&l=as4&m=amazon&f=ifr&ref=ss_til&asins=4480837167" style="width:120px;height:240px;" scrolling="no" marginwidth="0" marginheight="0" frameborder="0"></iframe>

読書に関する本、つまりメタ本。本を読み終えた、とはいかなる状態だろうか、という問。

# 結論

全体としてはやはり何らかの形でコンピュータサイエンスに関わるような本が多くなったと思う。
では、これらの本を読んで、私はコンピュータサイエンスの教養を身につけることができたのだろうか。
そんなことはない、結局コンピュータサイエンスとは何なのかということは分からずじまいだ。
逃げ水の如く、知れば知るほど「知らないといけないような気がする」ものが現れてくる。
きっと、大人になってから得た幸せが、青春を埋め合わせることができないのと同じなのだろう。

そういえば、なんでも良いから知識をつけないといけないという強迫観念にとらわれていた時に[こんなスレッド][]を見つけた

その中にこんな書き込みがある。

> By explore do you mean "I have no idea what is going on here but I want to understand" or do you mean "I'm awesome in computer science but I want to be Knuth 2.0"
> If it's the former then #4 is very much out of place. Of all the people I know who have heard of the series, I don't know one person who has actually read and understood a significant piece of it.
> I suspect you got AoCP as a recommendation from someone who never read it but thinks they should one day.
> If this is something you can read and have a use for doing so then you will know before ever opening it. uOtherwise you probably want to save yourself the money finding out the hard way that the book is not for you.

> 見たところ、君が言っていることは「良くわからないんだけど、とにかく知りたい。」もしくは「俺はコンピュータサイエンスに精通しているけど、クヌース2.0になりたい。」という感じだろうか。
> (中略)
> もし、これらの本を読むことができて、実際に役立つのであれば、君は本を開く前から既にその内容を理解しているのだろう。
> そうでなければ、おそらく君は、その本が自分に相応しく無いという困難な道を知るためのお金を節約したいはずだ。

結構痛いところを突いている。自分は「とにかく知りたい」というフェーズを抜けないといけないだろう。
そして、向こう1,2年はこれほどインプットに費やさないとも思う。

しかし、こうやって闇雲にインプットを得ているつもりでも、そこには偏りがあって、
「何だか良くわからないけど知りたいこと」と「何だか良くわからないけど知ろうとさえ思わないこと」というのが明確に出てくる。
変わろうとする自分の中にある、どうしても変えられない部分が明確になっただけでもこの2年余りは貴重だったのかもしれない。

[ハイゼンベルク]:http://ja.wikipedia.org/wiki/%E3%83%B4%E3%82%A7%E3%83%AB%E3%83%8A%E3%83%BC%E3%83%BB%E3%83%8F%E3%82%A4%E3%82%BC%E3%83%B3%E3%83%99%E3%83%AB%E3%82%AF
[不確定性原理]:http://ja.wikipedia.org/wiki/%E4%B8%8D%E7%A2%BA%E5%AE%9A%E6%80%A7%E5%8E%9F%E7%90%86
[フレーゲ]:http://ja.wikipedia.org/wiki/%E3%82%B4%E3%83%83%E3%83%88%E3%83%AD%E3%83%BC%E3%83%97%E3%83%BB%E3%83%95%E3%83%AC%E3%83%BC%E3%82%B2
[ゲーデル数]:http://ja.wikipedia.org/wiki/%E3%82%B2%E3%83%BC%E3%83%87%E3%83%AB%E6%95%B0
[古典論理]:http://ja.wikipedia.org/wiki/%E5%8F%A4%E5%85%B8%E8%AB%96%E7%90%86
[述語論理]:http://ja.wikipedia.org/wiki/%E8%BF%B0%E8%AA%9E%E8%AB%96%E7%90%86
[唯物論者]:http://ja.wikipedia.org/wiki/%E5%94%AF%E7%89%A9%E8%AB%96
[Swiftを書いていくようになる]:http://yuseinishiyama.com/posts/2014/06/25/swift-lt1/
[モヒカン]:http://ja.wikipedia.org/wiki/%E3%83%A2%E3%83%92%E3%82%AB%E3%83%B3%E6%97%8F_(%E3%83%8D%E3%83%83%E3%83%88%E7%94%A8%E8%AA%9E)
[J.Random Hacker]:http://en.wikipedia.org/wiki/J._Random_Hacker
[Cask]:https://github.com/cask/cask
[helm]:https://github.com/emacs-helm/helm
[アァァァァ]:http://ja.wikipedia.org/wiki/%E7%9B%B8%E5%AF%BE%E4%B8%BB%E7%BE%A9#.E7.9B.B8.E5.AF.BE.E4.B8.BB.E7.BE.A9.E3.81.B8.E3.81.AE.E7.B5.B6.E5.AF.BE.E4.B8.BB.E7.BE.A9.E7.9A.84.E7.AB.8B.E5.A0.B4.E3.81.8B.E3.82.89.E3.81.AE.E6.89.B9.E5.88.A4
[ドラゴンブック]:http://www.amazon.co.jp/dp/4781905854
[ウラジミール・プロップ]:http://ja.wikipedia.org/wiki/%E3%82%A6%E3%83%A9%E3%82%B8%E3%83%BC%E3%83%9F%E3%83%AB%E3%83%BB%E3%83%97%E3%83%AD%E3%83%83%E3%83%97
[デビッド・カトラー]:http://ja.wikipedia.org/wiki/%E3%83%87%E3%83%B4%E3%82%A3%E3%83%83%E3%83%89%E3%83%BB%E3%82%AB%E3%83%88%E3%83%A9%E3%83%BC
[Larry Wall]:http://ja.wikipedia.org/wiki/%E3%83%A9%E3%83%AA%E3%83%BC%E3%83%BB%E3%82%A6%E3%82%A9%E3%83%BC%E3%83%AB
[(かわいそうな)GPU]:http://kagamin.net/hole/gpu/
[こんなスレッド]:http://www.reddit.com/r/books/comments/ch0wt/a_reading_list_for_the_selftaught_computer/
