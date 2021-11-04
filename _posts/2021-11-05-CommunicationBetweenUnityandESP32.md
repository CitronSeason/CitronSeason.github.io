---
title: UnityとESP32の間で通信する方法を考える
date: 2021-11-25　2:00：00
categories:
- 研究メモ
- Unity
- ESP32
tags:
- Unity
- ESP32
- Bluetooth
---

今日の記事はほぼ備忘録

現在行っている研究で、ESP32の先にサーボモータが備え付けられている。  
Unityの空間上でシミュレートした結果をもとに、ESP32へ情報を送りサーボなどのアクチュエータを制御したい。
ESP32を搭載する装置の可搬性や簡易性からPCとESP32の間は無線通信を想定しているが、その通信方法について検討する。

<!-- more -->

まず、ESP32にはESP32-WROVER-Eの評価基板[ESP32-DevKitC-VE(8MB)](https://akizukidenshi.com/catalog/g/gM-15674/)を使用している。  
これは、無線規格としてIEEE 802.11b/g/n(2.4GHz)、Bluetooth V4.2 BR/EDR BLEを採用している。(WiFi 5GHzは使用できない点に注意)  
したがって、想定できる無線通信としては

- TCP(via WiFi)
- WebSocket(via WiFi)
- UDP(via WiFi)
- OSC(via WiFi)
- Serial communication(via Bluetooth)
- BLE(via Bluetooth)

があるだろう。私の研究では
- 低遅延
- 同時多接続（とそれに伴うソフトウェア上での機器情報の管理の簡易さ）

が求められそれに適合する規格を選ぶ。  
そうした中で、注目したのはSrial communicationとOSCである。  

TCPはUDPに比べて、伝送遅延がある。  
WebSocketはTCPの上位層、NAT超えや双方向通信などで魅力的ではあるが、開発工数を鑑み今回は不適当。  
UDPは低遅延で行えるが、後述のOSCに比べてメッセージ送受信のためのプログラムの開発が必要。   
OSCはUDPを使用したリアルタイム性の高いメッセージ送受信が行える。  
Serial communicationは事前にペアリングが必要。
BLEは省電力であるが、同時多接続通信には向かない。  

via WiFiは環境にWiFiルーターが必要で可用性に劣ることとESP32がネットワークに接続されるためセキュリティ対策を考慮する必要が発生することなどから、via BluetoothのSerial communicationを使用することにした。
複数の子機の接続管理を考えたとき、少ない機器情報の管理で済む。