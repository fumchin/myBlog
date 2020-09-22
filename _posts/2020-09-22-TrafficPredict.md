---
layout: single
title: TrafficPredict - Trajectory Prediction for Heterogeneous Traffic-Agents
excerpt: Yuexin Ma1;2, Xinge Zhu3, Sibo Zhang1, Ruigang Yang1, Wenping Wang2, Dinesh Manocha
categories:
    - Paper
author_profile: true
classes: wide
---

## 論文連結
[TrafficPredict: Trajectory Prediction for Heterogeneous Traffic-Agents](https://arxiv.org/pdf/1811.02146.pdf)<br>
（在讀這篇之前最好先讀[Social Attention: Modeling Attention in Human Crowd](https://arxiv.org/pdf/1710.04689.pdf)，TrafficPredict裡面有很大一部分是從Social Attention來的。）

## Introduction
這篇聚焦在在Heterogeneous（包含不同的agents, including vehicles, pedestrians, bikes ...）上。<br>
Main Results：
1. a novel LSTM-based algorithm (new approach for trajectory prediction in heteogeneous traffic) -> **4D GRAPH**
2. collect a  new dataset
3. smaller prediction error

## System Overview
![system_overview](https://raw.githubusercontent.com/fumchin/myblog/master/assets/images/post_images/papers/TrafficPredict/system_overview.png)
簡單來說勒，這篇paper主打的就是他的4D GRAPH，雖然聽起來很厲害，但其實就是4個不一樣的features而已拉，跟維度沒啥關聯。
主要分成兩個Layer：
1. Instance Layer：抓出（capture）instances之間的動態、互動關係
2. Category Layer：歸納（conclude）類似instances的移動方式（ex:車子的移動方式、行人的移動方式）

**Q：等等 那到底4D是在4D啥？**  
A：4D就是上面那個圖裡面的4個東東：  
* 在Instance Layers中的每個instance（1st D）, instance相連的spatial edge棕色線線，在空間中的關聯性（2nd D）。
* 在Category Layers中的6角形東東（high-level categorization，同一纇instance的移動特性）（3rd D）。
* frame（n） 跟 frame（n+1）之間的temperal edge虛線，時域的關聯性（4th D）。

看起來很厲害，但其實就是4種不一樣的時間、空間features而已～～～

## Related Works
主要就來講一下Social Attention和Attention相關的東西。
1. Social Attention
    * 先來看個Social Attention的架構圖><，是不是跟我們的4D GRAPH超級像阿
    ![SocialAttentionSysytemOverview]()
