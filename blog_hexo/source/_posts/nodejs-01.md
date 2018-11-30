---
title: Non-Blocking Code
date: 2018-11-30 10:32:20
tags: 
- language
- nodejs
- nonblock
categories: 
- language
- nodejs
---

## NodeJs 기초 (라고 들은 것들)

nodejs를 사용할때에는 기본적으로 non-blocking 코딩을 해야 한다.

알기 쉬운 예시를 찾아보니

내가 청소, 빨래, 설거지를 해야 한다고 할때 blocking은 내가 청소를 하고 빨래를 하고 설거지를 순서대로 끝내는 것이고
non-blocking은 청소, 빨래, 설거지 업자에게 전화해서 해달라고 한 후 업자들에게 결과만 듣는 거라고 한다.


VELOPERT님 강의로 배웠다. 하나씩 정리하면서 느낀건데 이분처럼 쉽게 설명하는 것이 쉽지가 않다. 
이것도 나 혼자 보려고 하는 거지만 가르치는 것처럼 쓰는 건데, 정말 강의 처럼 설명 하려면...
정말 감사하신 분이다.

[VELOPERT.LOG](https://velopert.com/255)

### Blocking Code (일반 코딩할때 쓰이는 순서가 정해진 코딩)

``` bash
var fs = require("fs");
var data = fs.readFileSync('input.txt');
console.log(data.toString());
console.log("program has ended");
```

참고로 사용된 input.txt의 내용은

Let's understand what is a callback function.
What the HELL is it?

이다.

### Non-Blocking Code

```bash
var fs = require("fs");

fs.readFile('input.txt', function (err, data){
    if(err) return console.error(err);
    console.log(data.toString());
});
console.log("Program has ended");
```


위 두개를 실행하면 결과가 다르게 나오는데 이는 fs.readFile을 하는 속도보다. 
console.log("Program has ended"); 
가 찍히는 속도가 빠르기 때문이다. 

처음의 예시로 설명하면 청소업자한테 먼저 전화를 하고 빨래업자에게 전화를 해서 일을 맡겼는데 빨래 업자가 더 일을 빨리 끝낸 것

만약 console.log가 아닌 다른 처리가 속도가 느린 동작을 했다면
console.log(data.toString());
쪽이 먼저 출력 되었을 수도 있다.