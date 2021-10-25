---
title: ScaniverseでスキャンしたモデルをUnityに取り込む方法
date: 2021-10-25　14:55：00
categories:
- 研究メモ
- Unity
tags:
- Unity
---

現実空間(部屋全体)をまるごとスキャンしてUnity上にデジタルツイン空間を生成する必要が出たため、その手法を調べた。  
ここでは、現実空間をスキャンする方法、データをPCに取り込む方法、Unityで取り込む際の設定を備忘録としてまとめる。  

<!-- more -->

## 現実空間をスキャンする方法
まず、iPad proのLiDARを使用して現実空間をスキャンするアプリの選定を行う。  

- メッシュスキャンができる(点群でない)  
- テクスチャ解像度が高い

という2つの理由と、次の2つのツイートを参考に、[Scaniverse](https://apps.apple.com/jp/app/scaniverse-lidar-3d-scanner/id1541433223)
を使用することを決めた。これは年額2000円の有料アプリだったが、ポケモンGOなどARゲームやプラットフォームを持つNiantic, inc.が買収し、無料となったことも理由の一つである。  

<blockquote class="twitter-tweet"><p lang="ja" dir="ltr">一泊旅行での3Dスキャンの楽しみ方をまとめてみました♪<a href="https://twitter.com/hashtag/Scaniverse?src=hash&amp;ref_src=twsrc%5Etfw">#Scaniverse</a> <a href="https://twitter.com/hashtag/WiDAR?src=hash&amp;ref_src=twsrc%5Etfw">#WiDAR</a> <a href="https://twitter.com/hashtag/Rememory?src=hash&amp;ref_src=twsrc%5Etfw">#Rememory</a> <a href="https://twitter.com/hashtag/Trnio?src=hash&amp;ref_src=twsrc%5Etfw">#Trnio</a> <a href="https://twitter.com/hashtag/LiDAR?src=hash&amp;ref_src=twsrc%5Etfw">#LiDAR</a> <a href="https://twitter.com/hashtag/3D%E3%82%B9%E3%82%AD%E3%83%A3%E3%83%B3?src=hash&amp;ref_src=twsrc%5Etfw">#3Dスキャン</a> <a href="https://twitter.com/hashtag/iPhone?src=hash&amp;ref_src=twsrc%5Etfw">#iPhone</a> <a href="https://t.co/YwvggzDPKg">pic.twitter.com/YwvggzDPKg</a></p>&mdash; 正露丸12 Pro (@seirogan_junkie) <a href="https://twitter.com/seirogan_junkie/status/1451563766722686980?ref_src=twsrc%5Etfw">October 22, 2021</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

<blockquote class="twitter-tweet"><p lang="ja" dir="ltr">このiPhone LiDARスキャンアプリまとめ画像が、日本のiPhone LiDAR界隈の標準だぞ<br>...と1000回ぐらい言い続ければ本当に標準になる！(ならない) <a href="https://t.co/jSG16XoEfk">pic.twitter.com/jSG16XoEfk</a></p>&mdash; iwama@無断で3Dスキャンイベントを応援する人 (@iwamah1) <a href="https://twitter.com/iwamah1/status/1420640233939357697?ref_src=twsrc%5Etfw">July 29, 2021</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

## データをPCに取り込む方法
Scaniverseは、多くのモデルの出力に対応している。

- FBX (Supported by most 3D software & game engines)
- OBJ (Text-based 3D model format)
- GLB (Open 3D format for the web (binary GLTF))
- USDZ (Best for ARKit and sharing to iPhones)
- STL (Stereolthography file)
- PLY (High-density color point cooud)
- LAS (Georeferenced color point cloud)

[Unityのドキュメント](https://docs.unity3d.com/ja/2018.4/Manual/3D-formats.html)によると、  
各種3Dオブジェクトファイルの中で、FBXが扱いやすいとわかった。   

ScaniverseはUIがわかりやすいため、使用方法は省く。Scanしたあと、アプリ内で適宜取り込む範囲をCropするといい。
Fileに保存して、Onedriveで共有するなり、メール添付で飛ばすなりしてPCにfbxが格納されているzipファイルを取り込む。  
私の場合、研究室一部屋をスキャンして20~30MB程度(最高解像度)だった。

## Unityへ取り込む方法
Unityプロジェクトを開いたらProjectに適切なフォルダを作成し、FBXファイルをドラッグ&ドロップで取り込める。  
しかし、そのままシーンに取り込むとテクスチャが反映されていない白いモデルが表示されてしまう。
そこで、fbxを選択した際のInspector - Materialsから[Textures: Extract Textures...]を選択する。
そして、fbxの格納されていたフォルダ自身を選択すると、fbxからテクスチャが抽出され、適用される。

これで、Unity上で現実のスケールと合わせたモデル表示が可能となる。  
ただし、マテリアルとDirectional Lightの関係でScaniverse上とモデルの見え方は異なる。適宜調節が必要。

Unity上で生成したTextureのTexture max sizeは16384 まで対応しているが、Scaniverseの出力が8192までしか対応していないのため、これ以上テクスチャ画質を上げることは出来ないと思われる。

