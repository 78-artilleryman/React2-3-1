# 윤병현 React2-3-1



<br>

## 9주차 정리 (23.10.26)
## 메타 데이터

- 페이스북의 오픈 그래프처럼 공유 자료를 카드 형태로 보내려면 몇가지 메타 데이터를 주 가해야 합니다
- Nextjs에서은 내장 Head 컴포넌트를 제공하여 이런 메타 데이터를 쉽게 다를 수 있습니다.
- 어떤 컴포넌트에서든 HTML페이지의 <Head> 내부 데이터를 변경, 추가, 삭제할 수 있습니 다.

﻿다음은 index.js에 title 태그를 추가하는 코드입니다.

```jsx
export default function Home() {
  return (
    <>
    <Head>
      <title>이것은 index페이지입니다.</title>
    </Head>
    <Navbar></Navbar>
    <Widget pageName="index"></Widget>
    </>
  )
}
```

## 실습 1

```jsx
import React, { useState } from 'react'
import Head from 'next/head'

export default function Widget({pageName}) {
  const [active, setActive] = useState(false);

  if(active){
    return (
      <>
      <Head>
        <title>{pageName} false페이지 입니다.</title>
      </Head>
      <div>
        <button onClick={()=>setActive(false)}>
          오리지널 타이틀
        </button>
        <br/>
        타이틀을 확인하세요
      </div>

      </>
      
    )
  }
  return (
    <>
    <Head>
      <title>{pageName} true페이지 입니다.</title>
    </Head>
    <div>
      <button onClick={()=>setActive(true)}>
        바뀐 페이지 타이틀
      </button>
    </div>

    </>
    
  )
  
}
```

- 버튼이 있는 컴포넌트를 만들고, 클릭하면 방문 페이지에 따라 페이지 title이 바뀌게 합니다.

## 실습 2

- HTMLS
클라이언트에 보내기 전에 특정 작업을 처리해야 하는 경우는 pages/ 디렉토리
안에 있는 _app.js와 _document.js 페이지를 이용합니다.

[_app.js 페이지]

Next 프로젝트를 생성하면 기본으로 다음과 같은 pages/_appjs 파일이 생성됩니다. 

```jsx
import "../styles/globals.css";
function MyAcp({ Component,page.Props}){
	return <Component { ,,,pagepropw).>;
}
export default myApp;
```

- 이 MyApp 컴포넌트와 그 속성인 pageProps를 반환합니다.
- 이에 앞서 제작한 Navbar 컴포넌트를 주가 하면 더 이상 index, about, contact 페이지에 Navbar 컴포넌트를 추가하지 않아도 됩니다.
- 이 뿐만 아니라 pages내의 모든 페이지에 적용 됩니다.

## 다크모드 설정

```jsx
import '@/styles/globals.css'

import { useState } from 'react'
import ThemeContext from '@/components/ThemeContext'
import Navbar from '@/components/Navbar'

const themes = {
  dark: {
    background: 'black',
    color: 'white'
  },
  light: {
    background: 'white',
    color: 'black'
  }
}

export default function App({ Component, pageProps }) {

  const [theme, setTeme] = useState('light')
  const toggleTheme = () =>{
    setTeme(theme === 'dark' ? 'light' : 'dark')
  }
  return(
    <>
    <ThemeContext.Provider value={{theme, toggleTheme}}>
      <div
      style={{
        width: '100%',
        minHeight: '100vh',
        ...themes[theme]
      }}>
      <Navbar></Navbar>
      <Component {...pageProps} />
      </div>
    </ThemeContext.Provider>
 
    
    </>
  ) 

}
```
## 7주차 정리 (23.10.12)

## 페이지에서 경로 매개변수 사용

```jsx
mport React from 'react'
import { useRouter } from 'next/router'

export async function getSeverSidePropsName({params}) {

  const {name} = params
  return {
    props:{
      name,
  },
}
}

export  default function Greet(props) {
  const {query} = useRouter();
  console.log(query)
  return (
  <h1>Hello {query.name}</h1>
  )
}
```

