---
title: UnityProjectをOculusQuest2に対応させる
date: 2021-08-17 00:34
categories:
- Unity
tags: 
- Unity
- Oculus Quest2
---


Unity 2021.1.16f1において、既存のプロジェクトをOculus Quest2で実行できるよう環境を変更する。  
既存のStandaloneプロジェクト(Windowsアプリケーション)も実行できるはず。  

<!-- more -->


## 前提環境  
まず、前提環境として  

必要なハードウェア
- [Oculus Quest2](https://amzn.to/3ySPwYC)
- [Oculus Link ケーブル](https://amzn.to/3yTEi5Y) (Oculus link仕様を満たしていればリンク先の製品である必要はない)
- 3DCGゲームをビルドできる程度にちゃんとつよいPC  

必要なソフトウェア
- [Oculus Link](https://www.oculus.com/setup/?locale=ja_JP)  
- [Oculus Developer Hub](https://developer.oculus.com/downloads/package/oculus-developer-hub-win/)  

ちなみに、Oculus Linkは導入に15GBくらい必要で、Cドライブに空き容量がなかったため、Dドライブに[インストールする処置](https://support.oculus.com/articles/getting-started/getting-started-with-rift/install-oculus-pc-app-different-drive/)をとった。  

## Unityにモジュールを追加  
使用するUnityのバージョンにAndroid Build Support モジュールが導入されていないならば導入する必要がある。  
Unity Hubを開き、インストールタブより使用しているUnityバージョンの︙からモジュールを加えるを選択。Android Build Support (サブモジュールのAndroid SDK & NDK Tools, OpenJDKのチェック必須)をチェック、で追加する。  

> 場合によってはあとからAndroidモジュールを導入するとUnity Editorにきちんと関連付けられない事がある。その時は、Unity EditorのEdit > Preferences... > External Toolsの項目を確認する。各Toolをアサインしなおす必要があるかもしれない。


## Unity Editor上での環境構築  
Unity Hubから該当プロジェクトを開き、Unity Editorが開かれたら、  
- File > Build SettingsよりPlatformからAndroidを選択
- Texture Compression をASTCにしてSwitch Platform  
- Build SettingsにあるPlayer Settings...を選択 
- Project SettingsのPlayerが開かれたら、Other Settings > Color Space をLinearにする
- Auto Grapchics APIのチェックがついていたらチェックを外すらしい(私はついていなかった)
- Graphics APIsにVulkanとOpenGLES3があるのでどちらかを選択し、-を押してリストから一方を削除する。  
- ScriptingBackendをIL2CPP にする
- Project SettingsのPlayerタブからXR Plug-in Managementタブに移動
- Install XR Plugin Managementを選択してインストール
- XR Plug-in Management > Androidタブ及びStandaloneタブ > Plug-in ProvidersでOculusにチェック(Project Settingsは閉じてOK)  

以上で、Unity Editor上での環境構築は99%済んだ  

## Oculus Quest2をPCに接続する
PCのOculusを起動する。Oculus Quest2をOculus Linkケーブルで接続する。  
デバイスとして認識されるなり、登録作業なりを済ませたら、Oculus Quest2側で開発者モードにする必要がある。(そこの手順は忘れてしまった。ググればでてくる)  
また、もしADB over Wi-FiがオンになってたらDisabledにする、これはOculus Developer Hubで可能。  
必要な設定が済んだら、Oculus Developer Hubは終了させる。(Oculus Developer HubのADBがUnity Editor上のADBと鑑賞するため。)  

## Unity Editor上で実行する&ビルドしてapkをOculus Quest2上に配置する
Quest2では設定よりOculus Linkを開くことができるようになっている。開いておく。
UnityのADBでOculus Quest2に自動的に接続される。Quest2側にUSBデバッグを有効にする?てきなダイアログが出るので、有効にする。  
Unity Editorで再びBuild Settingsを開く。 UnityとQuest2の接続が成功していれば、Android > Run DeviceにてQuest2が選択できる  

PlayボタンでUnity Editor上で実行すると、Oculus Link上で再生される。
Build SettingsからBuildを押してapk をQuest2上に配置することもできる。これはOculus Linkを起動しておく必要はない。PC上にapkを生成して、Oculus Developer Hubを使用してQuest2上に配置しても良い。  
