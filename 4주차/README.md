# 4. 이미지 갤러리 최적화

# 캐시 최적화

> **캐시란?**
> 자주 사용하는 데이터나 값을 미리 복사해 둔 임시 저장공간 또는 저장하는 동작.
> 최초에만 다운로드하여 캐시에 저장해두고 그 이후 요청시에는 저장해둔 파일을 사용한다.

## 캐시의 종류

- 메모리 캐시 : 메모리에 저장하는 방식. 메모리는 RAM
- 디스크 캐시 : 파일 형태로 디스크에 저장하는 방식

- 어떤 캐시를 사용할지는 직접 제어할 수 없다. 브라우저가 사용빈도나 파일크기에 따라 특정 알고리즘에 의해 처리한다.

## Cache-control

- 리소스의 응답 헤더에 설정되는 헤더
- 브라우저는 서버에서 이 헤더를 통해 캐시를 어떻게, 얼마나 적용해야하는지 판단한다.

- no-cache : 캐시를 사용하기 전 서버에 검사 후 사용
- no-store : 캐시 사용 안 함
- public : 모든 환경에서 캐시 사용 가능
- private : 브라우저 환경에서만 캐시 사용, 외부 캐시 서버에서는 사용 불가
- max-age : 캐시의 유효 시간

<aside>
👉🏻 **캐시된 리소스와 서버의 최신 리소스가 같은지 다른지 어떻게 체크할까?**
서버에는 캐시된 리소스의 응답헤더에 있는 Etag 값과 서버에 있는 최신 리소스의 Etag 값을 비교하여 캐시된 리소스가 최신인지 아닌지 판단한다.

</aside>