- 파일명은 [name].js로 해준다
- 내장 getSeverSidePropsName 함수를 통해 URL에서 동작하는 [name] 변수 값을 가져오는 것
- name/yun 주소로 가면 “Hello yun” 문구가 렌더링된다.

## 컴포넌트에서 경로 매개변수 사용

- pages 밖에서는 getSeverSideProps나 getStaticProps 함수를 사용하지 못한다
- useRouter 훅을 이용하면 컴포넌트 안에서 경로 매개변수를 사용할 수 있다.
- useRouter 훅을 사용해 query 매개 변수를 가져옵니다

## 클라이언트에서의 내비게이션

```jsx
import React from 'react'
import Link from 'next/link'

export default function Navbar() {
  return (
   <>
    <div>
      <Link href="/">Home</Link> |
      <Link href="/about">about</Link> |
      <Link href="/contact">contact</Link>

    </div>
   </>
  )
}
```

- Link 컴포넌트를 사용하여 서로 다른 페이지 간의 이동을 최적화 할 수 있음
- 다른  페이지 또는 웹 사이트의 일부를 연결할 때 LInk 컴포넌트를 사용합니다
- Link로 연결된 페이지는 이미 클라이언트에 다운로드된 상태이기 때문에 화면 전환 속도가 바쁨

## 동적 경로 매개 변수

- 중첩 라우팅으로 /blog/[date]/[slug].js 라는 페이지를 연결하는 경우 10버전 이후부터는 as 값을 href값처럼 사용하면 됩니다.

```jsx
<Link href="/blog/2000-04-01/happyhappy">Post</Link>
```

## 자동 이미지 최적화

```jsx
/** @type {import('next').NextConfig} */
const nextConfig = {
  reactStrictMode: true,
  images:{
    domains:['cdn.pixabay.com']
  }
}

module.exports = nextConfig
```

- 10 버전 이후부터 image 컴포넌트를 사용해서 이미지를 자동으로 최적화할 수 있습니다
- 먼저 next.config.js에 images 속성에 다음과 같이 서비스 호스팅명을 추가합니다
- 이렇게 설정해 두면 해당 도메인에서 가져오는 이미지는 자동으로 최적화 됩니다.

### layout 속성값

- fixed : 이미지의 크기를 지정하면 화면의 크기와 상관 없이 이미지 크기를 유지 합니다
- Responsive : fixed와 반대 방식으로 화면 크기를 조절하면 그에 따라 이미지를 최적화 해서 제공합니다
- Intrinsic : fixed와 Responsive를 절반씩 수용합니다. 크기가 작은 화면에서는 이미지 크기를 조절하고, 이미지보다 큰 화면에서는 이미지 크기를 조절하지 않습니다.
- fill : 부모 요소의 가로와 세로 크기에 따라 이미지를 늘립니다. fill을 사용할 경우 width와 height는 함께 사용할 수 없습니다.

<br>

## 6주차 정리 (23.10.05)
## 정적 사이트 생성(SSG: Static Site Generation)

- SSG는 일부 또는 전체 페이지를 빌드 시점에 미리 렌더링 합니다.
- SSG는 SSR 및 CSR과 비교했을 때 다음과 같은 장점이 있습니다.

1. 쉬운 확장
    
    정적 페이지는 단순 HTML 파일이므로 CDN을 통해 파일을 제공하거나, 캐시에 저장하기 쉽습니다.
    
2. 뛰어난 성능
    
    빌드 시점에 HTML 페이지를 미리 렌더링하기 때문에 페이지를 요청해도 클라이언트나 서버가 무언가를 처리할 필요가 없습니다.
    
3. 더 안전한 API 요청
    
    외부 API를 호출하거나, 데이터베이스에 접근하거나, 보호해야 할 데이터에 접근할 일이 없습니다.
    
    필요한 모든 정보가 빌드 시점에 미리 페이지로 렌더링 되어 있기 때문입니다.
    

