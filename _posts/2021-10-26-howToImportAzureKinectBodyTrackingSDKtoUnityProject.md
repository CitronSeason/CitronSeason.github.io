---
title: AzureKinectのBodyTrackingSDKをUnityプロジェクトに導入する方法
date: 2021-10-26　22:00：00
categories:
- 研究メモ
- Unity
tags:
- Unity
- Azure Kinect
- NuGet
---

マイクロソフトの[Azure-Kinect-Samples](https://github.com/microsoft/Azure-Kinect-Samples/tree/master/body-tracking-samples/sample_unity_bodytracking)のUnity bodytrackingのREAD ME をもとに既存のプロジェクトに導入する。READMEのとおりに行うと、サンプルプロジェクト上に構築されてしまうため、そうではなく自分のプロジェクトに導入する方法をまとめた。
ただし、手順中で使用するMoveLibraryFiles.batの一部が適切でないため失敗する。
そこで、README.mdをもとに執筆時点(2021/10/26)で正しい導入方法を記録する。
失敗した箇所は手動で移動する必要がある。


## NuGet PackageからのAzure Kinect SDKのダウンロード
NuGet Packageを利用してAzure Kinect使用に必要なSDKを導入する。  
Unityで適当なスクリプトを開きVisual Studioを起動する。(外部エディタにVSを設定していない場合は設定してから)  
ツール > NuGet パッケージマネージャー > ソリューションのNuGetパッケージの管理  
参照タブを開き、[azure kinect]と検索する。  
[Microsoft.Azure.Kinect.Sensor]を全てのプロジェクトにインストールする。
[Emit.Lightweight]と検索する。
[System.Reflection.Emit.Lightweigh]を全てのプロジェクトにインストールする。
他にもインストール済みタブを開いて、[packages.config](https://github.com/microsoft/Azure-Kinect-Samples/blob/master/body-tracking-samples/sample_unity_bodytracking/packages.config)
上のパッケージがインストールされてないようだったら同様の手順で適宜インストール

※RTX30系はMicrosoft.Azure.Kinect.BodyTrackingを1.1.0以上でしか利用できない。
NuGet Packageでは1.0.1までしか配布されていないため、手動でダウンロードする。

## Azure Kinect Body Tracking SDKをダウンロード

[https://docs.microsoft.com/ja-jp/azure/kinect-dk/body-sdk-download](https://docs.microsoft.com/ja-jp/azure/kinect-dk/body-sdk-download)  
から最新のSDKをダウンロードする。ここのあたりはREADME.mdに従って行う。  

自分のUnityプロジェクトのルート(Assetsフォルダと同じ階層)にMoveLibraryFiles.batファイルを作成し、  
[https://github.com/microsoft/Azure-Kinect-Samples/blob/master/body-tracking-samples/sample_unity_bodytracking/MoveLibraryFiles.bat](https://github.com/microsoft/Azure-Kinect-Samples/blob/master/body-tracking-samples/sample_unity_bodytracking/MoveLibraryFiles.bat)  
の内容をコピペする。


## MoveLibaryFiles
PowerShellを起動し、ルートディレクトリに移動したらMoveLibraryFilesを実行する。  
するとコピーできないファイルがいくつか現れるのでbatファイルを適宜修正して、再びバッチを実行する。  
例えば、
> copy packages\System.Reflection.Emit.Lightweight.4.6.0\lib\netstandard2.0\System.Reflection.Emit.Lightweight.dll Assets\Plugins
指定されたパスが見つかりません。

というエラーが出たが、これはインストールしたNuGetパッケージのバージョンとディレクトリ名が一致していないことからくる。  
私は、4.7.0をインストールしたため、バッチファイルの該当部分を4.6.0 -> 4.7.0に修正した。

また、次のようなエラーに対しては、
>copy "C:\Program Files\Azure Kinect Body Tracking SDK\tools\"cudnn64_cnn_infer64_8.dll .\
指定されたファイルが見つかりません。  
>copy "C:\Program Files\Azure Kinect Body Tracking SDK\tools\"cudnn64_ops_infer64_8.dll .\
指定されたファイルが見つかりません。

ファイル名がBodyTrackingSDKのバージョンアップによって変更されたことによる。
同じようにバッチの該当箇所を  

cudnn64_cnn_infer64_8.dll -> cudnn_cnn_infer64_8.dll  
cudnn64_ops_infer64_8.dll -> cudnn_ops_infer64_8.dll  

に変更する。

以上で開発は可能。

さらに、最終的にビルドする際は、README.mdに従い適切なファイルを実行ファイルと同じ階層に配置する必要がある。

