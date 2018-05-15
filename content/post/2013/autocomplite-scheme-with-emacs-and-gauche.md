+++
title = "EmacsでSchemeの自動補完を行う。"
date = "2013-12-01"
+++

環境構築に手間をかけたくない質なので、最初はDrRacketを利用してSchemeのコードを書いていた。

[Gaucheでメタプログラミング][]

しかし、上記のサイトなどから、Emacsでも「別ウインドウで評価しながら、ソースを書けること」が分った。  
その結果、結局Emacs+Gaucheの環境に落ち着いた。

しばらくはこれで満足していたし、新しい言語であれば、全て手で打つことも、理解を早めるという点では無駄ではないだろう。  

しかし、SICPの分量・難易度を考えると、あまりゆっくりコーディングしている暇はない。さすがに、最低限の自動補完が欲しくなった。

ところが、「scheme emacs
補完」とかで検索すると、結構面倒臭そうな方法が多い。  

結局、emacsのパッケージ管理システムを使ってauto-completeをインストールする方法が一番楽そうだ、という結論に達した。  

その方法を備忘録も兼ねて記載しておく。ちなみにEmacsのバージョンは24.2だ。

まず、パッケージ管理システムを利用したことのない人はリポジトリを設定しよう。

    :::lisp
    ;;;;;;;;;;;;;;;;;;;;Package;;;;;;;;;;;;;;;;;;;;
    (require 'package)
    (add-to-list 'package-archives
         '("marmalade" . "http://marmalade-repo.org/packages/")
         '("melpa" . "http://melpa.milkbox.net/packages/"))

設定後は

M-x package-list-packages

でインストール可能なパッケージの一覧が確認できる。「i」で選択し「x」でインストールの実行だ。  

もちろんここでは「auto-complete」をインストールする。2013年12月1日現在、最新版は1.4だ。

ちなみに、パッケージ管理システムでインストールしたelispファイルはデフォルトで、「\~/.emacs.d/elpa/パッケージ名」内に格納される。毎回、パッケージ名のディレクトリをload-pathとして追加するのも面倒なので、elpa以下は自動的にload-pathに追加されるようにしておくと便利だろう([特定のディレクトリ配下のディレクトリをload-pathに追加する][]を参考にした)。

    :::lisp
    ;;;;;;;;;;;;;;;;;;;;LoadPath;;;;;;;;;;;;;;;;;;;;
	;; load-pathを再帰的に追加。
	(defun add-to-load-path (&rest paths)
	(let (path)
        (dolist (path paths paths)
        (let ((default-directory (expand-file-name (concat user-emacs-directory path))))
        (add-to-list 'load-path default-directory)
         (if (fboundp 'normal-top-level-add-subdirs-to-load-path)
             (normal-top-level-add-subdirs-to-load-path))))))

    ;; elpa配下をロードパスに指定。
    (add-to-load-path "elpa")

さて、肝心のSchemeを自動補完可能にする方法だが、調べるとscheme-complete.elと組み合わせる方法ばかり出てくる。  
しかし、auto-completeの辞書ファイルが格納されているディレクトリ(
\~/.emacs.d/elpa/auto-complete-1.4/dict)には「scheme-mode」のファイルがあるので、コレを使う方法が今は一番簡単なのではないだろうか。もし、scheme-complete.elを使う利点があればどなたか教えてほしい。

とりあえず今回は、デフォルトで用意されている辞書ファイルにパスを通そう。

    :::lisp
	;;;;;;;;;;;;;;;;;;;;Auto-Complete;;;;;;;;;;;;;;;;;;;;
	(require 'auto-complete-config)
	(ac-config-default)
	(add-to-list 'ac-dictionary-directories "~/.emacs.d/elpa/auto-complete-1.4/dict")

これらの設定を有効にし、Scheme-modeに入れば、補完がポップアップで表示されるはずだ。

  [Gaucheでメタプログラミング]: http://www.atmarkit.co.jp/ait/articles/0812/17/news149_3.html
    "Gaucheでメタプログラミング"
  [特定のディレクトリ配下のディレクトリをload-pathに追加する]: http://qiita.com/icb54615/items/4c652ad4afccae5fe2ef
    "特定のディレクトリ配下のディレクトリをload-pathに追加する"
