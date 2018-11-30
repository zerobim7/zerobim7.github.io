---
title: Nodejs03 Loop Event Handling
date: 2018-11-30 16:24:59
tags:
- language
- nodejs
- nonblock
- event
categories:
- language
- nodejs
---

## 참고 싸이트

[VELOPERT.LOG](https://velopert.com/267)
[옵저버패턴](https://ko.wikipedia.org/wiki/%EC%98%B5%EC%84%9C%EB%B2%84_%ED%8C%A8%ED%84%B4)
위의 페이지에서 보는 것이 자세함


## EventEmitter 사용

위의 옵저버 패턴을 보면 알겠지만 여러 concrete observer 들을 Observer에 등록 시켜서 사용한다.

Java의 SpringFramework로 이해하자면 Controller 혹은 ServiceImpl 단에서 service 혹은 mapper에 등록되어 있는 하위 메소드를 부르는것
예들 들어 service.method(param); 처럼 부르는 것이라고 이해하면 빠를 것이다.

여기서는 
EventEmitter.on(event1) 으로 EventEmitter에 새로운 event(옵저버)를 등록하고 
EventEmitter.emit(event1) 으로 등록되어 있는 event(옵저버)를 사용한다.

### EventEmitter 예시

```bash
//events 모듈사용
var events = require('events');

//EventEmitter 객체생성
var eventEmitter = new events.EventEmitter();

// EventHandler 함수 생성
var connectHandler = function connected(){
    console.log("Connection Successful");

    // data_recevied 이벤트를 발생시키기
    eventEmitter.emit("data_received");
}

//connection 이벤트와 connectHandler 이벤트 핸들러를 연동
eventEmitter.on('connection', connectHandler);

//data_received 이벤트와 익명 함수와 연동
// 함수를 변수안에 담는 대신에, .on() 메소드의 인자로 직접 함수를 전달
eventEmitter.on('data_received', function(){
    console.log("Data Received");
});

//connection 이벤트 발생시키기
eventEmitter.emit('connection');

console.log("Program has ended");
```


출력 결과

```bash
Connection Successful
Data Received
Program has ended
```
