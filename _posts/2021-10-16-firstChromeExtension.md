---
title: chrome拡張機能の開発を始めてみる
date: 2021-10-16　17:00：００
categories:
- chrome拡張機能開発
---

約一ヶ月半ぶりのブログ更新になる。  
この一ヶ月半は、ポスター論文を2本投稿 -> 2本採用:smile:　->　査読対応&掲載用原稿準備を行っていた。  
そのため、大きく空いてしまった、学会発表まで一ヶ月あるためしばらくは多めに更新していきたい。  

さて、とあるchrome拡張機能を作りたくなったため、どのように開発すればよいのかを調べた。
<!-- more -->

調べると、開発に必要なフレームワーク等はなく、シンプルなものであれば簡単に作れるということがわかってきた。  
まず、必ず必要なものがmanifest.json
これは、拡張機能の名前や説明にとどまらず、chromeに許可してもらうAPI機能の定義やファイルの定義等に用いる。

chrome拡張機能の公式ドキュメントは [Chrome Developers](https://developer.chrome.com/docs/extensions/mv3/) にあるが、~~英語なので~~とりあえず入門系のサイトで簡単な拡張機能を作ってみた。

作成したファイルはmanifest.jsonとcontent.jsである。  

manifest.json
```json
{
    "name": "Sample",
    "version": "1.0.0",
    "manifest_version": 3,
    "permissions": ["activeTab","scripting"],
    "description": "Sample Chrome Extension",
    "background": {
        "service_worker": "src/content.js"
    },
    "action": {}
  }
```

content.js
```js
function showAlert(){
    alert("こんにちは！！！！！！！！");
}

chrome.action.onClicked.addListener((tab) => {
	chrome.scripting.executeScript({
		target: { tabId: tab.id },
		function: showAlert
	});
});
```

chrome右上から拡張機能のボタンを押すとアラートが出現するだけのプログラムである。
サイトは次の2つを参考にした。

- [Chrome拡張機能 を作ってみる](https://noitalog.tokyo/chrome-extensions/)
- [Chrome拡張機能の作り方。誰でもかんたんに開発できる！](https://original-game.com/how-to-make-chrome-extensions/)

つぎはポップアップでなにかウィンドウを表示させたい。