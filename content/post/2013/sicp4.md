+++
title = "SICP 第2章「Building Abstractions with Data」【後編】"
date = "2013-12-08"
tags = ["Programming"]
+++

後半はGeneric Procedure関する解説。

オブジェクト指向の言語で開発を行っていると、「そもそも型とは」というようなことを考える機会がほとんどない。言語のプリミティブな機能を利用して、型の仕組みを実装してみるのはなかなか貴重な経験だ。

ここでは、Generic Procedureを実現するための戦略が3つ紹介される。

-   explicit dispatch

    総称関数側(「たす」、「ひく」など)が、データの型(「実数」、「複素数」など)を見て、実際に実行される函数を決定する。

    型が追加されるたびに、総称関数側の分岐を追加する必要がある。既存のコードを修正する必要があるので、余り得策とはいえない？

-   data-directed

    「総称函数名」、「型」、「実際に実行される函数」の結びつきを管理するテーブルを作成する。  

    総称関数内での分岐を作る必要はないが、新しい函数や型が追加されるたびにグローバルなテーブルを更新する必要がある。

-   message-passing
    "intelligent operation"ではなく、"intelligent data
    object"。つまり、型に応じた処理を行う「賢い処理」の代わりに、処理に応じて動作を変える「賢いデータ」を作るという方針。

message-passingは非常にオブジェクト指向的だ。直交座標系で複素数を表現する、以下の例が紹介されている。

    :::lisp
    (define (make-from-real-imag x y)
      (define (dispatch op)
        (cond ((eq? op 'real-part) x)
              ((eq? op 'imag-part) y)
              ((eq? op 'magnitude)
               (sqrt (+ (square x) (square y))))
              ((eq? op 'angle) (atan y x))
              (else
               (error "Unknown op -- MAKE-FROM-REAL-IMAG" op))))
      dispatch)

つまり、処理の塊としての函数が返却される。
利用する側は、以下の様なコードを書けば良い。

    :::lisp
    (define real-imag (make-from-real-imag 2 2))
    (real-imag 'magnitude)

    ;; 実行結果
    ;; gosh < 2.8284271247461903

かなり、オブジェクトっぽい。3章ではクロージャがでてくるので、いよいよデータと手続きが一緒になったデータを定義できるようになるだろう。

この後、継承などについても軽く触れていたが、この本ではクラスや継承などについて深くは扱わないようだ。もう少し理解が深まったら型システムなどもじっくり勉強してみたい。

<iframe src="http://rcm-fe.amazon-adsystem.com/e/cm?lt1=_blank&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=usay0b-22&o=9&p=8&l=as4&m=amazon&f=ifr&ref=ss_til&asins=4274069117" style="width:120px;height:240px;" scrolling="no" marginwidth="0" marginheight="0" frameborder="0"></iframe>
