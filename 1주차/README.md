# performance-guide 1 주차 정리
# 도입
- 성능 최적화에 대한 필요성에 관련해서 6 page에 나와있는 세션당 페이지는 무슨 의미일까요?
- local Storage vs Session Storage는 언제 사용하며 왜 사용할까요?
  local storage 닫아도 데이터가 유지됨, session storage
- 교제를 안에 source panel에 대한 분석 방법이 없는데 source panel에서 중요한 점이 있을까요?
- source map > 난독화 js

❓ **성능 점수에 가중치가 부여되는 방식**
TBT(30%) - LCP(25%) - CLS(15%) - FCP(10%) - SI(10%) - TTI(10%)

- [참고사이트](https://nitropack.io/blog/post/what-is-total-blocking-time-tbt)
- [Lighthouse가 전체 성능 점수를 계산하는 방법](https://developer.chrome.com/ko/docs/lighthouse/performance/performance-scoring/#weightings)   


# 1. 이미지 사이즈 최적화
- 레티나 디스플레이를 고려하여 이미지 크기를 약 2배로 설정해야 하는데, 피그마에서 .png 이미지를 다운로드할 경우 원하는 크기의 2배로 키워 다운로드해야 한다.
- next.js 프레임워크에서 next/img 컴포넌트를 사용할 경우, 컴포넌트 Props로 srcset을 내려주면 디바이스에 따라 다른 크기의 이미지를 내려줄 수 있도록 설정할 수 있다.

https://fe-developers.kakaoent.com/2022/220714-next-image/

# 2. 코드분할 & 지연 로딩
- loading과 fetching의 차이를 알고 있나요? 가령 react-query에서도 isLoading과 isFetching이 분류되어있는 것을 생각해보면 어떤 것 같으신가요?
- react-query를 활용하여 리액트에서 제공하는 suspense 기능을 활용할 수 있게 도와주는 기능이 있는데 아시나요?
- suspense 기능을 왜 사용하나요?
- [rollup-plugin-visualizer](https://github.com/btd/rollup-plugin-visualizer)
- usetransition이랑 suspense같이쓰면 suspense가 동작안함

# 3. 텍스트 압축
- content-encoding : brotli를 아시나요? 
brotli 압축이 잘 된다. 처음부터 웹에서 압축할목적으로 만들어짐. 브라우저에서 제공하는 사용하는 단어를 사용하고있어서 압축률이 더 좋다
[참고](https://yceffort.kr/2021/01/brotli-better-html-compression)
- Content-Encoding:gzip


# 4. 병목코드 최적화
- lodash, immer
```javascript
- import _ from 'lodash';
- import { a } from 'lodash';
```

```javascript
const a = 1.12;

Math.floor(a);

~~a; // 1
~~undefined;// 0
~~null;// 0
```

추가 질문
1. Next/Image가 제공해주는 성능 상의 이점에는 무엇이 있을까요? : 사이즈를 미리 설정하니깐 cls를 발생하지않는다. img blur설정하면 사용자에게 더 자연스럽게 이미지를 보여줌
2. useEffect를 일으키는 의존성 배열에 함수를 명시해도 될까요? [useEffectEvent](https://react.dev/reference/react/experimental_useEffectEvent)
3. suspense 사용 경험
4. 이미지 최적화 사용 경험
5. 라이트하우스 최적화 경험
6. 문자를 제거할 떄 어떤 메서드를 주로 사용하고, 어떤 것이 효율이 좋은지
