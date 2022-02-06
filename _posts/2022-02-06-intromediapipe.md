---
title: 論文メモ 2112~
date: 2021-02-06 22:30:00
categories:
- Tips
tags:
- Python
- MediaPipe
- TouchDesigner
---

なぜAnacondaでなくvenv+pyenvなのか

[https://messefor.hatenablog.com/entry/2020/08/22/181519](https://messefor.hatenablog.com/entry/2020/08/22/181519)

pyenvの導入

[https://www.3ryu-engineer.work/windows-pyenv/](https://www.3ryu-engineer.work/windows-pyenv/)

パスの追加に注意

すでに他のPythonが入っていると、パスの優先順位によってはpythonパスが適用されない

pyenvで導入するパスを上位にもってくる

venvの導入

python 3.3以上なら標準で入っている

[https://qiita.com/fiftystorm36/items/b2fd47cf32c7694adc2e](https://qiita.com/fiftystorm36/items/b2fd47cf32c7694adc2e)

mediapipeの導入

以上の内容を実行した上で

`pip install mediapipe`

TouchDesignerにPreferenceからPython 64-bit Module Pathにvenvで構築した環境のLibsのsite-packagesのパスを入れて適用する

mediapipeのインストールでmatplotlib3.5.1が入るが、TouchDesigner内のnumpy(1.16.2)では必要なバージョンに足りないため、matplotlibのバージョンを下げる

matplotlibのgitを見ると

[https://github.com/matplotlib/matplotlib/commit/aa500f5f2b901cdef8a5d38aae8792a294c6b348](https://github.com/matplotlib/matplotlib/commit/aa500f5f2b901cdef8a5d38aae8792a294c6b348)

ここでversionを1.16.0から1.17.3に上げてるので、2021.4.20以前のバージョンなら行けそう

[https://github.com/matplotlib/matplotlib/releases?page=1](https://github.com/matplotlib/matplotlib/releases?page=1)

からリリース時期を見るとver 3.4.1が良さそう、

**`pip install matplotlib==3.4.1`**

mediapipeで hand trackingをするときに参考にしたサイト

[https://google.github.io/mediapipe/solutions/hands.html](https://google.github.io/mediapipe/solutions/hands.html)

handのlandmarkのindexがわかる

おまけ

dir()関数を使うと、importしたモジュールの名前の定義リストが見れる

dir(numpy)など