- SSG는 높은 확장성과 뛰어난 성능을 보이는 프런트엔드 애플리케이션을 만들고 싶을 때 가장 좋은 방법입니다.
- 한 가지 문제점은 일단 웹 페이지를 만들고 나면 다음 배포 전까지 내용이 변하지 않는다는 것입니다.,
- 조금이라도 수정하려면 필요한 데이터를 가져와서 수정하고 다시 생성하는 과정을 반복해야 합니다
- 이런 문제 때문에 나온 방법이 바로 “증분 정적 재생성(ISR: Incremental Static Regeneration)”입니다.
- 예를 들어 동적 콘텐츠를 제공하지만 해당 콘텐츠 데이터를 로딩 하는데 시간이 오래 걸린다면 SSG와 ISR을 함께 사용하여 문제를 해결할 수 있습니다
- 많은 양의 데이터를 필요로 하는 복잡한 대시보드를 만든다면, 데이터를 불러 오기 위한 REST API에 수 초가 소요됩니다.
- 만일 데이터가 자주 변하지 않는다면 SSG와 ISR을 사용해서 데이터를 10분동안 캐싱할 수 있습니다.

## 라우팅 시스템

- React의 React Router, Reach Router 등은 클라이언트 라우팅만 구현할 수 있습니다
- Next는 파일시스템 기반 페이지와 라우팅을 합니다
- 페이지는 /pages 디렉토리 안의 *.js *.jsx *.ts *.tsx 파일에서  export한 React 컴포넌트입니다.

## 파일명 []


Next.js는 React 기반의 프레임워크로, 동적 라우팅을 지원하기 위해 파일 이름에 [] (괄호)를 사용합니다. 이 괄호 안에 있는 값은 파일 이름의 일부로 간주되지 않고, 대신 Next.js는 이 값을 동적 경로 매개 변수로 해석합니다.

예를 들어, 파일 이름이 [pid].js인 경우, id는 동적 경로 매개 변수가 되며, Next.js는 이 매개 변수를 사용하여 URL의 일부를 동적으로 생성할 수 있습니다. 이를 통해 사용자가 입력한 정보를 페이지에서 동적으로 처리할 수 있습니다.

### **괄호를 사용한 동적 경로 지원 방식**

Next.js에서 괄호를 사용하여 동적 경로를 지원하는 방식은 다음과 같습니다.

1. [] 괄호를 사용하여 파일 이름을 지정합니다. 예를 들어, /pages/posts/[id].js 파일은 id 매개 변수를 사용하여 동적인 경로를 처리할 수 있습니다.
2. 동적인 경로에서 사용할 매개 변수를 괄호 안에 지정합니다. 예를 들어, [id]는 id라는 매개 변수를 나타냅니다.
3. Next.js는 이 매개 변수를 사용하여 동적 경로를 처리하고, 해당 값을 페이지 컴포넌트에 전달합니다.

<br>

## 4주차 정리 (23.09.21)
## 가. 클라이언트 사이드 렌더링(CSR)

CSR은 SPA 트렌드와 CPU 성능 상승 + JS 표준화(리액트, 뷰, 앵귤러 등의 프레임워크 발전)와 함께 본격적으로 시작되었습니다.

CSR은 쉽게 말해서 클라이언트에서 모두 처리하는 것인데, 서버에서 전체 페이지를 한번 렌더링 하여 보여주고 사용자가 요청할 때마다 리소스를 서버에서 제공받아 클리언트가 해석하고 렌더링 하는 방식이다. 

서버 사이드 렌ㄹ딩과 달리 서버에 HTML 문서를 용청하는 것이 아니라, 브라우저에서 자바스크립트로 콘텐츠를 렌더링 한는 것이다.

<aside>
📌 SPA (Single Page Application): 최초 한 번 페이지 전체를 로딩한 뒤, 데이터만 변경하여 사용할 수 있는 애플리케이션

</aside>

CSR의 간단한 예제를 살펴보면, body 안에는 id=”root”만 달랑 하나 들어있고, 어플리케이션에 필요한 자바스크립트의 링크만 들어가있다.

