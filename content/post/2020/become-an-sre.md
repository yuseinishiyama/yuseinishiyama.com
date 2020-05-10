---
title: "iOSからSREに転向した"
date: 2020-05-10T03:55:00+02:00
---

昨年7月にiOS Engineerとして[SoundCloudに入社した]({{< ref "join-soundcloud.md" >}})が、今年の4月からは同社でSREをしている。

## モバイルエンジニアのキャリア

Software Engineerになって約8年、基本的にはずっとiOS Developerを名乗ってきた。そこそこ真面目にやった甲斐もあり、[国内外のカンファレンスで登壇したり](https://academy.realm.io/posts/yusei-nishiyama-mobilization-2017-building-ios-apps-at-scale/)、[書籍を執筆したり](https://www.amazon.co.jp/dp/B086VVQ3DJ)、[それなりの規模のプロジェクトでTech Leadをする](https://staffblog.cookpad.com/entry/2018/03/14/152346)機会に恵まれた。

その一方で、長らく水平方向にスキルを伸ばしたいという思惑があり、[伸びそうなSRE・ML・セキュリティ](https://www.oreilly.com/radar/oreilly-2020-platform-analysis/)といった分野を検討していた。その中でも特にSREがしっくりきたので、少し前からインフラ関連技術のキャッチアップをはじめていた。

{{< tweet 1212766180471164931 >}}

## 転職、フルスタック化

現職にはiOS Engineerとして入社したものの、入ってみるとMicroservice・権限移譲が割と進んでいて、フルスタック的な働き方もできそうだった。そこで、バックエンド（主にScala）の開発、オンコール、データパイプラインを触ったりと好き勝手やってみた。ちなみに、このあたりのことは[社内のブログにも書いている](https://developers.soundcloud.com/blog/a-happy-new-employee)。

楽しくやっていたものの、なんでも屋でいるとManagerに対する期待値の調整が難しいし、何をやってもちょっとずつ居心地が悪い。そんな折、偶然SREのポジションが空いたので応募してみたところ運良く受け入れてもらえたわけである。

社内異動のプロセスがオープンなのは現職の良いところだ。社外に対して募集をかけているポジションの多くは社内からも応募可能である。具体的なプロセスはチームによって異なるが、今回のケースでは簡易的なホワイトボードの面接があった。

## SREになって

モバイルとはかなり距離のある分野なので、毎日新卒のような気持ちで働いている。とはいえ、探せば出来ることはあって、Kubernetes（Self-managed）のアップグレード、新しいマシンイメージの適用、Cassandraのメトリクスを取得するためのJobとアラートの実装など、この一ヶ月で雑多なことをやった結果、いくらか土地勘がついた。

## 役に立ったもの

モバイルエンジニアがSREになるケースはやや珍しいと思うので、重宝したリソースをいくつか挙げてみる。

### [Site Reliability Engineering](https://www.oreilly.com/library/view/site-reliability-engineering/9781491929117/)（通称SRE本）

とりあえず語彙を揃えるためにも。概念中心かと思ったが、後半のLoad Balancingの章などは結構具体的だった。

### [Designing Data-Intensive Applications](https://www.oreilly.com/library/view/designing-data-intensive-applications/9781491903063/)

これはマジで良い。そこそこモダンな分散システムを触るならその日から使える知識の宝庫。

### [AWS Solutions Architect - Associate](https://aws.amazon.com/certification/certified-solutions-architect-associate/)

前職で流行っていたので、「通信空手」などと揶揄されながらも取得してみたが、おかげで抵抗なくコンソールが触れるし、チームメンバーが話している内容の文脈も理解できて、結構役に立っている。

### ネットワークの基礎

モバイルエンジニアだと、L7がギリギリ、そこから下とDNSはほぼ無という人もいるのではないだろうか（自分はそうだった）。[『ネットワークはなぜつながるのか』](https://www.amazon.co.jp/dp/4822283119)ぐらいの簡単なものでも目を通しておくとだいぶマシになる気がする。

### 社内のRunbook、Postmortemの過去ログ

なんだかんだで社内固有のオペレーションをすることが多いと思うので、眺めておくと良いイメトレになる。

### Kubernetes関連

最初はかなり混乱したが、有名な[Kubernetes The Hard Way](https://github.com/kelseyhightower/kubernetes-the-hard-way)をやって、その後もう少し親切な説明を見たらなんとなく分かってきた。[Carson Andersonという人の解説](https://vimeo.com/245778144/4d1d597c5e)が良かった。

## おわりに

シニアのモバイルエンジニアにとって、今はキャリア的に難しい局面にあると思う。技術の流行り廃りが激しい中、エンジニアが新しい分野に取り組む心理的安全性を担保するような仕組みが組織には必要かもしれない。それと、よりオープンで汎用的な技術で開発できるように、Appleには一層頑張って欲しい。
