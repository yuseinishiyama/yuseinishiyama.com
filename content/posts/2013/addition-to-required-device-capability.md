+++
title = "UnityでiOS向けのビルド時に、RequiredDeviceCapabilityを追加する方法。"
date = "2013-09-07"
tags = ["Programming"]
+++

[RequiredDeviceCapability][]を正しく設定していないと、Appleにリジェクトされてしまうことは、広く知られている。

例えば、アプリに静止画を撮影する機能がある場合、このRequiredDeviceCapabilityに「still-camera」を追加してやらないといけない。

RequiredDeviceCapabilityは正しく設定されていなくても、ビルドに失敗しないし、Validationも通過する。だからこそ、リジェクトされてから気づくわけで、こういう作業は特に自動化するメリットが大きいだろう。

そこで、PostProcessBuildPlayerを利用し、Unityからのビルド時にInfo.plistを書き換えて、RequiredDeviceCapabilityを編集する関数を作成してみた。

    :::python
    def process_plist(plist_filepath):
      pl = plistlib.readPlist(plist_filepath)
	  new_settings = {"still-camera"}
	  if "UIRequiredDeviceCapabilities" in pl:
	    pl["UIRequiredDeviceCapabilities"].extend(new_settings)
	  else:
        pl["UIRequiredDeviceCapabilities"] = [new_settings]
	  plistlib.writePlist(pl, plist_filepath)

Info.plistのパスを与えてやると、Info.plistに「still-camera」を追記してくれる。ちなみに[plistlib][]を利用しているので、インポートしておく必要が有る(当たり前のように書いてはいるが、plistlibの存在は今回はじめて知った...)。

  [RequiredDeviceCapability]: https://developer.apple.com/library/ios/documentation/general/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html
  [plistlib]: http://docs.python.jp/2/library/plistlib.html