```tsx
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
  <div class="root"></div>
  <script src="app.js"></script>
</body>
</html>
```

HTML이 텅텅 비어있기 때문에 처음 접속하게 되면 빈 화면만 보이게 되고, 링크된 자바스크립트를 다운로드 받게 된다. 여기에는  어플리케이션에 필요한 로직, 구동하기 위한 프레임워크, 라이브러리의 소스코드들도 모두 포함되어 있다.

그렇기 때문에 처음 다운로드 받을 때 꽤나 시간이 소요될 수 있다. 또한, 앞서 말했듯이 추가적으로 데이터가 필요하면 서버로부터 데이터를 받아와서 클라이언트 쪽에서 자바스크립트와 함께 동적으로 화면을 구성하여 사용자에게 최종적으로 화면을 보여주게 되는 것이다.

## 주의점

- 네이티브 앱처럼 느껴지는 웹 앱
    - 전체 자바스크립트 번들을 다운로드 한다는 것은 렌더링할 모든 페이지가 이미 브라우저에 다운로드 되어 있다는 뜻입니다.
    - 다른 페이지로 이동해도 서버에 요청할 필요 없이 바로 페이지를 이동할 수 있습니다.
    - 페이지를 바꾸기 위해 새로 고칠 필요가 없습니다

- 쉬운 페이지 전환
    - 클라이언트에서의 내비게이션은 브라우저 화면을 새로 고칠 필요없이 다른 페이지로의 이동을 가능하게 만듭니다
    - 페이지 간 전환에 멋진 효과를 넣을 수도 있습니다. 애니메이션을 방해할 요소가 없기 때문입니다.

- 지연된 로딩과 성능
    - 웹 앱은 최소 필요한 HTML만 렌더링합니다
    - 버튼을 누르면 나오는 모달도 실제 버튼이 눌렀을 때 동적으로 생성하게 됩니다.

- 서버 부하 감소 - 서버리스 환경에서 웹 앱을 제공할 수도 있습니다

## 단점

1. 네트워크 속도가 느린 환경에서는 번들이 모두 다운로드 될 때까지 계속 빈 페이지를 보아야합니다 그렇기 때문에 사용자가 첫 화면을 보기까지의 시간이 오래 걸릴 수 있다는 단점이 있습니다.
2. SEO(Search Engine Optimization)를 꼽을 수 있다.

📌SEO란 구글과 네이버와 같은 검색 엔진들은 서버에 등록된 웹 사이트를 돌아다니면서 웹 사이트의 HTML 문서를 분석해서 우리가 검색할 때 웹 사이트를 빠르게 검색할 수 있도록 도와준다. 하지만 CSR에서 사용되어지는 HTML의 바디는 앞 선 예제처럼 대부분 텅텅 비어있기 때문에 검색 엔진들은이 CSR로 작성된 웹페이지를 분석하는데 많은 어려움을 겪고 있다.

## 코드 분석 (40p ~ 41p)

```tsx
import { useEffect } from 'react';
import Head from 'next/head';
import hljs from 'highlight.js';
import javascript from 'highlight.js/lib/languages/javascript';

function Highlight({ code, language = 'js' }) {

  useEffect(() => {
    hljs.registerLanguage('javascript', javascript);
    hljs.initHighlighting();
  }, []);

  return (
    <>
      <Head>
        <link rel='stylesheet' href='/highlight.css' />
      </Head>
      <pre>
        <code className={language}>
          {code}
        </code>
      </pre>
    </>
  )
}

export default Highlight;;
```

- CSR인 React에서는 다음 코드가 문제없이 작동하지만 Next.js의 빌드 과정에서는 문제가 생깁니다.
- Highlight.js 라이브러리가 document라는 전역 변수를 사용하는데, 이 변수는 Node.js에서 제공하지 않으며 오직 브라우저에서만 접근할 수 있기 때문입니다.
- 이 문제는 hljs 호출을 useEffect 훅으로 감싸서 해결할 수 있습니다.

