+++
title = "iPhoneでの同時動画再生数の上限について"
date = "2013-06-13"
+++

現在動画編集アプリを作っている。  
動画にエフェクトとかBGMとかなんやらの設定をしていくわけだが、  

画面上部にプレビュー画面を持ち、その下に編集用のOutletがいくつか存在するような見た目のViewControllerがNavigationControllerでいくつか続いていくというような作りだ。

ここで奇妙な問題が発生した。  
MPMoviePlayerControllerを使用して、動画を再生しようとしたところ

```objc
Error Domain=AVFoundationErrorDomain Code=-11839 "Cannot Decode"
```

こんなエラーがでた。デコードできない？動画の形式が悪いのだろうか？  

だが、試しにそのViewControllerだけにして実行してみたところ問題なく再生できた。  
ということは、特定のコンテキストにおいてのみデコードが失敗するわけだ。

[先日の記事][]にもある通り、いざ実装を始めると全く予期しないところで躓くことが多々ある。  

特にこういうロジックの問題でない箇所は原因の特定が難しく、心が折れそうになる。

とにかくググるしかないので、上記のログをそのまま検索バーに貼り付けた。  
すると同じ様な現象で困っている人を発見することが出来た。

[http://stackoverflow.com/questions/8608570/avplayeritem-fails-with-avstatusfailed-and-error-code-cannot-decode][]

> There is a limit on the number of concurrent video players that
> AVFoundation will allow. It is due to the limitations of iOS hardware.
> The limit for current devices is 4 players. If you create a 5th
> player, you will get the "cannot decode" error. It is not a limit on
> the number of instances of AVPlayer, or AVPlayerItem. Rather,it is the
> association of AVPlayerItem with an AVPlayer which creates a "render
> pipeline", and you are limited to 4 of these. For example, this causes
> a new render pipeline:

どうやら、「AVFoundationで同時に実行できるプレイヤーの数はハードウェアによって制限がある」らしい。

> What I eventually found is that the AVPlayers were not being released
> when I had thought they were. In my case I was pushing my AVPlayer
> View Controller onto a Navigation Controller. Even though I was only
> creating one AVPlayer instance at a time, when the View Controllers
> are popped off a nav controller they were not being released
> immediately. It was then very easy for me to reach 4 AVPlayer
> instances before the old View Controllers were cleaned up.

確かに、自分の場合はMPMoviePlayerControllerでエラーが出たが、それ以前に複数のAVPlayerを使用していて、それらはNavigationControllerに残ったままだ。AVPlayerのインスタンスを開放しろとのことだが、試しに以下のコードを画面遷移時に実行してみた。

```objc
[_player replaceCurrentItemWithPlayerItem:nil];
```

なんと、これだけで治った。

というわけで、AVPlayerやMPMoviePlayerController(MPMoviePlayerViewControllerも)なんかを複数使うときは、AVPlayerItemのインスタンスが残ったままにならないように注意しよう。

  [先日の記事]: http://yuseinishiyama.com/archives/51
  
  [http://stackoverflow.com/questions/8608570/avplayeritem-fails-with-avstatusfailed-and-error-code-cannot-decode]: http://stackoverflow.com/questions/8608570/avplayeritem-fails-with-avstatusfailed-and-error-code-cannot-decode
