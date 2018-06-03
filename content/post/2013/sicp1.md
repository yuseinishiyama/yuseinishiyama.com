+++
title = "SICP 第1章「Building Abstractions with Procedures」を読み終えて。(前編)"
date = "2013-10-27"
tags = ["Programming"]
+++

[先日の投稿「代替不可能なプログラマとは。」][代替不可能なプログラマとは。]にある通り、SICPを読み始め、先日第1章「Building
Abstractions with
Procedures」を読み終えた。実際には[先日の投稿][代替不可能なプログラマとは。]より少し前から読み始めていて、1章読み終えるのに3週間ほどかかったことになる。(さらに言うと、3ヶ月ほど前に、ビデオ講義を3時間分ほど鑑賞した。この時は、テキストが存在することを知らなかったのと、当たり前だが偉大なるハル・アベルソン氏とジェラルド・ジェイ・サスマン氏の早口の英語が聞き取れるはずもなく、あえなく挫折した。)

学習環境は以下。

・テキストは原著で。

テキストは原著がオンラインで閲覧できるので、原著で読むことにした。日本語訳に対する酷評を聞き過ぎたせいで、理解できなかった場合に翻訳のせいにしてしまいそうだからだ。それと、語学能力の訓練にもなればと思っている。英語だけをやっている時間がないので、英語とプログラミングを一緒に効率良く学びたい。

・開発環境にはDrRacketを使用。

コマンドラインとかEmacsとかも試してみたが、結局一番楽そうなものに落ち着いた。

SICPを読んだ結果できるようになること、得られる知識というのははっきりしていなくて、ただただ今よりプログラミングを深く理解できるに違いないという予感だけがモチベーションになっている。SICPに関して「[Why
Structure and Interpretation of Computer Programs
matters][]」という記事を見つけた。

> Before SICP, the first CS course was almost always entirely filled
> with learning the details of some programming language. SICP is about
> standing back from the details to learn big-picture ways to think
> about the programming process. It focused attention on the central
> idea of abstraction -- finding general patterns from specific problems
> and building software tools that embody each pattern. It made heavy
> use of the idea of functions as data, an idea that's hard to learn
> initially, but immensely powerful once learned. (This is the same
> idea, in a different form, that makes freshman calculus so notoriously
> hard even for students who've done well in earlier math classes.) It
> fit into the first CS course three different programming paradigms
> (functional, object oriented, and declarative), when most other
> courses didn't even really discuss even one paradigm.

SICPはプログラミングの"big-picture"について語っているらしく、私の予感はある程度当たっているに違いない。

私の様なプログラミング新参者にとっては、日常的に利用するものはほぼすべて高度に抽象化されているのが当たり前だ。もちろん抽象化されているものを、そのインターフェースに見合った抽象化された思考によって用いることが業務では非常に重要だし、都度自分で全部作るというのはむしろ仕事ができない人間がやることだろう。だが、こういう作業をひたすらに進めていっても、「How
to Program」までにしかたどり着かず、全体像つまり「What is
Programming」を知ることは永遠にないのではないかという考えがが日増しに強くなってきた。これは道具の使い方を覚えているだけの作業ではないのか。この言語は最新版でこんなモダンな機能が追加された、このIDEではこんな便利な補完機能がある、エディタを自分好みにカスタマイズするには・・・。世の中の大半のことは道具を使うことであるはずなのに、プログラミングに関してはそれが大した意味をなさないように感じる。

道具には寿命がある。例えば物理や数学という道具は寿命が長い。そうとう大きなパラダイムシフトがなければ、その基板が崩れることはないだろう。だから、その道具を使えることには普遍的価値がある。私はピアノを演奏するが、ピアノはどうだろう。ピアノとはまさに近代音楽のパラダイムそのものを体現するようなもので、こちらもかなり普遍的だ。一方で、音楽のパラダイムは大きな変化が予想され、既に音楽に対するピアノというインターフェースは以前ほど重要ではなくなっている。楽器が弾けず、楽譜も読めない人が、音楽の世界で大成功する時代だ。だから、物理や数学に比べるとやや脆いかもしれない。さて、特定のプログラミングスキルはどうだろうか。これは、先の2つの例よりはるかに寿命が短いと言わざるをえない。ツールはもちろん、言語そのものや、プログラミングのパラダイムまで短期間で移り変わる。寿命が短いものほど、覚えることが知恵というよりは知識でしかなくなっていく。

とは言え、プログラミングという概念そのものの寿命は相当長いはずだ。詳細は大きく変化していったとしても、コンピュータが我々の生活を支えるという環境が変わってしまうということは、今のところ想像できない。だから、プログラミングを知っている、ということは紛れも無く知恵だろう。結局、情報が普遍的かどうかが、「知識」と「知恵」の境目である。だから、プログラミングの全体像について書かれたSICPで学べることは、知恵に違いない。ツールの使い方や、局所的なケースにやたらとこだわる人もいるし、突き詰めればそういうことも意味をなすかもしれないが、状況が変わればまた別のことを覚えなければならない。それでは技術のラットレースに飲み込まれていくだけで、体力がなくなったり、頭が硬くなったらその時点で終わりだ。使い古された「プログラマ30歳定年説」という言葉も、そういうラットレースに巻き込まれた人にとってはあながち間違いではないかもしれない。自分はそういう風になりたくない。過去の積み重ねが現在にも役立つように、自分のポートフォリオが豊かになるように、そういう風になりたい。だから、普遍的なプログラミングの本質を学びたいのだ。[（後編へ続く)][後編]

[代替不可能なプログラマとは。]: http://yuseinishiyama.com/posts/2013/10/23/necessary-programmer/
[Why Structure and Interpretation of Computer Programs matters]: http://www.cs.berkeley.edu/~bh/sicp.html
[後編]: http://yuseinishiyama.com/posts/2013/10/27/sicp2/