- `Cache-Control`
- 적절한 캐시 유효 시간
    - 파일 유형별 캐시 유효시간을 다르게 가져간다.
        - HTML: 항상 최신 버전의 웹 서비스를 제공하기 위해 no-cache 설정.
            - 항상 최신 버전의 리소스를 제공하면서도 변경 사항이 없을 때만 캐시를 사용하기 위해서
        - JS,CSS: public, max-age = 31536000 (1년)
            - 빌드된 자바스크립트와 CSS 파일은 파일명에 해시를 함께 가지고 있다. (main.bb8aac28.chunk.js). 코드가 변경되면 해시도 변경되어 완전히 다른 파일이 된다
            - 캐시를 아무리 오래 적용해도 HTML만 최신 상태라면 자바스크립트나 CSS 파일은 당연히 최신 리소슬를 로드한다

    - 빌드된 파일을 S3에 올릴 때 이전 파일 관리.
    - [읽으면 좋은 자료: 토스 기술 블로그](https://toss.tech/article/smart-web-service-cache)
- 캐시보다 빠른 레지스터
- [캐시무효화](https://velog.io/@gil0127/%EC%BA%90%EC%8B%9C-%EB%AC%B4%ED%9A%A8%ED%99%94)
- react-query SSR 관련 이슈
    - [공식문서](https://tanstack.com/query/v4/docs/react/guides/ssr#using-hydration)


![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/fa066aaf-9963-4fca-b4d4-e45903492786/Untitled.png)

path.endsWith 메서드를 이용해 파일의 종류를 구분하여 캐시를 적용

# 불필요한 CSS 제거

## PurgeCSS

- 사용하지 않는 CSS 코드를 제거
- 파일에 들어있는 모든 키워드를 추출하여 해당 키워드를 이름으로 갖는 CSS 클래스만 보존하고 나머지 매칭되지않은 클래스는 모두 지우는방식으로 CSS 파일을 최적화한다.
- - [관련 이슈(저번주 숙제)](https://github.com/Learning-Is-Vital-In-Development/23-12-FrontendPerformanceOptimizationGuide/issues/34)

# 이미지 지연로딩

- 라이브러리를 사용해서 구현

# 레이아웃 이동 피하기

- 화면상의 요소 변화로 레이아웃이 갑자기 밀리는 현상
- ux에 좋지않은 영향을 준다.
- lighthouse의 cls 지표를 확인해보기!
- css : aspect-ratio 속성을 사용해 이미지의 사이즈를 기정의할수있다!!!!

- layout shift 수식
  [https://web.dev/i18n/ko/cls/](https://web.dev/i18n/ko/cls/)
  
  
<aside>
❓ Layout Shift Score = Impact Fraction * Distance Fraction
</aside>


- lighthouse의 CLS 지표
- 레이아웃 이동의 원인
- 레이아웃 이동의 해결
    - 공간을 미리 잡아둔다.
- Next.js를 쓰는 이유
    - 이미지 컴포넌트, 폰트 등 최적화를 알아서 잘 해주는 프레임워크.
- [layout thrashing](https://web.dev/avoid-large-complex-layouts-and-layout-thrashing/#avoid-layout-thrashing)
- https://patterns-dev-kr.github.io/


# 리덕스 렌더링 최적화

- useSelect로 관찰하고있는 값이 실제로 변경되었을때 리렌더가 발생하는 방식으로 처리할 수 있다.

- [useState에서 함수를 쓰는 경우](https://react.dev/reference/react/useState#updating-state-based-on-the-previous-state)


### Topic: useState hook - Lazy Initializing
이때까지 항상 습관적으로 React에서 useState 훅의 기본값을 그냥 넣어서 사용해왔습니다.
하지만 이전에 어떤 영상에서 useState 기본값으로 콜백 함수를 넣는 것을 본 적이 있습니다.
당시에 무거운 데이터 같은 경우에 최적화를 고려하여 콜백 함수로 넣을 수도 있다(?)로만 막연하게 잘못 인지하였습니다.
오늘 저녁 당시의 궁금증이 다시 상기되어 찾아봤습니다.
우선 결론적으로 useState 기본값으로 항상 해왔던 방식으로 원시값 ex) 문자열, 숫자, 불리언 등 을 세팅하거나
react-query로 서버 상태 관리하기 이전 방식인 useEffect 를 사용하여 api 초기 데이터를 넣을 수 있습니다.
(물론 useEffect를 사용하면 초기에는 데이터가 없어서 깜빡거릴 수 있는데 스켈레톤이나 loading spinner로 보통 처리하곤 합니다)
useState의 기본값으로 그냥 넣거나 useEffect를 이용해서 초기화 하는 것은 방금 말씀드렸습니다.
그러면 기본값으로 콜백 함수를 넣는 것은 왜 그런 것일까요.
어떤 데이터에 대해서 사전에 연산 비용이 많은 경우에 콜백 함수를 넣어
리렌더링이 되더라도 콜백 함수가 재호출되어 사전 전처리 연산을 또 할 필요 없이 초기값으로 넣은 콜백 함수가 1번만 호출되도록 할 수 있습니다.
만약 1초가 걸리는 연산이 사전에 있고 콜백 함수를 이용하지 않으면 리렌더링 할 때 마다 함수형 컴포넌트가 재호출되면서 해당 연산을 계속 1초씩 하게 될 수 있지만,
콜백 함수를 이용한다면 맨 처음에 초기값을 생성 시에만 호출시킬 수 있게 됩니다.
state가 업데이트 되어 리렌더링이 될 때 초기값을 또 연산 비용을 들여가면서 생성할 이유가 없기에
맨 처음에 초기값 세팅 시 1번만 콜백 함수로 호출할 수 있게 하는 것입니다.
(우선 초기화할지 말지를 내부적으로 비교하고 초기값이 필요하면 그제서야 콜백 함수를 호출해서 초기값이 세팅됩니다.)
자 그러면 반대로 만약에 모든 useState의 초기값으로 콜백 함수를 활용한 Lazy Initializing 기법을 이용한다면 어떨까요.
두 가지를 비교해야 합니다.
콜백 함수 이용 시 한 번만 호출되어 연산 비용을 줄일 수 있지만
JS 엔진이 해당 실행 컨텍스트를 평가하는 과정에서 함수를 만드는 비용도 존재하게 됩니다.
(물론 함수 내부 코드의 평가/실행 과정은 호출되기 이전이라 아직입니다.)
만약 단순히 () => false 와 같은 원시값을 만든다면 그냥 false를 직접 넣는 것이 효율적입니다.
굳이 heap에 함수 공간을 차지하는 메모리를 잡아서 할 이유가 없습니다.
실제로 저는 해당 기법을 써본 적은 없지만, 보통 비싼 비용의 계산이 들거나 localStorage의 접근, 배열을 사전에 조작하는 연산, new Date() 등에 활용하는 것 같습니다.
마지막 참고로 이런 코드를 사용할 수도 있을 것 같습니다.
```javascript
const [counter, setCounter] = useState(() => Math.floor(Math.random() * 16));


const 무거운_함수 = () => {};

const [counter, setCounter] = useState(무거운_함수());
const [counter, setCounter] = useState(무거운_함수);

const [count, setCount] = useState(
  Number.parseInt(window.localStorage.getItem(cacheKey)),
)
const [count, setCount] = useState(() =>
  Number.parseInt(window.localStorage.getItem(cacheKey)),
)
```

- [레퍼런스 1] - [React 공식 문서](https://reactjs.org/docs/hooks-reference.html#lazy-initial-state)
- [레퍼런스 2] - [[React] Lazy Initializing를 사용해 최적화](https://satisfactoryplace.tistory.com/277)
- [레퍼런스 3!!!] - [리액트의 useState와 lazy initialization](https://yceffort.kr/2020/10/IIFE-on-use-state-of-react)
- [레퍼런스 4] - [[Hook 시리즈] Lazy initialization 이 대체 뭔데 그래서](https://velog.io/@samkong/Lazy-initialization)
- [레퍼런스 5] - [ReactJS useState Hook - lazy initialization and previous state](https://blog.greenroots.info/react-hook-usestate-lazy-initialization-previous-state)

- [공식문서](https://legacy.reactjs.org/docs/hooks-reference.html#lazy-initial-state)


# 병목 코드 최적화

- 메모이제이션

<aside>
❓ useCallback과 useMemo, React.memo
</aside>

- 메모이제이션
    - [메모이제이션 관련 글](https://d2.naver.com/helloworld/9223303)
    - 상태를 상위 컴포넌트로 올린다.
    - useReducer
- 깊은 복사
    - `JSON.stringify`, `JSON.parse`
    - 재귀 함수
    - [structuredClone](https://developer.mozilla.org/en-US/docs/Web/API/structuredClone)


- double tilde

~~10.324 > 10

- Math.floor() 대신 사용
    - 속도 측면: ~~ > Math.floor() > parseInt
- undefined 또는 null을 0으로 변환할 때 사용하기

- 문자열로 데이터 타입 변환
기호가 속도가 가장 빠르다 > toString
