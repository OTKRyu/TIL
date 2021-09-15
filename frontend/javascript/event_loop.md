# js

- dom.addEventListener('click', callback)
- fetch('URL')
- setInterval(callback, 1000)
- 위 함수들의 공통점은 비동기적으로 실행된다는 것

## 비동기란?

- 동기적인 방식에서는 함수의 호출과 실행이 순차적으로 진행된다.
- 비동기 방식에서는 함수의 호출 순서와 실행 순서가 다를 수 있다.
- js 엔진은 call stack에서 함수의 실행을 관리한다. 
- 단일 스레드 기반 언어인 js는 어떻게 비동기 함수를 처리할까(call stack 하나짜리)

## event loop

1. 비동기 함수는 call stack에 바로 쌓이지 않고 web api를 호출한다
2. 이후 백그라운드 상태에 있다가 실행 조건이 만족되면 callback queue로 이동한다
3. event loop는 call stack이 비었는지를 반복해서 확인한다
4. call stack이 비게 되면 callback queue의 함수를 call stack으로 이동시킨다.

# 참고하면 좋을 사이트

라인 기술 블로그[https://engineering.linecorp.com/ko/blog/dont-block-the-event-loop/]
