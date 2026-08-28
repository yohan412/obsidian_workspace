
### Q. nodeJS는 싱글 스레드인가 멀티 스레드인가?

**[원본 답변]** nodeJS의 주 실행 흐름은 싱글 스레드 기반의 이벤트 루프 모델입니다.

I/O 작업을 자신의 메인 스레드(이벤트 루프를 도는 싱글스레드)가 아닌 다른 스레드(libuv에서 관리하는 thread pool에 존재)에 위임함으로써 싱글 스레드로 non blocking I/O를 지원합니다.

(참고: event-driven모델을 사용하는 서버는 대체로 event loop를 활용하여 동작합니다. 예시로 redis(multiplexing),spring webflux(Reactor) 등이 있습니다.)

### Q. 참조복사(얕은복사) vs 값복사(깊은복사)

**[원본 답변]**

1. 얕은 복사(Shallow copy)는 참조 타입 데이터가 저장한 '메모리 주소 값'을 복사한 것을 의미한다.
    

JavaScript

```
/* 얕은 복사시 주의!!! */ 
let origin = ["a", "b"];
let copy = origin;

copy.push("c");

console.log(origin); //["a", "b", "c" ]; // 원본까지 바뀌어버림
console.log(copy); //["a", "b", "c"];
```

따라서 원본까지 바뀌는것에 주의해야 한다.

2. 반대로 깊은 복사(Deep copy)는 새로운 메모리 공간을 확보해 완전히 복사하는 것을 의미한다.
    
