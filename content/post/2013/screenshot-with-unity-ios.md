+++
title = "Unityでスクリーンショットを撮影し、iPhoneのカメラロールに保存する方法。"
date = "2013-09-06"
+++

Unityでスクリーンショットを撮るのは非常に簡単である。

[Application.CaptureScreenshot][]という関数が用意されているからだ。

だが、この方法ではアプリのデータ領域に画像が保存されるだけなので、ユーザーが、その画像を閲覧できるようにはならない。アプリ内で画像を管理できるようにしてもいいかもしれないが、スクリーンショットを確認したいだけなのであれば、そこまでする必要はないだろう。

そこで今回は、UnityからiOSのカメラロールにアクセスする方法を紹介する。

```objc
#import <Foundation/Foundation.h>
#import <AssetsLibrary/AssetsLibrary.h>
#import <AVFoundation/AVFoundation.h>

/* 
 スクリーンショット撮影時に利用するネイティブコード。
 */

// 指定したパスの画像をカメラロールに保存する。
extern "C" void _WriteImageToAlbum (const char* path)
{
    UIImage *image = [UIImage imageWithContentsOfFile:[NSString stringWithUTF8String:path]];
    ALAssetsLibrary *library = [[[ALAssetsLibrary alloc] init] autorelease];
    NSMutableDictionary *metadata = [[[NSMutableDictionary alloc] init] autorelease];
    [library writeImageToSavedPhotosAlbum:image.CGImage metadata:metadata completionBlock:^(NSURL *assetURL, NSError *error) {
        // 書き込み終了後、Unity側へコールバック。
        UnitySendMessage("CaptureScreenShot", "DidImageWriteToAlbum", [error.description UTF8String]);
    }];
}

// システムのシャッター音を鳴らす。
extern "C" void _PlaySystemShutterSound ()
{
    // NOTE:
    //      マナーモードや本体音量に左右されずに鳴る。
    AudioServicesPlaySystemSound(1108);
}
```

このネイティブコードをファイルの拡張子を.mm(Objective-C++の拡張子)にしてStreamingAssets内に配置しておけば、そのままXcodeのプロジェクトに追加される。

さて、Unityのスクリプトから、このファイルの関数を呼び出すわけだが、writeImageToSavedPhotosAlbumでの書き込みは非同期で行われるので、Unity側へコールバックを返したい。そこで、Unity側の関数を呼び出すことができる、[UnitySendMessage][]を利用する。UnitySendMessageは第1引数にオブジェクト名、第2引数に関数名、第3引数に呼び出す関数の引数をとる。

また、iOS標準のスクリーンショット機能のように、シャッター音が鳴るようにしたいので、システムのシャッター音を鳴らす関数(\_PlaySystemShutterSound)を作成した。おそらく、カメラからの入力がある状態でスクリーンショットを撮られることを考慮してシャッター音がなるようにしているのであろう。最近はAR等でカメラを使うことも多く、その場合はキャプチャ時にシャッター音が実装されていなければ高確率でリジェクトされるであろう。

次は、スクリプト側の実装だ。

