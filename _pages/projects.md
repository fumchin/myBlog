---
title: "一些小作品"
layout: splash
sitemap: true
permalink: /projects/
author_profile: true
classes: wide

header:
  overlay_image: /assets/images/heart.jpg
  overlay_filter: 0.5
excerpt: 這些日子的心血 QQ

feature_row1:
  - image_path: /assets/images/project_images/music_player.jpg
    alt: "黑膠唱片"
    title: "Java Music Player"
    excerpt: 'Java 期末專題，可以讀wav檔並修改，包含速度、音量，有存檔的功能。<br>還有等化器(equalizer)、和弦辨識等功能。'
    url: "https://fumchin.github.io/Java_Wav_Signal_Processing/"
    btn_label: "Read More"
    btn_class: "btn--primary"

feature_row2:
  - image_path: /assets/images/project_images/jetbot_image.jpg
    alt: "jetbot"
    title: "Jetbot（大學部專題）"
    excerpt: '大學部專題主要是跟著實驗室的學長做自駕車Trajectory Prediction相關的研究。<br>上學期以讀paper比較多，下學期主要利用Nvidia出產的jetbot進行理論的實踐。'
    url: "https://drive.google.com/file/d/1W1Ld35zb7nSSqm9jlnsh6vTdDkYvlkuB/view?usp=sharing"
    btn_label: "影片連結"
    btn_class: "btn--primary"

feature_row3:
  - image_path: /assets/images/project_images/elevator.jpg
    alt: "elevator"
    title: "大二下進階物件導向專題"
    excerpt: '簡單來說就是設計一個電梯，會在不同樓層載人及放人，在此同時要解一些程式題目（CPE那類的）。<br>然後全班比賽，比誰題目解得比較快、誰的電梯演算法最好，最後好像是有到前十這樣吧。'
    url: "#test-link"
    btn_label: "Read More"
    btn_class: "btn--primary"

---
{% include feature_row id="feature_row1" type="left" %}
{% include feature_row id="feature_row2" type="right" %}
{% include feature_row id="feature_row3" type="left" %}

<!-- <figure class="half">
  <a href="http://google.com">
    <img src="https://raw.githubusercontent.com/fumchin/myblog/master/assets/images/project_images/music_player.jpg" width="50px" alt="music player" title="music player" >
  </a>
  <a href="http://google.com">
    <img src="https://raw.githubusercontent.com/fumchin/myblog/master/assets/images/project_images/elevator.jpg" width="50px" alt="music player" title="music player" >
  </a>
</figure> -->

