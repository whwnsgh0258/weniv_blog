## git 과 GitHub에 대해서 설명

### 1. git이란?

- git은 소스코드 및 파일의 변경 내역을 저장 하는 분산 버전관리 시스템

### 2. GitHub이란?

- git 기반의 웹서비스(해외 사이트이기 때문에 개인정보, 국방과 같은 민감한 정보는 git 기반의 국내 사이트를 사용하기도 한다)
- 유지보수, 저장용량, 코드 수정이 언제 어떻게 되었는지 추적(추적관리), 코드용량(최초의 문서에서 수정된 내용만 저장)
- 버전관리( ex - 버전4에 와서 버전 2가 더 좋으면 돌릴 수 있음)
- 코드 관리(협업, 코드 피드백 및 리뷰)

## 회사에서 사용하는 GitHub(프로젝트, 위키, 컨벤션 언급)

### 1. GitHub 프로젝트

1. 깃헙 이슈
2. 깃헙 위키
3. GitHub 프로젝트

## 간단한 프로젝트(리드미 + HTML)

1. 데이터베이스 모델링(중요 - 별 5개)
2. Mermaid, 마크다운
3. 퍼블리싱 작업
    1. settings → pages → branch(main → save)
    2. 깃허브에서 만든 페이지의 도메인을 커스텀 하고 싶다면 aws, 카페24와 같은 도메인 구입 사이트에서 도메인을 구입하여 사용
4. **https://whwnsgh0258.github.io/test/(위의 작업을 통해 만든페이지)**

## CLI(pull, add, commit, push, clone)

git pull: 코드를 원격 저장소에서 가져오는것 입니다

git add: 새로운 버전을 만들기 위한 준비작업을 진행하고 변경된 소스코드를 등록 합니다

git commit: 새로운 버전을 만듭니다.

git push: 원격 저장소에 코드를 올립니다.

1. 포트폴리오를 만들고 있을때 CLI 사용
    1. 레퍼지스토리 파일 생성(add readme file 선택 - 안해도 되는데 따로 설정해야 하는게 있음)
    2. 코드 누르고 링크 복사
    3. 복사한 코드를 창에서 shift + insert로 붙혀넣기
    4. git add .
    5. git commit -m ‘🗒️ 커밋 메세지’
    6. git push

주로 b ~ f 과정을 반복하여 사용

- 간단한 CLI 실습 하기
    - hello 폴더에 a폴더 만들어서 깃허브에 올리기(빈 파일은 깃허브에 올리기 불가능), readme파일 수정
        - 해결 및 과제수행
            1.  GitHub에서 hello라는 repository 만들기
            2. git 푸시를 위한 사용자 정보 설정(이때 이메일 설정 이상하게 하면 푸시 안되니까 똑바로 설정)

            ```bash
            $ git config --global user.name "깃허브 이름"
            $ git config --global user.email 깃허브이메일@이메일.com
            $ git gonfig --list
            ```

            1. 사용자 정보 확인

            ```bash
            $ git config --get user.name
            $ git config --get user.email
            ```

            1. hello 폴더열고 a 폴더 생성(빈폴더는 푸시가 안되기 때문에 빈파일 하나 만들어줘야함)

            ```bash
            $ cd hello
            $ mkdir a
            $ cd a
            $ touch 빈파일.txt
            $ cd ..
            ```

            1. 리드미 파일 수정

            ```bash
            $ vi readme.md echo
            
            #hello
            #hello
            #hello
            :wq!
            ```

            1. a폴더, 수정된 [readme.md](http://readme.md) 푸시하기

            ```bash
            $ git add .
            $ git commit -m ':pencil:리드미'
            $ git push
            ```

          결과

          <img alt="깃헙예시.png" src="/blog/TIL/5:3/깃헙예시.png"/>


## 피드백 및 실습

- 피드백
    - 기존에 학교를 다니면서 깃허브를 이용하여 파일을 내려받았지만 까먹은 내용들이 많았는데 오늘 수업을 들으면서 다시 상기하게 되었다.
    - readme파일을 수정하는 방법에 대해서는 사실 잘 몰랐는데 금일 수업을 들으면서 리드미 작성 방식에 대하여 알게되었다.
    - 항상 개인 프로젝트만 해오거나 팀 프로젝트를 진행 할때도 거의 혼자 코드를 짜서 깃허브에서 협업을 한적이 없었는데 금일 수업을 통해 어떤 식으로 협업이 진행되는지 알게되었다.
    - 찍먹 했으니까 이제 반복 하면서 다시는 까먹는 일 없도록 하자

5/8 html 과제 올려보기(airbnb - EventPage 만들어보기)

https://github.com/whwnsgh0258/airbnbEvent/tree/main

5/8 블로그 소스 내려받아서 배포해보기