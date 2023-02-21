### SSR

- 완성된 HTML 이 서버에서 내려주기 때문에, Client 는 완성된 것을 보여주기만 하면 된다.
- 완성된 것을 서버에서 내려주기 때문에 초기 용량이 작고, 보안에 유리하다.
- 페이지마다 새로운 HTML 을 그려줘야하기 때문에, 로딩에 대한 지연이 있을 수 있고, 사용자가 많을 때에는 서버 부하의 위험이 있을 수 있다.
- 새로고침때마다 화면 깜빡임이 있다.
- 완성된 HTML 이므로, 크롤링에 유리하기 때문에 SEO 에 적합하다.

### CSR

- HTML DOM 에는 텅빈 HTML 만 존재한다.
- 처음에는 빈 HTML 파일만 받고, 자바스크립트를 다운로드하여 리액트, 즉 자바스크립트를 실행합니다.
- 자바스크립트로 인해 DOM 이 렌더링됩니다.
- 페이지가 넘어가더라도 화면 깜빡임이 없다. 대신 초기 용량이 크다.
- 랜더링은 자바스크립트를 통해 이루어지므로 캐싱이 가능하다.
- 그러나 데이터가 필요할 경우 서버와 계속해서 통신을 해야 하기 때문에 상대적으로 보안에 취약하다.
- SSR 에 비해 SEO 에 부적합하다.

### SSG

- SSG 는 pre-rendering 이라는 개념을 사용한다.
- static 한 HTML 을 build 타임에 만들어준다.
- SSR 은 request time, 즉 서버에 접속할 때마다 HTML 을 만든다.
- SSG 는 미리 정적인 HTML 을 만들어두기 때문에 서버에 부하가 없고, HTML 자체를 캐시할 수 있습니다.
- 완성된 HTML 이기 때문에 SEO 에도 적합하다.
- 따라서 내용이 동적으로 변하지 않는 정적인 사이트에 이용한다.

## Next.js

- SSR, CSR, SSG의 장점만 고려하여 페이지를 자유롭게 routing/rendering 할 수 있도록 API를 제공함
    - SSR/SSG의 용량과 보안
    - CSR의 페이지 이동 속도, 깜빡임 없음
    - Next.js의 방향성

- pre-rendering -> JS disable: CRA vs Next.js
    - Next.js 는 js 파일을 다운로드 받지 않아도 사용 가능한 것 : pre-rendering

- Section 1: SSR/CSR/SSG, ISR(revalidate), next/link
    - getStaticProps(SSG)
    - getServerSideProps(SSR)
    - revalidate: Incremental Static Regeneration(ISR)
    - CSR

- `app` directory