---
layout: post
title: JavaScript30-Day01-Drum-Kit
category: practice-note
tags: JavaScript
---

## 完成 Demo 
[Drum-Kit][Drum-Kit] <br>

## 功能 
按下鍵盤對應的英文，出現對應的聲音。

## pseudocode:

- 監聽按下鍵盤時
- 找出按下的鍵盤符號
  - 按下去時播放音效
  - 按下去時切換CSS效果 
- 當動畫播放完成後，切回初始狀態


## 重點觀念:

- querySelector 抓取 HTML 元素
- addEventListener 在 HTML 元素中加入事件監聽
- 操作 classList 新增 / 移除 CSS 效果 

## 細節:

- querySelector >>> 選取器概念
- HTML >>> audio >>> play()
- HTML >>> audio >>> set currentTime
- addEventListener >>> transitionend
<br>

## 程式碼:

{% highlight JavaScript %}
  function playSound(e) {
    // 按下 keyboard >>> 要知道是多少 >>> 播放聲音 / 變化動畫
    // 抓 audio / div>.key
    const audio = document.querySelector(`audio[data-key="${e.keyCode}"]`);
    const key = document.querySelector(`.key[data-key="${e.keyCode}"]`);
    if (!audio) return;
    // 當按下 audio 對應 keyCode >>> 播放音樂
    // 每次播放將 currentTime 設定為 0
    audio.currentTime = 0;
    audio.play();
    // key classList 加上 playing CSS 效果
    key.classList.add("playing");
  }
  function removeTransition(event) {
    if (event.propertyName !== "transform") return;
    this.classList.remove("playing");
  }
  // 監聽器 >>> keyboard
  window.addEventListener("keydown", playSound);
  // 當 playing CSS 效果結束時 >>> 把動畫拉掉
  // 把所有 key 抓出來 >>> 監聽 CSS 動畫效果
  const keys = document.querySelectorAll(".key");
  // 在每一個 key 上設定監聽器
  keys.forEach((key) =>
    key.addEventListener("transitionend", removeTransition)
  );
{% endhighlight %}



[Drum-Kit]: https://kris3131.github.io/JavaScript30/01%20-%20JavaScript%20Drum%20Kit/index.html