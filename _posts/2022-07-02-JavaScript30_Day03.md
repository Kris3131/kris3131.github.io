---
layout: post
title: JavaScript30-Day03-Playing with CSS variable and JavaScript
category: practice-note
tags: JavaScript
---

調整 Spacing / Blur / Base Color 可以調整圖片間距 / 模糊 / 背景顏色

## [Demo][Demo]

## pseudocode

- CSS variable
- 抓 controls inputs DOM elements
  - 監聽 DOM elements 變化
  - 隨著變化調整 CSS variable

## 程式碼

{% highlight JavaScript %}
  const inputs = document.querySelectorAll(".controls input");
  function handleUpdate() {
    const suffix = this.dataset.sizing || "";
    document.documentElement.style.setProperty(
      `--${this.name}`,
      this.value + suffix
    );
  }
  inputs.forEach((input) =>
    input.addEventListener("mousemove", handleUpdate)
  );
{% endhighlight %}

[Demo]: https://kris3131.github.io/JavaScript30/03%20-%20CSS%20Variables/index.html