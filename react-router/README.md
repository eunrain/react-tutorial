# react-router 를 통한 리액트 싱글 페이지 애플리케이션 만들기

## SPA 란?

Single Page Application (싱글 페이지 어플리케이션) 의 약자입니다. 말 그대로, 페이지가 1개인 어플리케이션이란 뜻입니다. 전통적인 웹어플리케이션의 구조는, 여러 페이지로 구성되어있습니다. 유저가 요청 할 때 마다 페이지가 새로고침되며, 페이지를 로딩 할 때 마다 서버로부터 리소스를 전달받아 해석 후 렌더링을 합니다. HTML 파일, 혹은 템플릿 엔진 등을 사용해서 어플리케이션의 뷰가 어떻게 보여질지도 서버에서 담당했죠.

요즘은 웹에서 제공되는 정보가 정말 많기 때문에 속도적인 측면에서 문제가 있었고, 이를 해소하기 위하여 캐싱과 압축을 하여 서비스가 제공되는데요. 이는 사용자와 인터랙션이 많은 모던 웹 어플리케이션에서는 충분하지 않을 수도 있습니다. 렌더링하는것을 서버쪽에서 담당한다는것은, 그 만큼 렌더링을 위한 서버 자원이 사용되는것이고, 불필요한 트래픽도 낭비되기 때문이지요.

그래서 우리는 리액트 같은 라이브러리 혹은 프레임워크를 사용해서 뷰 렌더링을 유저의 브라우저가 담당하도록 하고, 우선 어플리케이션을 브라우저에 로드 한 다음에 정말 필요한 데이터만 전달받아 보여주지요.

싱글페이지라고 해서, 한 종류의 화면만 있냐구요? 그건 아닙니다. 예를들어 블로그를 만든다면, 홈, 포스트 목록, 포스트, 글쓰기 등의 화면이 있겠지요. 또한 이 화면에 따라 주소도 만들어줘야 합니다. 주소가 있어야, 유저들이 북마크도 할 수 있고 서비스에 구글을 통해 유입될 수 있기 때문이죠. 다른 주소에 따라 다른 뷰를 보여주는것을 라우팅 이라고 하는데요, 리액트 자체에는 이 기능이 내장되어있지 않습니다. 따라서 우리가 직접 브라우저의 API 를 사용하고 상태를 설정하여 다른 뷰를 보여주어야 합니다.

이번에 배우게 될 react-router 는, 써드파티 라이브러리로서, 비록 공식은 아니지만 (페이스북 공식 라우팅 라이브러리는 존재하지 않습니다) 가장 많이 사용되고 있는 라이브러리인데요. 이 라이브러리는 클라이언트 사이드에서 이뤄지는 라우팅을 간단하게 해줍니다. 게다가 서버 사이드 렌더링도 도와주는 도구들이 함께 딸려옵니다. 추가적으로 이 라우터는 react-native 에서도 사용 될 수 있습니다.

만약에 여러분이 여러 화면으로 구성된 웹 어플리케이션을 만들게 된다면, react-router 는 필수 라이브러리입니다.

## SPA 의 단점

SPA 의 단점은, 앱의 규모가 커지면 자바스크립트 파일 사이즈가 너무 커진다는 것 입니다. 유저가 실제로 방문하지 않을수도 있는 페이지에 관련된 렌더링 관련 스크립트도 불러오기 때문이죠. 하지만 걱정하지마세요. [Code Splitting](https://velog.io/@velopert/react-code-splitting) 을 사용한다면 라우트 별로 파일들을 나눠서 트래픽과 로딩속도를 개선 할 수 있습니다.

리액트 라우터같이 브라우저측에서 자바스크립트를 사용하여 라우트를 관리하는것의 잠재적인 단점은, 자바스크립트를 실행하지 않는 일반 크롤러에선 페이지의 정보를 제대로 받아가지 못한다는 점 입니다. 때문에, 구글, 네이버, 다음 등 검색엔진에서 페이지가 검색결과에서 잘 안타날수도 있습니다. 추가적으로, 자바스크립트가 실행될때까지 페이지가 비어있기 때문에, 자바스크립트 파일이 아직 캐싱되지 않은 사용자는 아주 짧은 시간동안 흰 페이지가 나타날 수도 있다는 단점도 있습니다. 이는, 서버사이드 렌더링을 통하여 해결 할 수 있습니다.

> 코드스플리팅과 서버사이드 렌더링은 지금 다루기엔 복잡한 개념들이므로, 나중에 다루도록 하겠습니다.