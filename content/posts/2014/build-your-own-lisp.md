+++
title = "\"Build Your Own Lisp\"をやってみて"
date = "2014-05-26"
tags = ["Programming"]
+++

# はじめに

私は、業務で主にObjective-Cを利用しているのだが、Cがよく分からないままObjective-Cを使っていると、なんともいえないモヤモヤした気持ちになる。低レイヤのAPIを使用するときや <s>、老害っぽいコード読む時など、</s>Cがよく分かっていないことを痛感させられる。Cをほとんど学んだことがないままで、CのスーパーセットであるObjective-Cをやっている。こうした境遇の人は結構多そうだ。そこで、Cを勉強するのに良いテーマが無いかと探していたところ下記のサイトを発見した。

[Buil Your Own Lisp](http://www.buildyourownlisp.com/)

うん、これしかないだろう。何かの言語を学ぶときにLispの処理系を作るのは定番だ。CでLispを実装し、HaskellでLispを実装し、LispでLispを実装するのだ(超言語抽象)。というわけで、このチュートリアルを最後までやってみたのだが、日本語の記事でこの内容に言及しているものはほとんどなかったので、どんな内容であったのか簡単にまとめてみた。一部翻訳があるが、完全に意訳である。ご了承いただきたい。

# どんな人がやるべきか

これは著者が冒頭で述べている通りである。

> This book is for anyone wanting to learn C, or who has once wondered how to build their own programming language. This book is not suitable as a first programming language book, but anyone with at least some minimal programming experience, in any language, should find something new and interesting inside.

> この本が対象としているのはCを学びたい人、もしくは、オリジナルの言語の作り方に一度でも関心を持ったことがある人だ。プログラミングを学ぶ最初の本としてはふさわしくないかもしれない。でも、いずれかの言語で最低限のプログラミング経験がある人なら、この本で未知の興味深い事柄を学べるはずだ。

実用的な言語の実装について知識のある人からすれば、お遊びのような内容だろうが、そうでない人であれば結構楽しめる。全部で16章だが、どれも短く、最終的に実装されるコード数も1000行程度である。私は1日1〜2章ずつ進めて、2週間弱で終わらせることができた。忙しい人でも挫折せずに完走できる分量だと思われる。

# 学べること

### Cの基礎

* 構造体
* ポインタ
* 手動メモリ管理

### Interpreterの基本的な動作

* 入力→AST→評価→プリント(パーサーは実装しないので、入力がいきなり抽象構文木になるようなイメージ)

### 変数スコープ

* グローバルスコープとローカルスコープの実装

### 言語への機能追加の流れ

* 型の追加
* 構文規則の追加(BNF風)
* 規則にマッチした際の処理の追加

### LISP、関数型言語の基礎

* S式(著者はQ式というものを導入してquote相当の機能を実現している)
* 再帰
* 畳み込み
* 遅延評価
* ラムダ式
* 部分適用

### 言語デザイン

* 組み込み関数とライブラリ


# 学べないこと

### 実用的な言語設計のための知識

* ユーザー定義型
* システムコール呼び出し
* ハッシュテーブルを利用した変数のルックアップ
* GC
* 末尾再帰最適化
* 静的型

やり終えてみると、最終章で述べられているように

> Although we've done a lot with our Lisp, it is still some way off from a fully complete, production strength programming language.

> オリジナルのLispでかなりのことをやり遂げたにも関わらず、実作業に耐えるような言語の完全な実装には程遠い。

ということが、身に沁みてよく分かる。とはいえ、さすがLisp。1000行程度でもかなり強力なものができあがるので、そこまでがっかりしないかもしれない。「電卓を作る」みたいなものよりは随分面白いと思う。

### Parserの実装

パーサーに関しては著者自身が作成したライブラリ[mpc](https://github.com/orangeduck/mpc)を利用するのみで、一切実装はしない。

> when I was learning about programming languages, I found the theory of formal languages fascinating. I really enjoyed getting into the mind set of thinking about languages more abstractly. I think this is a fun thing to teach, as it opens up a lot of new thoughts and avenues of exploration. In my mind, the actual implementation details are less important.

> プログラミング言語を学んだとき、私は形式言語の理論に魅力された。言語というものをより抽象的に考えるという思考方法に夢中になったのだ。...(中略)...私の考えでは、実装の詳細はそれほど重要ではない。

ということらしい。この部分は完全にブラックボックスなので他の書籍等をあたって勉強する必要がありそうだ。字句解析はともかく、構文解析に関しては重要なテーマがありそうなので、もう少し深く勉強したいところだ。

# 結論

CもLispも余裕な人や、コンピュータ言語の実装経験がある人にとっては目新しいことはないかもしれないが、そうでなければかなり有意義なチュートリアルだと感じた。実装した言語側でライブラリを作って動かす時はかなり嬉しいので、体験したことがない人にはぜひオススメしたい内容だ。

# 補足

全体を通してウィットに富んだ文章が特徴的だ。

> I attempted to research why Lisps use ; for comments, but it appears that the origins of this have been lost in the mists of time. In absence of truth I imagine it as a small rebellion against the imperative languages such as C and Java which use semicolons so shamelessly and frequently to separate/terminate statements. Compared to Lisp all these languages are just comments!

> 私はLispがなぜセミコロンをコメントに用いるのかを調べてみたが、その起源は時の流れの中で失われてしまっていた。史実を無視して、私は以下の様に想像している。Lispにおけるセミコロンの使用は、分離/終端の式として恥ずかしげもなく多量にセミコロンを使用するCやJavaのごとき言語へのちょっとした反抗なのだ。Lispと比較すると、これらの言語は単なるコメントでしかないということだ！

至言である(笑)
