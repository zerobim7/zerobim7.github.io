---
title: Nodejs04 Event_Function_Use
date: 2018-11-30 16:44:53
tags:
- language
- nodejs
- nonblock
- event
categories:
- language
- nodejs
---

## on(), emit(), function()

loop event 포스트에 약간 추가 내용이다.

실제로 eventEmitter와 callback function을 사용할때 어떤 방식으로 값을 넘기고 받고
출력 순서가 어떻게 되는지를 확인하는 코드다.


### 예시 코드

```bash
const events = require('events');
const eventEmitter = new events.EventEmitter();

let connectHandler1 = (a, b, c, callback) => {
    console.log('connenctHandler1 function start');
    callback(a+b+c, c-b-a);
    console.log('connenctHandler1 function end');
}

let connectHandler2 = (a, b, c, callback) => { 
    console.log('connenctHandler2 function start');
    callback(c*b*a);
    console.log('connenctHandler2 function end');
}

eventEmitter.on('connect1', (a,b,c,callback) => {
    console.log('connect1 event start');
    connectHandler1(a,b,c,(res1,res2) => {
        console.log('connenctHandler1 event start');
        callback(res1, res2);
        console.log('connenctHandler1 event end');
    });
    console.log('connect1 event end');
});


eventEmitter.on('connect2', (d,e,f,callback) => {
    console.log('connect2 event start');
    connectHandler2(d,e,f,(res3) => {
        console.log('connenctHandler2 event start');
        callback(res3);
        console.log('connenctHandler2 event end');
    });
    console.log('connect2 event end');
});

    


eventEmitter.on('connect3', (a,b,c,callback) => {
    console.log('connect3 event start');
    let result1, result2, result3; 
    eventEmitter.emit('connect1',a,b,c,(res1, res2)=>{
        console.log('connect3 event connect1');
        console.log(res2);
        console.log(res1);
        result1 = res1;
        result2 = res2;
    });

    console.log('//////////////////////');
    console.log('connect3 event middle');
    console.log('//////////////////////');

    eventEmitter.emit('connect2',a,b,c,(res)=>{
        console.log('connect3 event connect2');
        console.log(res);
        result3 = res;
    });
    callback(result1, result2, result3);
    console.log('connect3 event end');
});

eventEmitter.on('connect4', (a,b,c,callback) => {
    console.log('connect4 event start');
    eventEmitter.emit('connect3', a,b,c,(res1,res2,res3) => {
        callback(res1,res2,res3);
    });
    console.log('connect4 event end');
});

eventEmitter.emit('connect4', 1,3,5,(res,res1,res2) => {
    console.log('event start');
    console.log(res);
    console.log(res1);
    console.log(res2);
    console.log('event end');
});
```


### 출력 결과
```bash
connect4 event start
connect3 event start
connect1 event start
connenctHandler1 function start
connenctHandler1 event start
connect3 event connect1
1
9
connenctHandler1 event end
connenctHandler1 function end
connect1 event end
//////////////////////
connect3 event middle
//////////////////////
connect2 event start
connenctHandler2 function start
connenctHandler2 event start
connect3 event connect2
15
connenctHandler2 event end
connenctHandler2 function end
connect2 event end
event start
9
1
15
event end
connect3 event end
connect4 event end
```


### 해당 코드 flow

내용은 별거 없는데 console.log를 일일이 찍느라고 길어졌다.

event.connect4 -> event.connect 3 -> event.connect2 -> function.connectHandler2 /OR/ event.connect1 -> function.connectHandler1

으로 하나씩 찾으면서 들어가게 되는데 다 필요 없고 위의 코드에서 설명하고자 한건

1. event.on() 과 event.emit() 사이에 입력값과 출력값을 이동시키는 방법
2. eventEmitter.emit('eventname'){ 이 라인은 
   eventEmitter.on('eventname'){ 의 동작이 시작과 동시에 끝나는 것을 의미
3. function의 callback동작이 어디서 끝나는지를 확인하라.

위의 3가지다.


