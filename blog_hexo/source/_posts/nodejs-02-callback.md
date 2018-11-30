---
title: Nodejs02 CallBack Function
date: 2018-11-30 13:18:36
tags:
- language
- nodejs
- nonblock
- callback
categories:
- language
- nodejs
---

## Non-Block, CallBack Function

Non-Blocking을 사용하는 이유는 최대한 빠르게 많은 동작을 실행하기 위함이다.
즉 더 많은 요청을 더 빠르게 처리할수 있게 된다는 것이다.

이 Non-Blocking 을 이용하는 가장 기본적인 형태가 CallBack 함수라고 이해하면 될것이다. 
CallBack 함수 사용 형태는 이론적 설명보다는 예시를 보는게 이해와 사용하기 빠를 것이다.


### Callback Function ex1)
```bash
ex1 = function(a,b,c,callback){
    var result = a + b + c;
    callback(result);
}

ex1(5,10,1,function(res){
    console.log(res);
})
```


### Callback Function ex2)
```bash
ex2 = function(a,b,callback){
    callback(a+b, a-b);
}

ex2(5,3,function(res1, res2){
    console.log(res1);
    console.log(res2);
})
```



두 예시에서 모두 윗단은 함수 생성이고 아랫단은 함수 사용 부분이다.
사용 방법은 다르지만 일단 개인적으로 callback은 return개념으로 사용하고 있다.

google 뒤지면 callback 보다는 next로 많이 사용 하는데 개인적으로는 callback이 편한듯
