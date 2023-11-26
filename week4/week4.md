브라우저의 동작 방식에 대해 : 브라우저는 어떻게 동작하는가?
===

브라우저의 주요 기능은 사용자가 선택한 자원을 서버에 요청하고 브라우저에 표시하는 것 이다. 웹 성능의 가장 중요하게 작용하는 것이 지연시간과 브라우저의 싱글 쓰레드 문제이다. 지연시간은 브라우저의 빠른 로딩을 위해서 고려해야 할 필수적인 요소이다.
네트워크의 지연시간은 네트워크를 통해서 컴퓨터로 바이트를 전송하는데 걸리는 시간을 의미한다. 웹이 최적화 되었다는 것은 페이지 로드가 최대한 빠르게 이뤄질 수 있도록 하는 것을 의미한다.
브라우저는 메인 쓰레드 하나로 작동되기 때문에 메인 쓰레드가 요청된 모든 작업을 수행하면서도 유저와의 상호작용에 최적화되어 반응하기 위해서는 렌더링 시간이 매우 중요하다.
브라우저가 싱글 쓰레드로 동작함을 이해하고 가능한 메인 쓰레드의 책임을 줄여주는 방식이 웹 성능 향상을 이룰 수 있다.

브라우저의 기본 구조는 사용자 인터페이스, 브라우저 엔진, 렌더링 엔진, 통신, UI 백엔드, 자바스크립트 해석기, 자료 저장소로 구성된다.
이떄 1. 사용자 인터페이스는 사용자가 접근할 수 있는 영역을 뜻하며, URI를 입력할 수 있는 주소 표시줄, 이전/다음 버튼, 북마크 메뉴, 새로 고침 버튼과 현재 문서의 로드를 중단할 수 있는 정지 버튼 , 홈 버튼 등 요청한 페이지를 보여주는 창을 제외한 나머지 모든 부분이다.
2. 브라우저 엔진은  사용자 인터페이스와 렌더링 엔진 사이의 동작을 제어하는 역할을 한다. 이는 Data Storage를 참조하며 로컬에 데이터를 쓰고 읽으면서 다양한 작업을 한다. 
3. 렌더링 엔진은 웹 서버로부터 응답 받은 자원을 웹 브라우저 상에 나타낸다. 예를 들어 HTML 문서를 응답받으면 HTML과 CSS를 파싱 하여 화면에 표시한다. 
브라우저의 동작 원리를 이해하려면 렌더링 엔진의 이해가 중요한데, 브라우저는 서버로부터 HTML 문서를 응답받으면 렌더링 엔진의 HTML 파서와 CSS 파서에 의해 파싱(parsing)되어 DOM, CSSOM 트리로 변환되고 렌더 트리로 결합한다. 이렇게 생성된 렌더 트리를 기반으로 브라우저는 웹 페이지를 나타내게 된다.
4. 통신은 HTTP 요청과 같은, 서버와 통신이 가능하게 하는 네트워크 호출에 사용된다. 
5. UI 백엔드는 select, input 등 기본적인 위젯을 그리는 인터페이스 이다.
6. 자바스크립트 해석기는 자바스크립트 코드를 해석하고 실행하는 역할을 한다.
7. 자료 저장소는 Cookie, Local Storage, Indexed DB 등 브라우저 메모리를 활용하여 저장하는 영역을 담당한다.

여기서 렌더링 엔진이란, html, xml, img 등 요청받은 내용을 브라우저 화면에 표시하는 엔진을 뜻한다.
각 브라우저마다 사용하는 렌더링 엔진이 다르기 때문에, 렌더링 엔진에 따라서 같은 코드임에도 페이지가 다르게 보이는 경우가 있다.

렌더링 엔진은 서버로부터 html 문서를 응답받으면서부터 시작하고, 이는 보통 8kb단위로 전송된다.
렌더링 엔진의 기본 동작 과정은
1. 렌더링 엔진이 html 문서를 파싱 해 dom트리를 구축한다. (이떄 dom은 markup과 1:1 관계를 성립하며, 브라우저는 서버로부터 받은 html문서를 html 파서를 통해 파싱하여 파싱 트리를 생성하고 이를 기반으로 dom트리 생성한다.)
2. 그다음 외부 css파일과 함께 포함된 스타일 요소를 파싱한다. 즉, CSSOM(Css Object Model)을 생성한다. (CSS 파일은 스타일 시트 객체로 파싱되고 각 객체는 CSS 규칙을 포함한다. )
3. dom트리와 2)의 결과물을 합쳐 렌더 트리를 구축한다. DOM + CSSOM을 생성. 렌더 트리는 문서를 시각적인 구성 요소로 만들어주는 역할을 한다. (웹킷은 이 구성요소를 렌더러 또는 렌더 객체라는 용어를 사용한다. )
4. 렌더 트리 각 노드에 대해 화면상의 배치 할 위치를 결정한다. 즉, 레이아웃을 배치한다. (렌더 트리의 각 객체들에게 어느 공간에 위치해야하는지 position과 size를 정해준다. )
5. UI 백엔드에서 렌더 트리의 각 노드를 그린다. UI 백엔드가 렌더 트리의 각 객체를 화면의 픽셀 값으로 나타내어준다.

여기까지가 html 문서를 파싱하여 html과 css를 렌더링 엔진이 처리하는 과정을 나타낸 순서이다. 이를 통해 웹 페이지를 화면에 나타낼 수 있다. 
하지만, javascript는 이 과정에서 처리되지 못하며 이는 자바스크립트 엔진이 처리한다.
html 파서는 <script> 태그를 만나면 dom프로새스를 중지하고 자바스크립트 엔진으로 권한을 넘긴다. 
제어 권한을 넘겨받은 자바스크립트 엔진은 <script> 태그 내의 Javascript 코드 또는 src 속성에 정의된 Javascript 파일을 로드하고 파싱 하여 실행한다. 
Javascript의 실행이 완료되면 다시 HTML 파서로 제어 권한을 넘겨서 중지했던 시점으로 돌아가 DOM 생성을 재개하게 되며, 이처럼 브라우저는 동기적으로 html, css, javascript를 처리한다.
이떄 자버스크립트 엔진에 제어 권한이 있을 때 자바스크립트 코드가 완성되지 않은 DOM을 조작한다면 에러가 발생하기 떄문에, 예기치 못한 상황에서의 에러를 방지하기 위해 html 파일에서 자바스크립트 코드를 <body>태그 하단에 위치시키는 이유이다.


* 참고: https://bbangson.tistory.com/87, mdn 브라우저 문서