```tsx
import { useEffect, useState } from 'react';
import Highlight from '../../components/HighLight';

function UseEffectPage() {
  const [isClient, setIsClient] = useState(false);

  useEffect(() => {
    setIsClient(true);
  }, []);

  return (
    <>
      <main className={styles.main}>
        <h1> React.useEffect hook </h1>
        <p className={styles.pageDescription}>
          Highlight.js uses the global <code>document</code> keyword for highlighting code. <br />
          Using the <code>React.useEffect</code> hook will make it highlight the desired content on
          the client side, once the component has been mounted.
        </p>
        {isClient && <Highlight code={codeExample} language="js" />}
      </main>
    </>
  );
}

export default UseEffectPage;
```

다음과 같이 React.useEffect와 React.usestate를 함께 써서 특정 컴포넌트를 정확히 클라이언트에서만 렌더링하도록 지정할 수 있습니다.

<br>


## 3주차 정리 (23.09.14)
## 가.  SWC와 Babel 비교

1. Babel 단점
- Babel로 변환된 코드를 이해하기 어렵다.
- 원 코드에 비해 변환 코드의 길이가 늘어난다.
- 변환에 시간이 많이 걸린다.

1. SWC의 장점
- Next 12 이후 별도의 설정 없이 SWC를 사용할 수 있다. Next.js에 내장되어 있다.
- Rust의 WASM(WebAssembly) 지원으로 어떤 종류의 플랫폼에서도 Next.js 개발을 할 수 있다
- 변환 시간이 빠르다
- 커뮤니티가 빠르게 성장하고 있어 도움 받기가 쉽다.

## 나.  서버 사이드 렌더딩(SSR)

- 생소할 수도 있지만 웹 페이지를 제공하는 가장 흔한 방법입니다.
- APM을 이용하는 일반적인 웹 페이지 생성이라고 보면 됩니다.
- 여기에 자바스크립트 코드가 적제되면 동적으로 페이지 내용을 렌더링합니다.

- Next.js도 이와 같이 동적으로 페이지를 렌더링할 수 있습니다.
- 그리고 여기에 스크립트 코드를 집어 넣어서 나중에 웹 페이지를 동적으로 처리할 수도 있는데 이를 하이드레이션이라고 합니다.

- 예를 들면 어떤 사람이 작성한 블로그 글을 한 페이지에 모아서 작성해야 한다면 SSR을 이용하는 것이 적당합니다.
- 서버 사이드 렌더링 → 자바스크립트가 하이드레이션된 페이지를 전송 → 클라이언트에서 DOM 위에 각 스크립트 코드를 하이드레이션

## 다.  렌더링 전략

- 렌더링 전략이란 웹 페이지 또는 웹 애플리케이션을 웹 브라우저에 제공하는 방법을 의미합니다.
- 정적인 페이지 제작에는 Gatsby를 추천합니다

- Next.js에서는 이 모든 방법을 완전히 새로운 수준으로 제공합니다.
- 어떤 페이지는 빌드 시점에 정적으로 생성하고, 어떤 페이지는 실행 시점에 동적으로 생성할지 쉽게 정할 수 있습니다.
- 또한 특정 페이지에 대한 요청이 있을 때마다 페이지를 다시 생성할 수도 있습니다.
- 그리고 반드시 클라이언트에서 렌더링해야 할 컴포넌트도 지정할 수 있어 개발이 쉽습니다.

- 리엑트 하이드레이션 덕분에 이 상태에서 웹 앱은 싱글 페이지 애플리케이션(SPA) 처럼 작동할 수 있습니다
- CSR과 SSR의 장점을 모두 가지는 것입니다.
- 특정 렌더링 전략만 사용한다고 가정하면 SSR이 CSR에 비해 여러 가지 장점이 있습니다.

### SSR의 장점

