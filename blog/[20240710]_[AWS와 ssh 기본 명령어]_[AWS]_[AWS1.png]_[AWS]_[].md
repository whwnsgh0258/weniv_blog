# ssh 기본 명령어1

## AWS 회원가입

[Cloud Services - Amazon Web Services (AWS)](https://signin.aws.amazon.com/)   
해당 페이지에 들어가서 콘솔에 로그인 버튼 클릭 후 회원가입 하면 됨

회원가입을 하고 로그인하면 아래와 같은 페이지가 나옴   
<img alt="#" src="blog/TIL/7:10/AWSConsole.png"/>

## ssh client 설치 및 접속 환경

### Tabby

- URL: [Tabby 홈페이지](https://tabby.sh/)
- 다운로드 링크(여기에서 각자 OS에 맞게 다운 받으면 됨)   
  [Tabby 깃헙](https://github.com/Eugeny/tabby/releases/tag/v1.0.209)
- 초기 화면 <img alt="초기화면" src="blog/TIL/7:10/Tabby.png"/>
- 프로필 및 연결 <img alt="TabbyProfile" src="blog/TIL/7:10/profile.png"/>
- 프로필 생성 <img alt="프로필 생성" src="blog/TIL/7:10/CreateProfile.png"/>
    - 호스트: AWS의 퍼블릭 IPv4 주소 입력
    - 사용자 이름, 비밀키: 이미지의 사용자 이름을 작성하고 다운받은 기본키를 추가해줌 <img alt="프로필 세팅" src="blog/TIL/7:10/ProfileSetting.png"/>
- 실행 <img alt="#" src="blog/TIL/7:10/sshStart.png"/>

# 기본 명령어

## cd

`cd`는 "change directory"의 약어로 디렉토리(폴더)를 이동 시킬때 사용하는 명령어 입니다.

### cd 사용 방법

- `cd [경로]`: 지정한 디렉토리로 이동 합니다.(폴더의 절대 경로와 상대 경로 모두 사용가능합니다.)
- `cd`: 아무런 인자 없이 `cd`만 입력하면 현재 사용자의 홈 디렉토리로 이동 합니다.
- `cd ~`: 홈 디렉토리로 이동 합니다.
- `cd ..`: 현재 디렉토리의 부모 디렉토리로 이동 합니다.
- `cd -`: 이전 디렉토리로 이동 합니다.(직전에 위치했던 디렉토리)

## pwd

`pwd`는 "print working directory"의 약어로 현재 작업중인 디렉토리의 절대 경로를 출랙해 줍니다.

## ls

`ls`는 "list"의 약어로 디렉토리의 내용물을 나열 해줍니다.(폴더, 파일, 링크 등)

### ls 사용법

`ls`는 `ls [옵션] [디렉토리 경로]`의 형식으로 작성됩니다.

- `-l`: 디렉토리의 내용물을 long format(긴 형식)으로 출력 합니다.(타입, 권한, 링크 수, 소유자, 그룹, 크기, 타임 스탬프, 파일 이름
  등)<img alt="ls -l 예시" src="blog/TIL/7:10/ls-l.png"/>
- `-a`: 디렉토리 내부의 모든 숨김 파일을 출력 합니다.<img alt="ls -a 예시" src="bog/TIL/7:10/img.png"/>
- `-h`: 파일의 크기를 쉽게 읽을 수 있는 형태로 변환하여 출(근데 `-h`는 `-lh`로 작성 해줘야함)<img alt="-h 예시" src="blog/TIL/7:10/ls-lh.png"/>

이 외에도 `ls`명령어의 옵션들이 있는데 `man ls`를 입력하면 여러 옵션들을 확인할 수 있음

## mv

`mv`는 디렉토리를 옮기거나 이름을 변경하는데 사용

> `mv`는 복사의 개념이 아닌 말 그대로 현재 파일 또는 폴더를 다른 곳으로 옮기거나 이름을 바꾸는 것이기 때문에 원본 데이터가 남지 않습니다.

### mv 사용법

```shell
mv [원본] [대상]
```

- 예시

```shell
touch test1.txt # test1.txt 파일 생성
mkdir document # document 폴더 생성
ls # 현재 위치에 있는 폴더 및 파일 출력
mv test1.txt /home/ubuntu/document # test1.txt 파일을 document 폴더로 이동
ls # 현재 위치에 있는 폴더 및 파일 출력
cd document # document 폴더로 이동
ls # document 폴더의 list 확인
```

<img alt="mv 예시" src="blog/TIL/7:10/mv.png"/>

## cp

파일 또는 디렉토리를 복사 후 다른 곳에 붙혀넣기 해줍니다.

> `cp`를 사용하면 원본 데이터는 그대로있고 새로운 파일 또는 폴더가 생성 됩니다.

### cp 사용번

```shell
cp [options] source destination
```

- option: `cp` 명령이 동작할 때 특정한 옵션을 추가해 줍니다.(선택사항임)
    - r과 R은 디렉토리의 내용을 모두 새로운 디렉토리에 복사합니다.
    - i는 대상 파일이 있는경우 덮어씌울 것인지 물어봅니다.
    - v는 어떤 파일이 복사되었는지 알려줍니다.
- 예시
    1. 파일 복사
  ```shell
  cp test1.txt test2.txt # test1.txt 파일을 복사해서 test2.txt 파일에 붙혀넣기
  ```
    2. 디렉토리 복사
  ```shell
  cp -r document documentCopy # document 폴더와 그 하위 내용들을 documentCopy 폴더에 붙혀넣기
  ```
    3. 복사 과정 표시
  ```shell
  cp -v document documentCopy # 복사가 진행되는 동안 진행상황을 표시
  ```
    4. 복사 전 확인
  ```shell
  cp -i document documentCopy # 기존에 documentCopy 폴더가 존재하는 경우 덮어씌울 것인지 물어봄
  ```

## rmdir

"remove directory"의 약어로 디렉토리를 삭제할 때 사용합니다.

> 삭제되는 디렉토리가 비어있을 때만 `rmdir`이 작동하며 비어있지 않으면 작동하지 않습니다.

### rmdir 사용법

```shell
rmdir [옵션] 디렉토리이름
```

- 옵션
    - `-p`, `-parents`: 지정한 디렉토리와 그 부모 디렉토리가 함께 삭제 됩니다.
    - `-v`, `-verbose`: 삭제되는 디렉토리의 이름을 보여줍니다.

## rm

"remove"의 약어로 파일 또는 디렉토리를 삭제하는데 사용됩니다.

### rm 사용볍

```shell
rm [옵션] 파일, 디렉토리 이름
```

- 옶션
    - `-r`, `-R`: 디렉토리와 내부의 서브디렉토리, 파일 등을 모두 삭제 합니다.
    - `-i` : 삭제하기 전에 사용자에게 확인을 요청합니다.
    - `-f` : 강제로 파일 또는 디렉터리를 삭제합니다.(삭제할 때 별도의 확인 없이 바로 삭제하기 때문에 주의 해야합니다.)
    - `-v` : 삭제된 파일 정보를 출력합니다.
- 예시

1. 파일 삭제

```shell
rm test1.txt # test1.txt 파일 삭제
```

2. 여러파일 삭제

```shell
rm test1.txt test2.txt # test1.txt, test2.txt 파일 삭제
  ```

3. 서브 디렉토리 및 내부 파일까지 삭제

```shell
rm -r document # document 폴더와 하위 폴더 및 파일 삭제
  ```

# sort

`sort`는 Linux/Unix에서 사용되는 매우 유용한 명령어 중 하나입니다. 이 명령어는 파일의 내용을 정렬하기 위해 사용되며, 라인 단위로 동작합니다. `sort` 명령어는 기본적으로 ASCII 문자 순서에
따라 오름차순으로 정렬합니다.

### sort 사용법

``` shell
sort [옵션] [파일]
```

- 옵션
    - `-r` : 내림차순으로 정렬합니다. (기본적으로 sort는 오름차순으로 정렬합니다.)
    - `-n` : 숫자로 정렬합니다. (기본적으로 sort는 문자열로 인식하여 정렬합니다.)
    - `-k` : 지정한 필드(열)로 정렬합니다.
    - `-t` : 필드 구분자를 지정합니다.
    - `-u` : 중복된 라인을 제거합니다.
- 예시
    1. 기본적인 정렬
  ```shell
  sort numbers.txt # numbers.txt 내부의 데이터를 오름차순으로 정렬  
  ```
    2. 내림차순 정렬
  ```shell
  sort -r numbers.txt # numbers.txt 내부의 데이터를 내림차순으로 정렬
  ```
    3. 숫자로 정렬(기본적으로 문자열로 정렬하기 때문에 숫자를 정렬할 때 사용)
  ```shell
  sort -n numbers.txt # numbers.txt 내부의 데이터를 숫자로 정렬
  ```
    4. 특정 필드로 정렬
  ```shell
  sort -k 2 -n names.txt # names.txt 파일의 두번째 필드로 정렬
  # 예시 데이터: 홍길동 20 의 2번째 필드는 20으로 vim 에서는 스페이스바로 필드를 구분함
  ```

## grep

"Global Regular Expression Print"의 약자로 텍스트의 패턴을 검색하는 데 사용되는 명령어입니다.

기본 구문은 다음과 같습니다:

```bash
grep [options] pattern [file...]
```

- `options`: 명령을 수행하는 방법을 변경하는 데 사용됩니다.
- `pattern`: 파일에서 찾으려는 문자열입니다.
- `file...`: 검색할 파일 또는 디렉토리입니다.

- 옵션
    - `-i`: 대소문자를 무시하고 검색합니다.
    - `-r` 또는 `-R`: 디렉토리 및 하위 디렉토리를 재귀적으로 검색합니다.
    - `-v`: 패턴과 일치하지 않는 줄을 출력합니다.
    - `-l`: 패턴과 일치하는 줄이 있는 파일 이름만 출력합니다.
    - `-n`: 패턴과 일치하는 줄과 함께 해당 줄 번호를 출력합니다.

- 예시

    1. 파일에서 단어 검색하기

  ```bash
  echo "Hello, this is a test file. It contains several words, including the word test." > test.txt
  grep "test" test.txt
  # test.txt 파일에서 "test" 단어가 있는 모든 줄을 출력
  ```  
    2. 여러 파일에서 단어 검색하기

  ```bash
  echo "This is a second test file. It also contains the word test." > test2.txt
  grep "test" test.txt test2.txt
  # test.txt 및 test2.txt 파일에서 "test"가 있는 모든 줄을 출력
  ```

    3. 재귀적으로 단어 검색하기

  ```bash
  mkdir test_dir
  echo "This is a file in a directory. It also contains the word test." > test_dir/test3.txt
  grep -r "test" .
  #  현재 디렉토리와 그 하위 디렉토리에서 "test" 단어가 있는 모든 줄을 찾아 출력
  ```

    4. 특정 패턴을 포함하지 않는 줄 찾기
  ```bash
  grep -v "test" test.txt
  # test.txt 파일에서 "test" 단어가 없는 모든 줄을 찾아 출력합니다.
  ```

## find

`find` 명령어는 파일 시스템에서 파일을 찾기 위해 사용하는 명령어 입니다.

### `find` 사용법

```bash
find [경로] [표현식]
```

- `[경로]`: 검색을 시작할 디렉토리를 지정(여러 경로 지정가능)
- `[표현식]`: 검색 조건과 검색 결과에 적용할 작업을 지정

- 표현식
    - `-name [파일명]`: 지정된 이름의 파일을 찾음
    - `-iname [파일명]`: `name`과 유사하지만, 대소문자를 구분하지 않음
    - `-type [d/f]`: `d`는 디렉토리, `f`는 일반 파일을 찾음
    - `-mtime [일수]`: 지정된 일수만큼 이전에 수정된 파일을 찾음
    - `-exec [명령] {} \\;`: 검색 결과에 대해 지정된 명령을 실행
- 예시
  1. 현재 디렉토리에서 파일 찾기
      ```bash
      find . -name example.txt
      ```

  2. 현재 디렉토리에서 대소문자를 구분하지 않고 파일 찾기

      ```bash
      find . -iname readme
      ```

  3. 현재 디렉토리에서 7일 이내에 수정된 일반 파일 찾기

      ```bash
      find . -type f -mtime -7
      ```

  4. 현재 디렉토리에서 파일이름 변경

      ```bash
      find . -name old -exec mv {} new \;
      ```

## which

`which` 명령어는 특정한 실행 가능 파일의 경로를 찾아주는 역할을 합니다.

### which 사용방법

```bash
which [명령어]
```

예시
1. 파이썬이 어디에 설치되어 있는지 출력
```bash
which python3 # 파이썬의 전체 경로
```

`gcc` 컴파일러가 어디에 설치되어 있는지 출력

```bash
which gcc # gcc 컴파일러의 전체 경로 출력
```

## whereis

`whereis`는 특정 바이너리, 소스, 또는 매뉴얼 페이지 파일의 위치를 찾을 때 사용되는 명령어입니다.

### whereis 사용법은

```shell
whereis [옵션] [파일명]
```

- `-b` : 바이너리 파일만 찾음
- `-m` : 매뉴얼 페이지만 찾음
- `-s` : 소스 파일만 찾음
- `-u` : 언급한 위치에서 파일을 찾지 못했을 때 사용
- 예시
    1. ls 명령어의 위치를 찾는 경우
  ```bash
  whereis ls
  ```

    2. python 바이너리 파일 위치를 찾는 경우
  ```bash
  whereis -b python
  ```

## 와일드카드

리눅스에서 와일드카드는 파일들을 일치시키는 데 사용되는 심볼이나 문자입니다. 가장 흔히 사용되는 와일드카드는 "*", "?", 그리고 "[]"입니다.

1. "*": 모든 문자를 대체할 수 있는 와일드카드입니다. 예를 들어, `*.txt`는 모든 txt 파일을 대상으로 합니다.
2. "?": 한 문자를 대체하는 와일드카드입니다. 예를 들어, `?.txt`는 한 문자로 된 모든 txt 파일을 대상으로 합니다.
3. "[]": 대괄호 안의 어떤 문자와도 일치하는 와일드카드입니다.

### 와일드카드 예시

1. `ls *.txt` 명령어는 모든 .txt 파일을 나열합니다.
2. `rm ?.jpg` 명령어는 한 문자로 된 이름을 가진 모든 .jpg 파일을 삭제합니다.
3. `cp [abc].txt new_directory/` 명령어는 a.txt, b.txt, c.txt 파일을 new_directory 디렉토리로 복사합니다.