```csharp
using UnityEngine;
using System;
using System.Collections;
using System.Runtime.InteropServices;

public class CaptureScreenshot : MonoBehaviour {

     const string ScreenshotFilename = "src.png";

#if  UNITY_IPHONE && !UNITY_EDITOR
     [DllImport("__Internal")]
     private static extern void _PlaySystemShutterSound ();
     [DllImport("__Internal")]
     private static extern void _WriteImageToAlbum (string path);

     public void CaptureScreenShot () {
          // ネイティブコードからシャッター音を再生。マナーモード時や、ボリュームオフ時もシャッター音を再生したいため。
          _PlaySystemShutterSound ();
          // スクリーンショットを撮影。Documents下に保存される。
          Application.CaptureScreenshot(temporaryScreenshotFilename);
          // スクリーンショットが書き込まれるまで待つ。書き込み完了後、画像をカメラロールへ保存する。
          StartCoroutine(WaitUntilFinishedWriting (()=>{ _WriteImageToAlbum (TemporaryScreenshotPath());}));
     }

     // スクリーンショットの画像が一時的に保存されるパス。
     string TemporaryScreenshotPath () {
          return Application.persistentDataPath + "/" + temporaryScreenshotFilename;
     }

     // スクリーンショットの書き込みが終了するまで、毎フレームファイルの有無を確認する。
     IEnumerator WaitUntilFinishedWriting (Action callback) {
          while (!System.IO.File.Exists (TemporaryScreenshotPath())) {
                Debug.Log(">>>>> Temporary Screenshot have not been written yet.");
               yield return null;
          }
          Debug.Log(">>>> Temporary Screenshot have been Written.");
          callback();
          yield break;
     }

     // カメラロール保存後、ネイティブ側から呼び出される。
     void DidImageWriteToAlbum (string errorDescription) {
          if (string.IsNullOrEmpty(errorDescription)) {
               Debug.Log(">>>>> Image have been Written To Album Successfully.");
               Debug.Log(">>>>> Delete Temporary Screenshot.");
               System.IO.File.Delete (TemporaryScreenshotPath());
          } else {
               Debug.Log(">>>>> An Error Occured. Error Description is..." + errorDescription);
          }
     }
#else
     public void CaptureScreenShot () {
         // Android端末での処理等。
     }
#endif
}
```

こちらは少しややこしいので補足しておく。

```csharp
[DllImport("__Internal")]
private static extern void _PlaySystemShutterSound ();
[DllImport("__Internal")]
private static extern void _WriteImageToAlbum (string path);
```

DllImportを使い、呼び出されるネイティブコードのシグネチャを宣言する。

```csharp
IEnumerator WaitUntilFinishedWriting (Action callback) {
    while (!System.IO.File.Exists (TemporaryScreenshotPath())) {
        Debug.Log(">>>>> Temporary Screenshot have not been written　yet.");
    yield return null;
    }
    Debug.Log(">>>> Temporary Screenshot have been Written.");
    callback();
    yield break;
}
```

コルーチンを利用し、画像がディスクに書き込まれるまで、毎フレームファイルの有無をチェックし、画像の存在が確認でき次第コールバック(ここではカメラロールへの保存)を実行する。

```csharp
void DidImageWriteToAlbum (string errorDescription) {
    if (string.IsNullOrEmpty(errorDescription)) {
        Debug.Log(">>>>> Image have been Written To Album Successfully.");
        Debug.Log(">>>>> Delete Temporary Screenshot.");
        System.IO.File.Delete (TemporaryScreenshotPath());
    } else {
        Debug.Log(">>>>> An Error Occured. Error Description is..." + errorDescription);
    }
}
```

こちらは、カメラロール保存後に前述のUnitySendMessageによって呼び出される関数だ。[Application.CaptureScreenshot][]は画像をDocuments内に保存するが、[一時ファイルをDocuments内に置いてはいけないことになっている][]ので、すぐに削除している。

実装するコードは以上である。

また、AssetsLibraryを利用しているため、フレームワークを追加する必要がある。Xcodeから手動で追加しても良いし、[PostProcessBuildPlayer][]にフレームワークを追加するコードを記述しても良い。このあたりはネット上で割りと簡単に情報が見つかるのでそちらを参考にしてほしい。

  [Application.CaptureScreenshot]: http://docs.unity3d.com/Documentation/ScriptReference/Application.CaptureScreenshot.html
  [UnitySendMessage]: docs.unity3d.com/Documentation/Manual/PluginsForIOS.html
  [一時ファイルをDocuments内に置いてはいけないことになっている]: https://developer.apple.com/jp/devcenter/ios/library/documentation/FileSystemProgrammingGuide.pdf
  [PostProcessBuildPlayer]: http://docs.unity3d.com/Documentation/Manual/BuildPlayerPipeline.html
