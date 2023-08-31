# 윤병현 React2-3-1

## 1주차 정리 (23.08.31)

## 가.  Next.js

### **Next.js는 리액트를 위해 만든 오픈소스 자바스크립트 웹 프레임워크 입니다.**

1. 리액트는 페이스북의 조던 발케가 만들어, 2013년 오픈소스 발표
2. 리액트는 클라이언트 사이들에서만 작동한다는 문제점이 있습니다.
    - 애플리케이션 실행 초기에 성능 부담
    - 검색 엔진 최적화의 효과를 거의 볼 수 없음
3. 이 문제를 해결하기 위해 서버에서 미리 렌더링해 두는 방법을 연구
4. 이러한 연구의 결과로 나온 것이 Next.js

### **리액트에는 없는 다양한 기능을 제공합니다.**

1. 코드 분할 : 페이지를 로딩 할 때 번들을 여러 조각으로 나누어 필요한 부분만 전송하는 방식
2. 서버 사이트 렌더링
3. 파일 기반 라우팅
4. 경로 기반 프리페칭 : 사용자가 다음에 이동할 수 있는 페이지를 미리 가져오는 기술
5. 정적 사이트 생성
6. 증분 정적 콘텐츠 생성
7. 타입스크립트에 대한 기본 지원
8. 자동 폴리필 적용 : 이전 브라우저에서 최신 기능을 제공하는 데 필요한 코드를 제공
9. 이미지 최적화  : 컴포넌트로 제공하는 이미지의 최적화 시술
10. 웹 애플리케이션의 국제화 지원 : 다국어 지원
11. 성능 분석

## 나. SSR, SSG, ISR
<br>
1. 서버 사이트 렌더링(SSR: Server-side Rendering)

SSR은 서버에서 사용자에게 보여줄 페이지를 모두 미리 구성하여 사용자에게 페이지를 보여주는 방식이다. JSP/Servlet의 아키텍처에서 이 방식을 사용했다.

<img src="https://velog.velcdn.com/post-images%2Fjeff0720%2F55e9d780-10a8-11ea-8d2b-e372e2115d30%2FSSR.png">

출처 : [https://velog.io/@jeff0720/Next.js-개념-이해-부터-실습까지-해보는-SSR-환경-구축](https://velog.io/@jeff0720/Next.js-%EA%B0%9C%EB%85%90-%EC%9D%B4%ED%95%B4-%EB%B6%80%ED%84%B0-%EC%8B%A4%EC%8A%B5%EA%B9%8C%EC%A7%80-%ED%95%B4%EB%B3%B4%EB%8A%94-SSR-%ED%99%98%EA%B2%BD-%EA%B5%AC%EC%B6%95)

SSR을 사용하면 모든 데이터가 매핑된 서비스 페이지를 클라이언트(브라우저)에게 바로 보여줄 수 있다. 서버를 이용해서 페이지를 구성하기 때문에 클라이언트에서 구성하는 `CSR`(client-side rendering)보다 페이지를 구성하는 속도는 늦어지지만 전체적으로 사용자에게 보여주는 콘텐츠 구성이 완료되는 시점은 빨라진다는 장점이 있다. 더불어 SEO(search engine optimization) 또한 쉽게 구성할 수 있다.

<aside>
📍 CSR은 client side rendering 약자로 자바스크립트 파일을 브라우저에서 해석해 랜더링하는 방식**입니다.

</aside>
<br>
2. 정적 사이트 생성(SSG: static site Generation)

개발자가 빌드 시 사전생성페이지(pre-render page)를 만들어 static 페이지로 가지고 있게 됩니다. 클라이언트에서 페이지 요청 시 사전생성페이지를 로드하여 보여 줍니다. 페이지를 사전 생성하여 가지고 있기 때문에 클라이언트 요청에 대한 응답이 빠릅니다. 빌드 할 때 페이지가 생성되므로 변경사항이 생기게 되었을 때는 next.js의 특정 함수를 활용하여 변경사항을 읽은 다음 페이지를 생성 합니다.

<br>
3. 증분 정적 재생성(ISR: Incremental static Regeneration)

ISR은 일정 주기마다 데이터의 최신 여보를 검사해서 업데이트된 데이터로 다시 페이지를 생성한다.

콘텐츠가 업데이트되었을 때 업데이트 된 정보를 보여줄 없다는  SSG의 단점을 보완하기 위한 생성 방식이다.

 즉, NextJS에서 성능상의 이점은 챙기면서도 변화된 내용에 대한 업데이트를 제공해줄 수 있는 방식이 바로 `ISR`(Incremental Static Regeneration) 방식이다. 

정적생성으로 미리 만들어놓은 사이트들도 필요하다면 업데이트가 가능하다는 이야기이다. 이를 이용한다면 정적생성의 장점을 취하되 단점을 보완할 수 있게 되는 것이다. ISR은 기존의 정적생성 방식에 몇가지 옵션들을 추가하면 바로 적용이 가능하다.

<aside>
📍 SSG(정적 페이지 생성)는 미리 만들어 놓은 페이지들 서비스 하기 때문에 속도는 빠르지만, 한번 생성하고 나면 수정이 불가능합니다. 이러한 단점을 보완하고자 나온 것이 ISR(증분 정적 재생성)입니다.

</aside>

## 다. Next.js와 비슷한 프레임워크

### 1. Gatsby

- 정적 웹 사이트를 만들 수 있는 프레임워크 입니다.
- 정적 사이트 생성만 지원합니다.
- 클라이언트 사이드 렌더링 만 지원합니다.
- 동적으로 변하는 복잡한 웹 사이트는 만들 수 없습니

### 2. Razzle

- 서버 사이드 랜더링이 가능한 자바스크립트 애플리케이션 개발이 가능합니다.
- CRA와 유사하게 프로젝트를 구성할 수 있다는 장정이 있습니다. (create-razzle-
app)
- React , Preact , Reason-React, Angular 및 Vue 와 함께 사용할 수 있습니

### 3. Nuxt.js

- Vue들 사용한 웹 애플리케이션 개발에서 리액트의 Next.js에 해당하는 프레임워크 입니다.
- Nuxt.js나 Next.js 모두 같은 목표를 갖는 프레임워크지만 Nuxt js는 더 많은 설정이 필요합니다.

### s4. Angular Universal

- 정적 사이트 생성과 서버 사이드 렌터링을 지원합니다.
- Nuxt나 Next와는 달리 대기업인 구글에서 만들었습니다.
- Angular로 개발하는 경우 Angutar Universal을 사용하는 것이 대부분입니다.