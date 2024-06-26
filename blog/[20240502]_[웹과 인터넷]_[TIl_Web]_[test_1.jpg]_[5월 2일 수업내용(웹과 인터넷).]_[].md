## 인터넷이란?

기존에 대량의 정보를 사람이 운송 하였는데 ARPA라는 미국 정부 기관에서 정보전달 속도를 개선 하고자 컴퓨터와 컴퓨터를 네트워크로 연결하여 만들어진 것으로 최초의 인터넷인 ARPANET이 만들어 졌다.

- 쿠바 미사일 사건 이후로 핵폭팔이 발생하면 전자기파로 인해 무선 라디오 통신이 교란될 것이고 당시 유선 통신 방식이 중앙 집중적 회선 교환 방식 이었기 때문에 상당수의 유선 네트워크의 기능 상실을 우려하여 국가 전역으로 통신 네트워크를 분산하여 거대 네트워크를 구성하기 시작했다.

## 웹이란?

인터넷을 통해 정보를 공유하고 다양한 서비스를 이용할 수 있게 해주는 서비스

- 최초의 웹 - http://info.cern.ch/
    
    인터넷망이 민간으로 퍼져나가고 유럽의 입자물리 연구소(CERN)에서 일하던 팀 리버스 라는 사람이 당시 수많은 정보들이 호환성이 없는 다른 컴퓨터들과 네트워크 시스템 위에서 떠돌고 있었고 팀은 서로 다른 운영체제, 파일 포멧, 하드웨어 등의 난관을 극복할 수 있는 정보를 공유하는 단일한 시스템의 필요성을 느꼈습니습니다. 그리고 1980년 팀은 대량의 정보에 쉽게 접근하게 하는 프로그램을 설계했고 1989년 World Wide Web이라는 시스템을 개발 하였습니다.
    

- 웹의 구성요소
    1. URL: 제공하는 리소스의 이름
    2. HTTP: 웹 브라우저와 웹 서버 간의 통신 규약
    3. DNS: 도메인을 IP네트워크에서 찾아갈 수 있는 IP로 변환해준다
    4. 웹서버: 클라이언트의 요청을 받아 처리하고 응답을 반환하는 서버
    5. 웹브라우저: 사용자가 웹 페이지를 요청하고 받은 응답을 해석해서 보여주는  프로그램
    - 도메인과 url의 차이
        
        도메인: IP를 사람이 인식 할 수 있게 만든것으로 
        
        URL: 도메인 보다는 좀 더 큰 개념으로path까지 포함된 개념
        
    - fragment(http 메세지 구조): 페이지 내의 특정 리소스에 바로 접근하게 만들어 주는 id
        
        (#helo)
        
    - query: 서버에 어떠한 값을 꺼낼때에 키, 벨류값
    (key1 = value1&key2 = value2)
- 웹의 동작
    
    도메인, URL: 우리가 접속할 웹사이트의 이름
    
    IP: 도메인,URL을 컴퓨터가 읽을 수 있게 만든 주소
    
    PORT: 운영체제에서 관리하는 프로세스에 접근할 수 있게 하는 통로
    
    (각 프로토콜의 데이터가 통하는 논리적인 통로이다.)
    
- IT분야 직업군
    - 프론트엔드 개발자(웹 퍼블리셔 - 국내에만 있는 직업으로 주로 디자인 계열의 사람들이 프론트엔드 지식이 있으면 해당 직업으로 가기도 함)
    웹 개발에서 사용자와 직접 상호작용 하는 부분을 담당한다. 프론트엔드 개발자는 HTML, CSS, Javascript등과 같은 기술들을 사용하여 웹 페이지의 구조, 디자인, 동작구현을 한다.
    - 주로 사용하는 라이브러리 및 프레임워크
        - React(사실상 국내외로 가장 많이 사용하는 프레임워크)
        - Vue
    - 백엔드
        - 백엔드는 웹 애플리케이션의 서버 측 로직과 데이터 처리를 담당하는 영역이다. 백엔드 개발자는 다양한 프로그래밍 언어와 프레임워크를 사용하여 웹 애플리케이션의 핵심 기능을 구현하고, 데이터베이스와 상호작용 하여 클라이언트의 요청에 응답합니다.
        - 가장 많이 사용하는 라이브러리 및 프레임워크
            - Java(전자정부표준 프레임워크로 국내에서는 압도적으로 많이 사용)
                - Spring
                - Spring-boot
            - Javascript(Node, Deno)
                - Express
                - Nest.JS
            - Python
                - fastAPI
                - Django
- 웹 상에서 HTML, CSS, Javascript, Java(Python)의 역할
    - HTML(Hyper Text Markup Language)
        - Hyper Text: 정해진 순서 없이, 참조를 통해 문서에서 다른 문서로 이동할 수 있는 텍스트
        - Markup Language: 태그 등을 이용하여 문서나 데이터의 구조를 표시하는 언어
        - HTML은 프로그래밍 언어가 아닌 콘텐츠의 구조를 정의하는 마크업 언어
        - 웹을 이루는 가장 기초적인 구성 요소
    - CSS(Cascading Style Sheets)
        - 웹 페이지의 모양/표현
        - 위에서 아래로 폭포처럼 내려감
        - 상속 가능(HTML은 불가능)
        - 연산가능(원래는 안되는데 지금은 가능)
    - Javascript
        - 웹 페이지의 데이터 처리 및 동적 이벤트 담당
        - 눈에 보이지는 않지만 핵심 기능을 수행하고 처리하는 역할