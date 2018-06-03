+++
title = "advertisingIdentifierが原因のリジェクトに対応する。"
date = "2014-02-12"
tags = ["Programming"]
+++

# 概要

この2月からadvertisingIdentifierの利用方法が原因でリジェクトされるケースが大量に発生しているようだ。自分自身でadvertisingIDを利用するコードを書いている人はあまりいないであろうが、UDIDが利用できなくなった現在、効果測定を行うSDKの大半がトラッキングのためにこのIDを利用している。そして、このadvertisingIDの利用方法がリジェクトされてしまう原因であるようだ。

ちなみにリジェクトされるとこのようなメッセージがくる。

> PLA 3.3.12
> 
> We found your app uses the iOS Advertising Identifier but does not include ad functionality. This does not comply with the terms of the iOS Developer Program License Agreement, as required by the App Store Review Guidelines.
> 
> Specifically, section 3.3.12 of the iOS Developer Program License Agreement states:
> 
> "You and Your Applications (and any third party with whom you have contracted to serve advertising) may use the Advertising Identifier, and any information obtained through the use of the Advertising Identifier, only for the purpose of serving advertising. If a user resets the Advertising Identifier, then You agree not to combine, correlate, link or otherwise associate, either directly or indirectly, the prior Advertising Identifier and any derived information with the reset Advertising Identifier."
> 
> Please check your code - including any third-party libraries - to remove any instances of:
> 
> class: ASIdentifierManager
> selector: advertisingIdentifier
> framework: AdSupport.framework
> 
> If you are planning to incorporate ads in a future version, please remove the Advertising Identifier from your app until you have included ad functionality.
> 
> To help locate the Advertising Identifier, use the “nm” tool. For information on the “nm” tool, open a terminal window and enter, “man nm.”
> 
> If you do not have access to the libraries source, you may be able to search the compiled binary using the "strings" or "otool" command line tools. The "strings" tool lists the methods that the library calls, and "otool -ov" will list the Objective-C class structures and their defined methods. These techniques can help you narrow down where the problematic code resides.

どうやら広告出稿の目的以外でadvertisingIdentiferを利用してはいけないらしい。

# リジェクトされる可能性のあるケース

* Google Analytics SDKを利用している。
* Facebook SDKを利用している。
* Unity製アプリである。

等々、対象になるアプリは多そうだ。advertisingIdentifierが利用されているかを調べるには、対象のappファイル内で下記のようなコマンドを実行してみると良い。

```shell
strings アプリ名 |grep advertisingIdentifier
```
# 対応方法

## Facebook SDK

いまのところSDK側では対応されていないので、該当箇所をコメントアウトする。具体的には、FBUtility内のadvertiserIDメソッド内でadvertisingIdentifierが使用されている。

```objective-c
+ (NSString *)advertiserID {
    NSString *advertiserID = nil;
    Class ASIdentifierManagerClass = [FBDynamicFrameworkLoader loadClass:@"ASIdentifierManager" withFramework:@"AdSupport"];
    if ([ASIdentifierManagerClass class]) {
//        ASIdentifierManager *manager = [ASIdentifierManagerClass sharedManager];
//        advertiserID = [[manager advertisingIdentifier] UUIDString];
    }
    return advertiserID;
}
```

## Google Analytics SDK

[このスレッド](https://productforums.google.com/forum/#!msg/analytics/kmaotiQRwQs/LTyz2Z7kTacJ)のやりとりにあるように、
リンカのフラグを変更する必要がある。少々ややこしいので手順をまとめてみた。

* SDKを最新版(3.03a)に更新する。
* AdSupport.frameworkを削除する。
* "Target --> Build Settings --> Other Linker Flags"に下記の設定を追加する。引数が文字列であることに注意。(パスはSDKのオブジェクトファイルの位置にあわせて変更する。)

```shell
-force_load\ "${PROJECT_DIR}/Source/Library/GoogleAnalyticsServicesiOS_3.03a"
```

* -ObjCやall_loadのフラグを削除する。
* -ObjCが必要なStatic Libraryがある場合は、さらに下記の設定を追加する。

```shell
 -force_load $(TARGET_BUILD_DIR)/ライブラリファイル名.a
```

ちなみに、私の場合は最終的に下記のような設定になった
<script src="https://gist.github.com/yuseinishiyama/8860687.js"></script>

### 追記

Gistにアップしていた設定に不備があったため修正(14/02/13)。

## Unity

[この記事](http://qiita.com/monry/items/b473e3db7e48f05be96b#1-2)に詳しい。

# 結論

該当箇所を削除したり、不要なコードがリンクされないようにしたりして、advertisingIdentifierが使用されないようにするしかない。もし、SDKから取り除くことができなければ、SDKが対応するまでの間は、そのSDKを使用しない他は対応策がないだろう。

