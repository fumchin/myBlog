---
layout: single
title: TrafficPredict - Trajectory Prediction for Heterogeneous Traffic-Agents [論文筆記]
excerpt: Yuexin Ma1;2, Xinge Zhu3, Sibo Zhang1, Ruigang Yang1, Wenping Wang2, Dinesh Manocha
categories:
    - Paper
author_profile: true
classes: wide
---

## Reference
論文連結：[TrafficPredict: Trajectory Prediction for Heterogeneous Traffic-Agents](https://arxiv.org/pdf/1811.02146.pdf)<br>
（在讀這篇之前最好先讀[Social Attention: Modeling Attention in Human Crowd](https://arxiv.org/pdf/1710.04689.pdf)，TrafficPredict裡面有很大一部分是從Social Attention來的。）

## Introduction
這篇聚焦在在Heterogeneous（包含不同的agents, including vehicles, pedestrians, bikes ...）上。<br>
> Main Results：
> 1. a novel LSTM-based algorithm (new approach for trajectory prediction in heteogeneous traffic) -> **4D GRAPH**
> 2. collect a  new dataset
> 3. smaller prediction error

## System Overview
![system_overview](https://raw.githubusercontent.com/fumchin/myblog/master/assets/images/post_images/papers/TrafficPredict/system_overview.png)
簡單來說勒，這篇paper主打的就是他的4D GRAPH，雖然聽起來很厲害，但其實就是4個不一樣的features而已拉，跟維度沒啥關聯。
主要分成兩個Layer：
1. Instance Layer：抓出（capture）instances之間的動態、互動關係
2. Category Layer：歸納（conclude）類似instances的移動方式（ex:車子的移動方式、行人的移動方式）

> **Q：等等 那到底4D是在4D啥？**  
> A：4D就是上面那個圖裡面的4個東東：  
> * 在Instance Layers中的每個instance（1st D）, instance相連的spatial edge棕色線線，在空間中的關聯性（2nd D）。
> * 在Category Layers中的6角形東東（high-level categorization，同一纇instance的移動特性）（3rd D）。
> * frame（n） 跟 frame（n+1）之間的temperal edge虛線，時域的關聯性（4th D）。

看起來很厲害，但其實就是4種不一樣的時間、空間features而已～～～  
這只是基本的部份而已，接下來還有整個的Architecture要討論。

## Related Works
主要就來講一下Social Attention和Attention相關的東西。
1. Social Attention（SA）
    * 先來看個Social Attention（RNN-based, Homogeneous）的架構圖><，是不是跟我們的4D GRAPH超級像阿（應該有像ㄅ），只是SA沒有Category Layer的分類而已。
    ![SocialAttentionSysytemOverview](https://raw.githubusercontent.com/fumchin/myblog/master/assets/images/post_images/papers/TrafficPredict/SA_system_overview.png)

2. Attention（注意力）
    * 訓練讓model知道"誰是重要的"，利用公式算出一個分數，越高分則代表越重要。
    * ex：在路上騎車時，正前方的車輛就很重要，因為他可能隨時會煞車、右轉等等，所以需要高注意力（attention分數高）；相反的，50公尺外的行人可能就不是那麼重要（attention分數低）。簡單來說Attention就是在訓練這些東西，讓系統知道誰是該注意的東西。

## Method
### Problem Definition
尷尬 有點懶得打

### How?（照順序來><）
> 先讓我們來稿清楚一下倒底用了什麼樣的LSTM
> 1. instance LSTM（每個instance會有一個LSTM來紀錄他們的移動特性，相同種類的會share parameters，論文中提到在他們的實驗裡有3種）
> 2. edge LSTM（找出在同一時間，instance之間的interaction）
> 3. super node temporal LSTM
> 4. super node LSTM

#### 1. Instance Layer
>capture the movement pattern of instances in traffic

* 利用edge LSTM抓出instance i和j的互動關係 -> （hij）
* 再來利用Attention打分數，以（hij）的結果，找出現在誰對i的重要性最大 ->（w） 
* 將（w）和上一階段的結果（h2）代入istance LSTM找出第一階段的結果 -> （h1, c）
* 圖  
![instance_layer](https://raw.githubusercontent.com/fumchin/myblog/master/assets/images/post_images/papers/TrafficPredict/instance_layer.png)

#### 2. Category Layer
>learn the movemet from the same category of instance

* 把相同類別的instance在第一階段（instance layer）算出來的cell state（c）取出平均，以此作為該類別的移動法則（internal movement law of the trajectory） -> （F）
* 以（F）通過super node temporal LSTM計算super node時域間的特性 -> （hii）
* 以（F）、（hii）通過super node LSTM計算super node的特性 -> （hu）
* 最後以（h1）和（hu）找出最後的解答 -> （h2）
* （h2）代回instance layer作為強化
* 看圖清楚多ㄌ
![category_layer](https://raw.githubusercontent.com/fumchin/myblog/master/assets/images/post_images/papers/TrafficPredict/category_layer.png)


### 3. Position Estimation
>bivariate Gaussian distribution 就高斯分佈這樣

#### 4. Loss Function
![loss_function]