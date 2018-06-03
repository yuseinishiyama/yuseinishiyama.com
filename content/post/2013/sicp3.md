+++
title = "SICP 第2章「Building Abstractions with Data」【前編】"
date = "2013-11-25"
tags = ["Programming"]
+++

1章が割りとすんなり終わったものだから、少し油断していた。2章からは分量も問題数も随分多くなりなかなか終わりそうにない。大体2章の60%ぐらいが終わったから、備忘録も兼ねてこの辺りで感想をまとめておこう。

ところで、Edsgar Dijkstraの言葉にこのようなものがある

> Being abstract is something profoundly different from being vague…
> The purpose of abstraction is not to be vague, but to create a new
> semantic level in which one can be absolutely precise.([「Best Programming Quotations」][]より)
>
> 抽象的であることと曖昧であることは全く違う。
> 抽象化の目的は、曖昧にすることではない。
> 抽象化は、新たなセマンティックレベルを創出し、
> そのレベルにおいて人は非常に正確で有り得る。(拙訳)

(ここでいうセマンティックレベルとは、抽象化の結果生まれる新しいレイヤーのことであろう。SICPではAbstraction
Barrierという言葉で述べられている概念だ。)

プログラミングをはじめた頃は、自分の頭の悪さを度々嘆いたものだ。特に深い再帰処理では、すぐにコールスタックを追うことができなくなる。こういうとき、自分のメモリがプログラミングをするには少なすぎる、そういう風に考えてしまうことが多かった。

だが、SICPをやり始めて一番身に沁みたことは、「抽象化\>記憶力」であるということだ。難しい問題を抽象化し、抽象化した部分をどんどんとモジュール化していけば、記憶力が良くなくても問題が解ける。つまり、モジュールの動作が保証されているのであれば、全体が把握しきれなくても良いのである。これは当然、単体テスト・自動テストなどが有用であることの理由にもなり得る。こんなことは当たり前かと思うかもしれない。しかし、いざやってみると、抽象化能力が不十分であることが露呈する。少なくとも私の場合はそうだった。

例えば、

listの長さを求める函数(length)
list同士を連結する函数(append)
listの全要素に任意の処理を行う函数(map)

を実装するとして、list構造に慣れていなければappendぐらいでも結構混乱してしまうものだ。だがそれは、これら3つ処理に共通する処理、つまり抽象化可能な処理が見えていないということに他ならない。その抽象化されるべき処理とは

「先頭の要素」と、「後述の要素に再帰的処理を行ったもの」との間の演算

である。この抽象化を実装すると、

```Lisp
(define (accumulate op initial sequence)
  (if (null? sequence)
      initial
      (op (car sequence)
          (accumulate op initial (cdr sequence)))))
```

というようになるわけだ(それにしても、高次の概念であるほど、英語で名前をつけるのが難しい。この処理にaccumulateという動詞を当て嵌めることは今の英語能力では到底出来ない…)。そして、これを用いれば、簡単に上記3つの問題に対処できるということがEx2.33.によって示される。

```Lisp
;; Exercise 2.33.  Fill in the missing expressions to complete the following definitions of some basic list-manipulation operations as accumulations:

;; (define (map p sequence)
;;   (accumulate (lambda (x y) <??>) nil sequence))
;; (define (append seq1 seq2)
;;   (accumulate cons <??> <??>))
;; (define (length sequence)
;;   (accumulate <??> 0 sequence))

(define (map p sequences)
  (accumulate (lambda (x y) (cons (p x) y)) '() sequences))
(define (append seq1 seq2)
  (accumulate cons (accumulate cons '() seq2) seq1))
(define (length sequence)
  (accumulate (lambda (x y) (+ 1 y)) 0 sequence))
```

ところで、記憶力の補完物として抽象化があるということは、プログラミング以外でも変わらない(人間の記憶力の弱さは、[ジョージ・ミラーのマジカルナンバー][]を考えれば明らかだ)。

例えば、「1ヶ月で300ページの本を読む」という目標は「昨日読んだ最後のページから10ページ読むことを1ヶ月間続ける」ということであるし、あらゆる目標の究極的な抽象化は「過去より現在のほうが進んでいる」ということである。そういう意味で、自己管理指標は、過去と現在の短い差分で計れるべきものでなくてはならない。

複雑な処理を抽象化し分解することで、対処すべき問題が明確になる。そういう意味でDijkstraは「precise」になれると言っているのであろう。人生も同じで、時に全体を見渡す必要があるが、「近視眼になれる仕組み作り」がなにより重要なのではないだろうか。

[「Best Programming Quotations」]: http://www.linfo.org/q_programming.html
[ジョージ・ミラーのマジカルナンバー]: http://ja.wikipedia.org/wiki/%E3%82%B8%E3%83%A7%E3%83%BC%E3%82%B8%E3%83%BB%E3%83%9F%E3%83%A9%E3%83%BC
