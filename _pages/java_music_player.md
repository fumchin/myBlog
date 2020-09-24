---
title: "Java Music Player"
layout: splash
sitemap: true
permalink: /projects/java_music_player/
---
<br>

# Java Music Player

---

三下Java的期末專題，利用Java進行數位訊號的處理。<br>
[github連結](https://github.com/fumchin/Java-Music-Player)

## 簡介
就是用Java讀Wav檔案進行加速、減速、大小聲調整、即時預覽播放、存檔、等化器、和弦偵測。

## 作者
1. [陳舫慶](https://github.com/fumchin)
2. [許惟傑](https://github.com/nctu0513325)

## 功能＆原理ㄅ
1. ### 讀取Wav檔案
   * 利用buffer[]讀入指定數量的16進位（每個項目的長度不同），讀至data chunk結束為止。
   * CODE
   ```
   input = new FileInputStream(fileName);
   byte[] buffer_four = new byte[4];
   input.read(buffer_four);
   ```
   * 讀入signal data 轉成十進位並存在ArrayList<Double>[]中，得到時域離散訊號
   * 詳情請見[WavFile.java](https://github.com/fumchin/Java-Music-Player/blob/master/WavFile.java) <- 自認寫很好 吧（？）

2. ### 儲存音訊
   * 將ArrayList<Double>[]中的訊號存入Wav檔中。
   * CODE
    ```
    AudioFormat:            宣告你要儲存的參數（sampleRate, 聲道數等等）
    byte[] buffer:          將ArrayList中資料一次一次抓出來
    ByteArrayOutputStream:  將buffer讀出的byte塞進去（append）
    AudioInputStream:       格式參數連同資料一起塞進去
    AudioSystem.write:      輸入AudioInputStream且輸出wav檔
    ```

3. ### 播放音訊
    * ~~利用javafx 的media player讀Wave File，再播放出來><~~<br>
    ->無法撥放即時修改的sample，所以捨棄
    * 寫一個能**即時撥放**修改過的sample的播放功能
    * 類似儲存Wav檔，只是最後將東西（byte[] buffer）輸入SourceDataLine 即可有撥放的功能
    * CODE
    ```
    //設定參數
    AudioFormat audioFormat = new AudioFormat(WavFile.getSampleRate(), WavFile.getBitsPerSample(),WavFile.getNumChannels(), true, true);
    DataLine.Info info = new DataLine.Info(SourceDataLine.class, audioFormat);
    SourceDataLine soundLine = (SourceDataLine) AudioSystem.getLine(info);
    soundLine.open(audioFormat, bufferSize);
    //開始塞data進去
    soundLine.start();      
    ```

4. ### 簡易音訊編輯
    * 聲音放大縮小、加速減速 <- 直接對ArrayList<Double>[]做修改
    * 選取段落播放、剪下、刪除
    * CODE
    ```
    for(int i=0; i<signal[0].size(); i++){
            signal[0].set(i, signal[0].get(i)* 0);
    }
    ```

5. ### Equalizer 等化器
   * 利用fft將時域訊號轉成頻域離散訊號，對特定的將圖形等化器的增減益(dB)在ArrayList上操作，再由ifft將頻域訊號轉回時域訊號撥放。
   * EQ的樣子>< <br>
    ![equalizer](https://raw.githubusercontent.com/fumchin/myblog/master/assets/images/project_images/java_music_player/equalizer.jpg)

6. ### 和弦辨識
   * 利用fft將時域訊號轉成頻域訊號，先找出根音（C2~B2之間強度最大的），再將所有音階音的訊號強度排列出來，看是否能對應出和弦（Major, minor, dim）。<br>-> 只適用於簡單不複雜的音樂。
   * 成果（拿let her go 前奏作為測試對象，結果都有偵測到）<br>
    ![equalizer](https://raw.githubusercontent.com/fumchin/myblog/master/assets/images/project_images/java_music_player/chord_result.jpg)

6. ### Beat Detection
    * 原本打算利用每一區間內最大能量判別拍點，但效果不佳，也來不及嘗試更好的辦法，以後有空再說哈哈哈。

## 心得
1. 要從Wav 檔中讀出資料，過程繁瑣。
2. 要做fourier transform並對聲音訊號做調整，常常花很多時間還不知道自己到底轉出了什麼，尤其有些還要轉回來(inverse)。
3. 要在眾多頻域訊號只提取或操作需要的頻率，存檔、讀檔、播放的功能要額外寫。
4. EQ不會調。
5. 其實後來發現我們用git的方法有點怪，是各自寫完功能後pr給對方，所以就常常conflict，下次再改進哈哈哈。

## 結果
拿了個佳作。

## reference
1. [FFT algorithm](https://introcs.cs.princeton.edu/java/97data/FFT.java.html)
2. [Wav File](http://knowledge-teaching.blogspot.com/2013/09/wav.html)