- 더 안전한 웹 애플리케이션: 쿠키 관리, 주요 API,데이터 검증 등과 같은 작업을 서버에서 처리하 기 때문에 중요한 데이터를 클라이언트에 노출할 필요가 없기 때문입니다.
- 더 뛰어난 웹 사이트 호환성: 클라이언트 환경이 자바스크립트를 사용하지 못하거나 오래된 브라우저를 사용하더라도 서비스를 제공할 수 있습니다.
- 더 뛰어난 SEO: 서버가 렌더링한 HTML을 받기 때문에 봇이나 웹 크롤러가 페이지를 렌더링할 필 요가 없기 때문입니다.

### SSR이 최적의 렌더링 전략이 아닌 경우

- 클라이언트가 페이지를 요청할 때마다 페이지를 다시 렌더링할 수 있는 서버가 필요합니다.
- 다른 방식에 비해 SSR이 더 많은 자원을 소모하고, 더 많은 부하를 보이며 유지 보수 비용도 증가 합니다.
- 페이지에 대한 요청을 처리하는 시간이 길어집니다.
- 페이지가 외부 API 또는 데이터 소스에 접근해야 한다면, 해당 페이지를 렌더링할 때마다 이를 다시 요청해야 합니다.
- 페이지 간의 이동은 CSR에 비해 느립니다.

- 중요한 것은 Next.js가 기본적으로 빌드 시점에 정적으로 페이지를 만든다는 것입니다.
- 페이지에서 외부 AP를 호출하거나 데이터베이스에 접근하는 등 동적 작업을 해야 한다면 해당하 는 함수를 페이지에 export해야 합니다.

<br>


## 2주차 정리 (23.09.07)
## 가. 프로젝트 생성 및 실행 방법

- create-next-app 이용하여  프로젝트 생성

```bash
npx create-next-app 프로젝트 파일명
```

- 프로젝트가 생성되면 프로젝트 디렉토리로 이동하여 다음 명령을 실행

```bash
npm run dev
```
<br>

## 나. Next.js 타입스크립트 지원

- Next.js는 타입스크립트로 작성되었기 때문에 고품질의 type definition을 지원합니다.
- 기본 언어를 타입스크립트로 지정하려면 root에 tsconfig.json이라는 설정파일 생성하면 됩니다.
- 패키지를 설치하고 나면 비어 있던 tsconfig.json 파일의 내용이 자동으로 채워집니다.
- 개발환경을 vscode 사용하면 설정 내용이 자동으로 채워지기 때문에 별도의 설치는 필요하지 않습니다.

```bash
npm install --save-dev tpyescipt @types/react @types/node
```
<br>

## 다. 바벨과 웹팩 설정 커스터마이징

- 바벨이나 웹팩의 설정도 커스터마이징 할 수 있습니다.
- 바벨은 자바스크립트 트랜스컴파일러이며, 최신 자바스크립트 코드를 하위 호환성을 보장 하는 스크립트 코드로 변환하는 일을 담당합니다.
- 하위 호환성이 보장되면 어떤 웹 브라우저에서든 자바스크립트 코드를 실행할 수 있습니다.
- 현재는 리엑트에서 바벨을 지원하지 않고 있습니다.

## 라. 웹팩

- 웹팩은 특정 라이브러리, 페이지, 기능에 대해 컴파일된 코드를 전부 포함하는 번들을 만들 어 줍니다.
- 라이브러리와 여러 개의 컴포트로 구성된 페이지를 개발했다면, 웹팩은 이 것을 하나의 번들로 합쳐줍니다.
- 만일 SASSL LESS 같은 CsS전처리기를 사용해서 개발하고 싶다면, 웹팩 설정을 수정해 주 면 됩니다. (설정 방법은 취급하지 않습니다.)

## 마. SWC를 포함한 프로젝트 생성

### SWC란?

- 말 그대로 **매우 빠른 자바스크립트 컴파일러**이며, 기존 Babel 이 하던 일의 대체제라고 합니다. 또한 SWC 는 컴파일러이지만 웹팩과 같은 자바스크립트 번들러의 기능도 제공하고 있다고 합니다. 따라서 그냥 컴파일러가 아닌 SWC 란 Rust 기반의 플랫폼입니다.

```bash
npx create-next-app@latest 파일명
```
<br>

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