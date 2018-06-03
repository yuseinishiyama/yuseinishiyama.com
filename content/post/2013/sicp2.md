+++
title = "SICP 第1章「Building Abstractions with Procedures」を読み終えて。(後編)"
date = "2013-10-27"
tags = ["Programming"]
+++

[(前編はこちら)][]

1章から学べたことを列挙する。

・Schemeに慣れた。  

if、cond、define、let、lambdaなどを用いてプログラミングできるようになった。例の「カッコ」にもだいぶ慣れてきた。

・linear recursionとiterationの違いが分かるようになった。  

今以上に不慣れな時のコードではあるが、以下のようにrecursionとiterationで再帰が書けるようになった。後者のほうが遥かに効率が良い。

```lisp
;recursion
(define (f n)
  (cond ((< n 3) n)
        (else (+ (f (- n 1))
                 (* 2 (f (- n 2)))
                 (* 3 (f (- n 3)))))))

;iteration
(define (f-iter product product-1 product-2 counter max-count)
  (cond ((> counter max-count) product)
        (else (f-iter (+ product (* 2 product-1) (* 3 product-2)) product product-1 (+ counter 1) max-count))))
```

・ファーストクラスオブジェクトについて理解した。  

当然Lispをやるのだから事前に知っていたことではあったが、Procedureを返す関数とかをたくさん書いているうちに以前よりはっきり「ファーストクラス感(？)」を理解できるようになった。

>  In general, programming languages impose restrictions on the ways in
> which computational elements can be manipulated. Elements with the
> fewest restrictions are said to have first-class status. Some of the
> \`\`rights and privileges'' of first-class elements are:
>
> They may be named by variables.  
>  They may be passed as arguments to procedures.  
>  They may be returned as the results of procedures.  
>  They may be included in data structures.
>
> Lisp, unlike other common programming languages, awards procedures
> full first-class status. This poses challenges for efficient
> implementation, but the resulting gain in expressive power is
> enormous.

・数学的な考え方が身についた。  

いや、はっきりいって数学で躓いて飛ばしてしまった問題とかもたくさんあって、むしろ数学をなんとかしないとヤバイ感じがするが、それでも前よりは数学的な考えが身についたような気がする。数学が苦手で今まで無視し続けた自分としては、これだけたくさんの近似値を求める式を実装しただけでもかなり変わったと思う。

> Several of the numerical methods described in this chapter are
> instances of an extremely general computational strategy known as
> iterative improvement. Iterative improvement says that, to compute
> something, we start with an initial guess for the answer, test if the
> guess is good enough, and otherwise improve the guess and continue the
> process using the improved guess as the new guess.

数学ってこういうことなんだな、とほんの少し思えた。

どうせ、最初はほとんど理解できないんだろうな、と思った割には重要な点はかなり吸収できたと思う。とは言え、数学でつまずいて1/3くらいのExerciseはあいまいなままだから、なんとかしないといけない。どっちにしろ、最近3Dグラフィックスとか画像処理とか齧るようになってきて、数学の重要性を実感してきたところだし、ちょうど良いだろう。

さて、次はデータの抽象化だ。

  [(前編はこちら)]: http://yuseinishiyama.com/archives/184
    "SICP 第1章「Building Abstractions with Procedures」を読み終えて。(前編)"
