+++
title = "SICP 3.1.2 The Benefits of Introducing Assignment"
date = "2013-12-19"
tags = ["Programming"]
+++

代入の効用について説明するだけにしては、Ex 3.5.
はちょっとやり過ぎな気もするが、なかなか興味深い内容ではある。

> Monte Carlo integration is a method of estimating definite integrals
> by means of Monte Carlo simulation. Consider computing the area of a
> region of space described by a predicate P(x, y) that is true for
> points (x, y) in the region and false for points not in the region.
> For example, the region contained within a circle of radius 3 centered
> at (5, 7) is described by the predicate that tests whether (x - 5)2 +
> (y - 7)2\< 32. To estimate the area of the region described by such a
> predicate, begin by choosing a rectangle that contains the region. For
> example, a rectangle with diagonally opposite corners at (2, 4) and
> (8, 10) contains the circle above. The desired integral is the area of
> that portion of the rectangle that lies in the region. We can estimate
> the integral by picking, at random, points (x,y) that lie in the
> rectangle, and testing P(x, y) for each point to determine whether the
> point lies in the region. If we try this with many points, then the
> fraction of points that fall in the region should give an estimate of
> the proportion of the rectangle that lies in the region. Hence,
> multiplying this fraction by the area of the entire rectangle should
> produce an estimate of the integral.
>
> Implement Monte Carlo integration as a procedure estimate-integral
> that takes as arguments a predicate P, upper and lower bounds x1, x2,
> y1, and y2 for the rectangle, and the number of trials to perform in
> order to produce the estimate. Your procedure should use the same
> monte-carlo procedure that was used above to estimate . Use your
> estimate-integral to produce an estimate of by measuring the area of a
> unit circle.
>
> You will find it useful to have a procedure that returns a number
> chosen at random from a given range. The following random-in-range
> procedure implements this in terms of the random procedure used in
> section 1.2.6, which returns a nonnegative number less than its
> input.8
>
> (define (random-in-range low high)  
>  (let ((range (- high low)))  
>  (+ low (random range))))

[モンテカルロ法][]を用いて、単位円の面積(円周率)を求める問題だ。

まず、乱数生成用の函数を定義する。[乱数を扱うので、srfi-27を使用する][]。

    :::lisp
    ;; ランダム数生成のため
    (use srfi-27)

    (define (random num)
      (* (random-real) num))

    (define (random-in-range low high)
      (let ((range (- high low)))
        (+ low (random range))))


次に、テキスト内でも使用されているモンテカルロ函数。試行回数と試行内容から、試行の成功確率を出力する。

    :::lisp
    (define (monte-carlo trials experiment)
      (define (iter trials-remaining trials-passed)
        (cond ((= trials-remaining 0)
               (/ trials-passed trials))
              ((experiment)
               (iter (- trials-remaining 1) (+ trials-passed 1)))
              (else
               (iter (- trials-remaining 1) trials-passed))))
      (iter trials 0))


そして、数値積分を行う。矩形の面積に1.0をかけているのは、単に小数で結果を見たいからだ。

    :::lisp
    (define (estimate-integral P x1 x2 y1 y2 trials)
      (define (rectangle-space)
        (* (- x2 x1)
           (- y2 y1)))
      (define (test)
        (P (random-in-range x1 x2) (random-in-range y1 y2)))
      (* (* (rectangle-space) 1.0)
         (monte-carlo trials test)))


最後に肝心の、正方形と(述語としての)円を与える。

    :::lisp
    (define (estimate-pi trials)
      (define (unit-circle)
        (lambda (x y) (>= 1 (+ (* x x)
                               (* y y)))))
      (estimate-integral (unit-circle)
                         -1 1 -1 1
                         trials))

大体100万回ぐらいで小数点以下2桁まで等しくなった。

    :::lisp
    ;; gosh> (estimate-pi 10)
    ;; 3.6
    ;; gosh> (estimate-pi 100)
    ;; 3.2
    ;; gosh> (estimate-pi 1000)
    ;; 3.196
    ;; gosh> (estimate-pi 10000)
    ;; 3.1216
    ;; gosh> (estimate-pi 100000)
    ;; 3.13956
    ;; gosh> (estimate-pi 1000000)
    ;; 3.142192
    ;; gosh> (estimate-pi 1000000)
    ;; 3.143176


  [モンテカルロ法]: http://ja.wikipedia.org/wiki/%E3%83%A2%E3%83%B3%E3%83%86%E3%82%AB%E3%83%AB%E3%83%AD%E6%B3%95
    "モンテカルロ法"
  [乱数を扱うので、srfi-27を使用する]: http://sicp.g.hatena.ne.jp/hyuki/20060505/random
    "SRFI-27疑似乱数発生器インタフェース"
