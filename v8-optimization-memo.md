V8 최적화 가이드
========
V8 최적화 관련해서 흥미로운 자료가 많았다. 당장은 그렇게까지 최적화를 하는
코딩을 할 일은 없지만, 나중을 위해 링크를 모아둔다.

글들이 아주 길지만, 대부분 중복되는 이야기를 하고있어서 글 몇개 완독하고나면
나머지는 빠르게빠르게 넘길 수 있다. 또한 아래의 가이드들은 대부분 V8 기준으로
작성된 것이긴 하지만, js 런타임들 작동원리가 대부분 큰 차이가 없기때문에 대부분의
js 런타임에도 공통적으로 적용되는 가이드들이 많다.

## 0. 마이크로최적화 하지 마라
React가 빠른 이유는 VDOM Reconciliation 알고리즘 덕분이다. 리액트 이후에 나온
다른 VDOM 라이브러리들이나 Inferno가 더 빠른 이유도, keyed children sorting
알고리즘이 더 빨라서 그렇다. 당신의 코드 또한, 알고리즘으로 개선될 여지가
있을것이다.

- [Reconciliation](https://facebook.github.io/react/docs/reconciliation.html)
- [ivi@0f6694e2, `src/vdom/implementation.ts`, Line 1503](https://github.com/ivijs/ivi/blob/0f6694e266199e874e0b4297d049a273543ae082/src/vdom/implementation.ts#L1503)

V8 최적화 가이드들은, 하나같이 편한 프로그래밍을 막고, 사람이 읽기 힘든 코드를
만들게되고, 고치거나 유지보수하기 힘든 코드를 만든다. 규칙들도 매우 많기때문에
이를 다 고려하면서 프로그래밍하는것은 불가능하다. 필요해질 때 **핫루프**만
최적화하면 되는거지, 불필요하게 모든 코드를 최적화하는것은 무의미하다.

- [Amdahl's law](https://en.wikipedia.org/wiki/Amdahl%27s_law)

## 1. V8 bailout 관련 긴글들
특정 자바스크립트 코드는, V8의 코드 최적화를 막는다. 그런 코딩만 하지 않아도
자바스크립트 실행속도가 빨라진다. 이하는 그런 "피해야 할 코딩" 리스트

- **[V8 bailout reasons](https://github.com/vhf/v8-bailout-reasons)**, by victor felder

기본적으로 위 리스트에 모두 설명되어있지만, 기왕이면 원리를 이해하는편이 더
낫다.

기본적으로는 V8의 작동원리를 이해하는편이 낫다. 이하는 V8 작동원리 관련 자료들.
이때 2010년 이후로 V8 내부구조가 2년 정도의 주기로 크게 바뀌고있으니 유념할것

이하는 구글 크롬 팀에서 직접 배포한 V8 관련 자료들

- [Design Elements](https://github.com/v8/v8/wiki/Design%20Elements)
- [Breaking the JavaScript Speed Limit with V8](https://v8-io12.appspot.com)

이하는 V8 관련 커뮤니티 읽을거리들

- [How the V8 engine works?](https://thibaultlaurens.github.io/javascript/2013/04/29/how-the-v8-engine-works/)
- [I-want-to-optimize-my-JS-application-on-V8 checklist](http://mrale.ph/blog/2011/12/18/v8-optimization-checklist.html)
- [V8 Resources](http://mrale.ph/v8/resources.html)

이하는 Crankshaft 작동원리

- [a closer look at crankshaft, v8's optimizing compiler](https://wingolog.org/archives/2011/08/02/a-closer-look-at-crankshaft-v8s-optimizing-compiler)
- [A tour of V8: Crankshaft, the optimizing compiler](http://jayconrod.com/posts/54/a-tour-of-v8-crankshaft-the-optimizing-compiler)

## 2. Inferno 사례연구
Inferno 개발자의 인터뷰를 읽어보니, 자주 쓰일만한 팁이 짧게 잘 요약되어있었다.
이하는 짧은 요약

- **[Inferno - Blazing fast, React-like UI library - Interview with Dominic Gannaway](http://survivejs.com/blog/inferno-interview/)** (반드시 읽을것)

`Object`의 생김새를 최대한 변하지 않게 유지하라. 인라인 캐싱에 도움을 준다.

`Object.prototype` 대신 외부 헬퍼함수를 써라. `Object`안에 함수를 적게 넣으면
메모리가 작은 모바일이나 코드가 최적화되기 전의 실행속도가 올라간다.

Locality를 항상 고려하라

객체/프로퍼티를 가능한한 재사용하여 GC를 줄여라

`firstChild`, `lastChild`, `parentNode`, `nextSibling`, `createElement`,
`removeChild`, `insertBefore`, `replaceChild`와 같이 DOM의 일부를 조작하는
함수가 `childNodes`, `innerHTML`과 같이 DOM 전체를 조작하는 함수보다 성능이
좋다.

한 엘리먼트의 자식을 전부 없애버릴 때에는, `elem.innerHTML = '';` 보다
`elem.textContent('');`가 빠르다.

자주 쓰이는 코드는 함수로 분리하여 인라이닝을 유도하라. 예를들어 코드 전체에
`foo === null`과 같은 코드가 반복적으로 쓰인다면 저 일을 하는 함수를 따로
분리하여 `isNull(foo)`로 대체하면 코드사이즈뿐만 아니라 JIT 성능도 향상된다.

터보팬은 휴리스틱으로 함수를 인라인시킬지 말지 정한다.

- [TurboFan Inlining Heuristics](https://docs.google.com/document/d/1VoYBhpDhJC4VlqMXCKvae-8IGuheBGxy32EOgC2LnT8)

Deoptimization이 발생하지 않았는지 검사하는 툴들이 있으니 활용하라.

- ["Not optimized" JavaScript profiler bailout reasons](https://github.com/GoogleChrome/devtools-docs/issues/53)
- [Optimization killers, Tooling](https://github.com/petkaantonov/bluebird/wiki/Optimization-killers#1-tooling)

Inferno는 몇몇 DOM 이벤트들에 대해, 브라우저가 제공하는 이벤트 기능을 쓰지 않고
자체적인 이벤트시스템을 구현하여 사용하였다. 이걸로 메모리/성능 향상을 얻을 수
있었다.

## 3. 그 외 기타 글들
크롬 내부에는 여러 종류의 컴파일러들이 같이 들어있고, 그중에 자주 실행되는 몇몇
코드들만이 최적화된다.

- [A New Crankshaft for V8](https://blog.chromium.org/2010/12/new-crankshaft-for-v8.html)

Hidden Class 개념을 이해해야한다. Object의 생김새가 계속 변해야만 한다면, Object
대신 Map을 써라. V8은 코드를 분석하여 특정 값이 32비트 정수인것을 확신할 수
있으면 이를 최적화에 사용한다. 그래서 `a = a|0` 과 같은 식의 코딩이 최적화가
되는 경우가 있다.

- **[What's up with monomorphism?](http://mrale.ph/blog/2015/01/11/whats-up-with-monomorphism.html)** (반드시 읽을것)
- [Performance Tips for JavaScript in V8](https://www.html5rocks.com/en/tutorials/speed/v8/)
- [Optimizations tricks in V8](https://blog.ghaiklor.com/optimizations-tricks-in-v8-d284b6c8b183#.l5a6a4fv3)

함수의 길이가 일정이상이 되면, 옵티마이저가 해당 함수의 인라인 캐싱을 포기한다.

- [#NodeJS : A quick optimization advice](https://top.fse.guru/nodejs-a-quick-optimization-advice-7353b820c92e#.d2hg6iv2t)

함수의 `arguments` 오브젝트를 잘못 사용하면 심각한 성능저하가 있을 수 있다.

- [JavaScript: Performance loss on incorrect arguments using](https://techblog.dorogin.com/javascript-performance-loss-on-incorrect-arguments-using-bd644f5c3ee1#.liymffpqh)
