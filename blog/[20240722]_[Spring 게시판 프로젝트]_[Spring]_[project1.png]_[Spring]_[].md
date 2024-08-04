# 스프링 1차 프로젝트

## 요구사항 명세

### 프로젝트 주제

- 웹 페이지의 기본이 될 수 있는 CRUD 기능을 활용하여 공지사항, 자유게시판, 채용정보 공유 페이지 만들기

### 프로젝트 개요

- 프로젝트명: **Crane Job** - Crane(학)이 일자리를 물어다준다는 의미
- 주요기능
    - 공지사항-게시판: Create, Read, Update, Delete 기능, 페이징 처리, 검색 기능(일반 게시판에서 페이지가 넘어가도 게시글 상단에 최근 공지사항 5개 고정)
    - 채용공고 게시판: Create, Read, Update, Delete 기능, 페이징 처리, 검색 기능
    - 일반 게시판: Create, Read, Update, Delete 기능, 페이징 처리, 검색 기능
    - 일반 사용자: 로그인, 회원가입, 회원정보 수정(비밀번호와 이름), 회원 탈퇴(논리 삭제)
    - 관리: 로그인, 회원가입, 일반 사용자 권한 부여, 일반 사용자 제제
    - 댓글: 각 게시글 상세페이지에서 회원만 CRUD가

### 라이브러리

- Lombok
- Spring Data JPA
- Spring Security
- Spring Web
- QueryDSL
- MySQL
- Validation
- Thymleaf

### 주요 기능별 요구사항

- 로그인 하지않은 사용자가 접근할 수 있는 페이지
    - 로그인, 회원가입 페이지
    - 모든 게시판 요약 리스트 페이지(메인 페이지)
    - 공지사항, 채용 공고 상세 페이지
- 사용자 권한 및 등급
    - 관리자
        - 일반 사용자에게 권한 부여를 할 수 있습니다.
        - 일반 사용자에게 제재를 부과할 수 있습니다.
        - 일반, 공지사항 게시판 게시글 CRUD
        - 일반, 공지사항 게시판 댓글 CRUD
    - 사용자
        - 게시글, 댓글 CRUD
        - 공지사항, 채용 공고 R

<div align="center"><h1>기술 스택</h1></div>
<h2>Back</h2>
<div>
<img align="center" alt="스프링부트" src="https://img.shields.io/badge/spring%20boot-none?style=for-the-badge&logo=spring%20boot&logoColor=white&labelColor=%236DB33F&color=%236DB33F">
<img align="center" alt="스프링 시큐리티" src="https://img.shields.io/badge/spring%20security-none?style=for-the-badge&logo=spring%20security&logoColor=white&labelColor=%236DB33F&color=%236DB33F">
<img align="center" alt="MySQL" src="https://img.shields.io/badge/mysql-none?style=for-the-badge&logo=mysql&logoColor=white&labelColor=%234479A1&color=%234479A1">
</div>
<h2>Front</h2>
<div>
<img align="center" alt="HTML" src="https://img.shields.io/badge/html5-none?style=for-the-badge&logo=html5&logoColor=white&labelColor=%23E34F26&color=%23E34F26">
<img align="center" alt="자바스크립트" src="https://img.shields.io/badge/javascript-none?style=for-the-badge&logo=javascript&logoColor=white&labelColor=%23F7DF1E&color=%23F7DF1E">
<img align="center" alt="타임리프" src="https://img.shields.io/badge/thymeleaf-none?style=for-the-badge&logo=thymeleaf&logoColor=white&labelColor=%23005F0F&color=%23005F0F">
</div>
<h2>Tool</h2>
<div>
<img align="center" alt="피그마" src="https://img.shields.io/badge/figma-none?style=for-the-badge&logo=figma&logoColor=white&labelColor=%23F24E1E&color=%23F24E1E">
<img align="center" alt="인텔리제이" src="https://img.shields.io/badge/intellij%20idea-none?style=for-the-badge&logo=intellijidea&logoColor=white&labelColor=%23000000&color=%23000000">
<img align="center" alt="깃헙" src="https://img.shields.io/badge/git%20hub-none?style=for-the-badge&logo=github&logoColor=white&labelColor=%23181717&color=%23181717">
<img align="center" alt="아마존 배포" src="https://img.shields.io/badge/amazonec2-none?style=for-the-badge&logo=amazonec2&logoColor=white&labelColor=%23FF9900&color=%23FF9900">
<img align="center" alt="디스코드 회의" src="https://img.shields.io/badge/discord-none?style=for-the-badge&logo=discord&logoColor=white&labelColor=%235865F2&color=%235865F2">
</div>

