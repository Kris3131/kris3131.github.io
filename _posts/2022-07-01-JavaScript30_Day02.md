---
layout: post
title: JavaScript30-Day02-JavaScript and CSS Clock
category: practice-note
tags: JavaScript
---
## 功能說明
時鐘 >>> 當下時間

## Demo
[時鐘Demo][Demo]

## pseudocode

- 現在時間
  - 秒
  - 分
  - 時
- 預設指針角度為 90 度
- 一圈 360 度
- 秒 / 分 / 時角度
  - 1 秒 >>> 6 度
  - 1 分鐘 >>> 60 秒 >>> 1 分 >>> 6 度
  - 1 小時 >>> 60 分 >>> 1 時 >>> 30 度
- 每經過一秒改變 CSS transform 角度

## 重點觀念

- [setInterval][setInterval] >>> 重複呼叫 function

## 細節

- new Date()
- getSeconds / getMinutes / getHours



{% highlight JavaScript %}
  //  抓出指針 HTML elements
  const secondHand = document.querySelector(".second-hand");
  const minHand = document.querySelector(".min-hand");
  const hourHand = document.querySelector(".hour-hand");

  function setClockTime() {
    // time
    const now = new Date();
    const seconds = now.getSeconds();
    const mins = now.getMinutes();
    const hours = now.getHours();

    // degrees
    const secondDegrees = seconds * 6 + 90;
    const minDegrees = ((mins * 60 + seconds) / 60) * 6 + 90;
    const hourDegrees = (((hours - 12) * 60 + mins) / 60) * 30 + 90;

    // transform
    secondHand.style.transform = `rotate(${secondDegrees}deg)`;
    minHand.style.transform = `rotate(${minDegrees}deg)`;
    hourHand.style.transform = `rotate(${hourDegrees}deg)`;
  }
  setInterval(setClockTime, 1000);
  setClockTime();
{% endhighlight %}

[setInterval]: https://developer.mozilla.org/en-US/docs/Web/API/setInterval
[Demo]: https://kris3131.github.io/JavaScript30/02%20-%20JS%20and%20CSS%20Clock/index.html