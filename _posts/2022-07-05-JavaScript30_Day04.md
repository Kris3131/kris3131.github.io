---
layout: post
title: JavaScript30-Day03-Array Cardio Day 1
category: practice-note
tags: JavaScript
---
## Array練習


{% highlight JavaScript %}
  const inventors = [
    { first: "Albert", last: "Einstein", year: 1879, passed: 1955 },
    { first: "Isaac", last: "Newton", year: 1643, passed: 1727 },
    { first: "Galileo", last: "Galilei", year: 1564, passed: 1642 },
    { first: "Marie", last: "Curie", year: 1867, passed: 1934 },
    { first: "Johannes", last: "Kepler", year: 1571, passed: 1630 },
    { first: "Nicolaus", last: "Copernicus", year: 1473, passed: 1543 },
    { first: "Max", last: "Planck", year: 1858, passed: 1947 },
    { first: "Katherine", last: "Blodgett", year: 1898, passed: 1979 },
    { first: "Ada", last: "Lovelace", year: 1815, passed: 1852 },
    { first: "Sarah E.", last: "Goode", year: 1855, passed: 1905 },
    { first: "Lise", last: "Meitner", year: 1878, passed: 1968 },
    { first: "Hanna", last: "Hammarström", year: 1829, passed: 1909 },
  ];
{% endhighlight %}

{% highlight JavaScript %}
const people = [
  'Bernhard, Sandra', 'Bethea, Erin', 'Becker, Carl', 'Bentsen, Lloyd', 'Beckett, Samuel', 'Blake, William', 'Berger, Ric', 'Beddoes, Mick', 'Beethoven, Ludwig',
  'Belloc, Hilaire', 'Begin, Menachem', 'Bellow, Saul', 'Benchley, Robert', 'Blair, Robert', 'Benenson, Peter', 'Benjamin, Walter', 'Berlin, Irving',
  'Benn, Tony', 'Benson, Leana', 'Bent, Silas', 'Berle, Milton', 'Berry, Halle', 'Biko, Steve', 'Beck, Glenn', 'Bergman, Ingmar', 'Black, Elk', 'Berio, Luciano',
  'Berne, Eric', 'Berra, Yogi', 'Berry, Wendell', 'Bevan, Aneurin', 'Ben-Gurion, David', 'Bevel, Ken', 'Biden, Joseph', 'Bennington, Chester', 'Bierce, Ambrose',
  'Billings, Josh', 'Birrell, Augustine', 'Blair, Tony', 'Beecher, Henry', 'Biondo, Frank'
];
{% endhighlight %}

## 題目

1 ***Filter the list of inventors for those who were born in the 1500's***

MDN 參考資料 [Filter][filter]
- 比對陣列所有元素 >> 符合條件的
- 回傳新陣列
- 後面接function
[filter]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter

方法一:標準 function 寫法

{% highlight JavaScript %}
const fifteen = inventors.filter(function (inventor) {
  if (inventor.year >= 1500 && inventor.year < 1600) {
    return true;
  }
});
{% endhighlight %}

方法二:簡化 function

{% highlight JavaScript %}
const fifteen = inventors.filter((inventor) => {
  if (inventor.year >= 1500 && inventor.year < 1600) {
    return true;
  }
});
{% endhighlight %}

方法三:只有 return 一個值 >> 簡化

{% highlight JavaScript %}
const fifteen = inventors.filter(
  (inventor) => inventor.year >= 1500 && inventor.year < 1600
);
{% endhighlight %}


2 ***Give us an array of the inventors first and last names***

MDN 參考資料 [Map][map]
- 比對陣列所有元素 >> 操作元素
- 回傳新陣列
- 後面接function
[map]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map#try_it

方法一: string "+"

{% highlight JavaScript %}
const fullName = inventors.map(
  (inventor) => inventor.first + " " + inventor.last
);
{% endhighlight %}

方法二: ``

{% highlight JavaScript %}
  const fullName = inventors.map(
    (inventor) => `${inventor.first} ${inventor.last}`
  );
  console.log(fullName);
{% endhighlight %}

3 **Sort the inventors by birthday, oldest to youngest**

MDN 參考資料 [Sort][sort]
- 排序
- 比對陣列所有元素
[sort]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort

方法一: if-else

{% highlight JavaScript %}
const ordered = inventors.sort(function (firstPerson, secondPerson) {
  if (firstPerson.year > secondPerson.year) {
    return 1;
  } else {
    return -1;
  }
});
{% endhighlight %}

方法二:ternary operator

{% highlight JavaScript %}
  const oldest = inventors.sort((a, b) => (a.year > b.year ? 1 : -1));
  console.table(oldest);
{% endhighlight %}

4 **How many years did all the inventors live all together?**
MDN 參考資料 [Reduce][reduce]
- 比對陣列所有元素
- previous / current value 相加  
- 初始值

[reduce]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce

方法一: for loop

{% highlight JavaScript %}
let totalYear = 0;
for (let i = 0; i < inventors.length; i++) {
  totalYear += inventors[i].passed - inventors[i].year;
}
{% endhighlight %}

方法二: reduce

{% highlight JavaScript %}
const totalYear = inventors.reduce((previousValue, currentValue) => {
  return (previousValue += currentValue.passed - currentValue.year);
}, 0);
console.log(totalYear);
{% endhighlight %}

5 **Sort the inventors by years lived**

方法一: if-else

{% highlight JavaScript %}
const oldest = inventors.sort((a, b) => {
  const currentGuy = a.passed - a.year;
  const nextGuy = b.passed - b.year;
  if (currentGuy > nextGuy) {
    return -1;
  } else {
    return 1;
  }
});
{% endhighlight %}

{% highlight JavaScript %}
const oldest = inventors.sort((a, b) => {
  const currentGuy = a.passed - a.year;
  const nextGuy = b.passed - b.year;
  currentGuy > nextGuy ? -1 : 1;
});
console.table(oldest);
{% endhighlight %}

6 **create a list of [Boulevards in Paris][paris_wiki_link] that contain 'de' anywhere in the name**

[paris_wiki_link]: https://en.wikipedia.org/wiki/Category:Boulevards_in_Paris

**change NodeList to Array**

{% highlight JavaScript %}
  const category = document.querySelector(".mw-category");
  // Array.from
  // [...category.querySelectorAll("a")]
  const links = Array.from(category.querySelectorAll("a"));
  const de = links
    .map((link) => link.textContent)
    .filter((streetName) => streetName.includes("de"));
  console.log(de);
{% endhighlight %}

7 **Sort the people alphabetically by last name**

{% highlight JavaScript %}
const alpha = people.sort((a, b) => {
  const [aLast, aFirst] = a.split(", ");
  const [bLast, bFirst] = b.split(", ");
  return aLast > bLast ? 1 : -1;
});
console.log(alpha);
{% endhighlight %}

8 **Sum up the instances of each of these**

{% highlight JavaScript %} 
const data = ['car', 'car', 'truck', 'truck', 'bike', 'walk', 'car', 'van', 'bike', 'walk', 'car', 'van', 'car', 'truck', 'pogostick'];
{% endhighlight %}

{% highlight JavaScript %}
const transportation = data.reduce((obj, item) => {
  if (!obj[item]) {
    obj[item] = 0;
  }
  obj[item]++;
  return obj;
}, {});
console.log(transportation);
{% endhighlight %}


