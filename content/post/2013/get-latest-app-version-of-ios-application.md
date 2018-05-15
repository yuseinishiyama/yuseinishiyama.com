+++
title = "iOSでアプリの最新バージョンを取得する方法。"
date = "2013-09-16"
+++

開発者であれば、ユーザーには自分の開発したアプリの最新版を使ってほしいと思うだろう。また、ユーザーのアプリバージョンにばらつきがないことは、ビジネス面のメリットも大きい。仕事として開発を行っている人であれば、「アプリのアップデートが行われた場合に通知する機能」の実装を要求されたことは多々あるだろう。

そこで今回は、

アプリの最新版を取得し、ユーザーが利用しているアプリが最新版より古ければ通知する機能

の実装方法を紹介する。

さて、まず考えなくてはいけないのは、どこからアプリの最新版を取得するか、ということである。これにはおおまかに言って、2通りの方法がある。「1.自前のサーバを利用する方法」、と「2.AppleのAPIを利用する方法」の2つだ。どちらが適切な方法であるか比較するために、それぞれのメリット、デメリットを挙げてみた。

1.自前のサーバを利用する方法

メリット

-   テストが容易。
-   更新のタイミングを自由に決めることができる。
-   取得先が勝手に変更されることがない。

デメリット

-   運用者が必要。
-   運用ミスが発生する可能性がある。

2.AppleのAPIを利用する方法。

メリット

-   自分で運用しなくて良い。
-   運用ミスが発生しない。

デメリット

-   テストが行えない。
-   APIが変更された場合、機能が無効になる。
-   APIから取得できるバージョンと、ストアへ反映されているバージョンとの間にタイムラグがあったりなどした場合、その間はどうすることもできない。

結論から言うと、「AppleのAPIを使ったほうが楽だが、その場合はなにか問題が発生してもどうすることもできない」ということになる。今回は、個人開発者や、規模の小さなアプリで、サーバを設置するのはコストに見合わない場合を想定し、「2.AppleのAPIを利用する方法」の実装を紹介することにした。

```objc
#import <Foundation/Foundation.h>

@interface UpdateChecker : NSObject

// 利用中のバージョンより、新しいバージョンのアプリがストアに公開されている場合に、
// ダイアログを表示する。
+ (void)showNeedUpdateAlertIfNeeded;

@end
```

 

```objc
#import "UpdateChecker.h"

#import "ApplicationInformation.h"

// アプリのID。
static NSString * const kAppStoreID = @"123456789";

@implementation UpdateChecker

+ (void)showNeedUpdateAlertIfNeeded
{
    [self getLatestAppVersionAsynchronousWithCompletionBlock:^(NSString *latestAppVersion) {
        // 現行のアプリバージョンが、最新のアプリバージョンよりも古い場合(NSNumericSearchでバージョン番号での比較が可能)、
        if ([latestAppVersion compare:[self applicationVersion] options:NSNumericSearch] == NSOrderedDescending) {
            static BOOL isAlreadyShow = NO;
            // 通知中でなければ、
            if (!isAlreadyShow) {
                isAlreadyShow = YES;
                // メインスレッドで通知を実行する。
                dispatch_async(dispatch_get_main_queue(), ^{
                    // ダイアログを表示するなど、通知の処理をここに記述。
                });
            }
        }
    }];
}

// アプリの最新バージョンをAppStoreから非同期で取得する。
+ (void)getLatestAppVersionAsynchronousWithCompletionBlock:(void(^)(NSString *))completionBlock
{
    NSURLRequest *request = [NSURLRequest requestWithURL:[NSURL URLWithString:[NSString stringWithFormat:@"http://itunes.apple.com/lookup?id=%@", kAppStoreID]]
                                             cachePolicy:NSURLRequestReloadIgnoringLocalCacheData // キャッシュしない
                                         timeoutInterval:20.0];
    [NSURLConnection sendAsynchronousRequest:request
                                       queue:[[NSOperationQueue alloc] init]
                           completionHandler:^(NSURLResponse *response, NSData *data, NSError *error) {
                               if (data) {
                                   NSDictionary *versionSummary = [NSJSONSerialization JSONObjectWithData:data
                                                                                                  options:NSJSONReadingAllowFragments
                                                                                                    error:&error];
                                   NSDictionary *results = [[versionSummary objectForKey:@"results"] objectAtIndex:0];
                                   NSString *latestVersion = [results objectForKey:@"version"];
                                   NSLog(@">>>>> Latest App Version is %@.", latestVersion);
                                   completionBlock(latestVersion);
                               } else {
                                   NSLog(@">>>>> Fail to Get Latest App Version.");
                               }
                           }];
}

// 利用中のアプリのバージョンを取得する。
+ (NSString *)applicationVersion {
    return  [[[NSBundle mainBundle] infoDictionary] objectForKey:@"CFBundleShortVersionString"];
}

@end
```

取得するタイミングは悩ましいところではあるが、例えばアプリがフォアグラウンドになる度に取得する場合は、

```objc
- (void)applicationDidBecomeActive:(UIApplication *)application
{
    // バージョンチェック。
    [UpdateChecker showNeedUpdateAlertIfNeeded];
}
```

AppDelegate内のコールバックから呼び出すことになる。

上述の通り、この確認方法は確実ではないので、アプリの最新版の通知がクリティカルな機能である場合は利用しないほうが良いかもしれない。
