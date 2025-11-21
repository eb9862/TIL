# 쉘 명령어 간단 정리

## 목차

- [파일 및 디렉토리 관리](#파일-및-디렉토리-관리)
  - [ls](#ls)
  - [pwd](#pwd)
  - [cd](#cd)
  - [mkdir](#mkdir)
  - [cp](#cp)
  - [mv](#mv)
  - [rm](#rm)
  - [find](#find)
  - [tree](#tree)
- [파일 내용 보기 및 편집](#파일-내용-보기-및-편집)
  - [cat](#cat)
  - [head / tail](#head--tail)
  - [vi(m)](#vim)
- [텍스트 처리 및 검색](#텍스트-처리-및-검색)
  - [grep](#grep)
  - [sort](#sort)
  - [uniq](#uniq)
  - [cut](#cut)
  - [awk](#awk)
- [프로세스 및 시스템 정보](#프로세스-및-시스템-정보)
  - [ps](#ps)
  - [df](#df)
  - [history](#history)
- [네트워크](#네트워크)
  - [curl](#curl)
  - [ssh](#ssh)
  - [scp](#scp)
- [권한 및 환경](#권한-및-환경)
  - [sudo](#sudo)
  - [chmod](#chmod)
  - [export](#export)
  - [alias](#alias)
- [명령어 및 스크립트 실행](#명령어-및-스크립트-실행)
  - [man](#man)
  - [echo](#echo)
  - [clear](#clear)
  - [bash](#bash)
  - [python](#python)
  - [nohup](#nohup)
- [입출력 리디렉션 및 파이프](#입출력-리디렉션-및-파이프)
  - [Redirection \& Pipe](#redirection--pipe)

---

## 파일 및 디렉토리 관리

### ls
- `list`의 약자로, 현재 위치한 디렉토리의 파일 및 폴더 목록을 보여주는 명령어
- 주요 옵션
  - `-l` : 상세 정보(권한, 소유자, 크기, 날짜)
  - `-a` : 숨김 파일 포함
  - `-h` : 파일 크기를 사람이 읽기 쉬운 형태로
  - `-t` : 수정된 시간 순으로 정렬
```bash
# 현재 디렉토리의 모든 파일 및 폴더(숨김 파일 포함)를 자세한 정보와 함께 보여줌
ls -al

# 수정된 시간 순으로 정렬하여 보여줌
ls -alt
```

### pwd
- `print working directory`의 약자로, 현재 작업중인 디렉토리의 절대 경로를 출력해주는 명령어
```bash
# 현재 작업 디렉토리의 경로를 출력
pwd
```

### cd
- `change directory`의 약자로, 다른 디렉토리로 이동할 때 사용하는 명령어
```bash
cd /path/to/directory # /path/to/directory 경로로 이동
cd .. # 이전 폴더로 이동
cd ~ # 홈으로 이동
```

### mkdir
- `make directory`의 약자로, 새로운 디렉토리(폴더)를 생성하는 명령어
```bash
mkdir new_folder # new_folder라는 이름의 새 폴더를 생성
mkdir -p project/src/assets # -p 옵션으로 중첩 폴더 생성
```

### cp
- `copy`의 약자로, 파일이나 디렉토리를 복사하는 명령어
```bash
cp source.txt destination.txt # source.txt 파일을 destination.txt 파일로 복사
cp -r src backup/ # -r 옵션으로 폴더 복사
```

### mv
- `move`의 약자로, 파일이나 디렉토리를 다른 위치로 옮기거나 이름을 변경
```bash
# old_name.txt 파일의 이름을 new_name.txt로 변경
mv old_name.txt new_name.txt

# file.txt를 /backup 폴더로 이동
mv file.txt /backup/
```

### rm
- `remove`의 약자로, 파일이나 디렉토리를 삭제하는 명령어
- 주요 옵션
  - `-r` : 디렉토리와 그 안의 내용들을 함께 삭제
  - `-f` : 강제로 삭제
  - `-i` : 삭제 전 확인 메시지 출력
```bash
# file.txt 파일을 삭제 (삭제 전 확인)
rm -i file.txt

# directory 디렉토리를 강제로 삭제
rm -rf directory
```

### find
- 파일 시스템에서 파일이나 디렉토리를 검색하는 명령어
- 주요 옵션
  - `-name` : 이름으로 검색
  - `-type` : 파일 타입으로 검색 (f: 파일, d: 디렉토리)
  - `-mtime` : 수정된 시간으로 검색 (n일 전)
```bash
# 현재 디렉토리에서 .log로 끝나는 파일을 검색
find . -name "*.log"

# /home 디렉토리에서 'user1' 소유의 파일을 검색
find /home -user user1
```

### tree
- 현재 디렉토리의 모든 하위 디렉토리와 파일들을 트리 구조로 보여주는 명령어
```bash
tree
tree -L 2 # 현재 디렉토리부터 2단계 깊이까지의 구조를 트리 형태로 보여줌
```

## 파일 내용 보기 및 편집

### cat
- `concatenate`의 약자로, 파일의 내용을 터미널에 출력하거나 여러 파일을 합치는 데 사용
- 주요 옵션
  - `-n` : 각 행의 앞에 번호를 붙여서 출력
```bash
# file.txt의 내용을 행 번호와 함께 출력
cat -n file.txt

# file1.txt와 file2.txt의 내용을 합쳐 new_file.txt를 생성
cat file1.txt file2.txt > new_file.txt
```

### head / tail
- `head`: 파일의 앞부분 몇 줄을 출력하는 명령어
- `tail`: 파일의 뒷부분 몇 줄을 출력하는 명령어
- 주요 옵션
  - `-n` : 출력할 줄의 수 지정
  - `-f` : (tail 전용) 파일의 마지막 부분을 실시간으로 계속 출력
```bash
# file.txt의 첫 5줄을 출력
head -n 5 file.txt

# file.txt의 마지막 3줄을 출력
tail -n 3 file.txt

# log.txt 파일의 변경사항을 실시간으로 확인
tail -f log.txt
```

### vi(m)
- 터미널 환경에서 사용 가능한 편집기(editor)
```bash
# filename.txt 파일을 vi 편집기에서 열기
vi filename.txt
```

## 텍스트 처리 및 검색

### grep
- 파일이나 표준 입력에서 특정 패턴(문자열, 정규표현식)을 포함하는 행을 찾아 출력하는 명령어
- 주요 옵션
  - `-i` : 대소문자 구분 없이 검색
  - `-r` : 하위 디렉토리까지 재귀적으로 검색
  - `-v` : 패턴과 일치하지 않는 행을 출력
  - `-n` : 행 번호를 함께 출력
```bash
# log.txt 파일에서 "error"라는 단어가 포함된 모든 행을 대소문자 구분 없이 출력
grep -i "error" log.txt

# 현재 디렉토리의 모든 파일에서 "hello"를 재귀적으로 검색
grep -r "hello" .
```

### sort
- 파일의 내용을 행 단위로 정렬하는 명령어
- 주요 옵션
  - `-r` : 내림차순으로 정렬
  - `-n` : 숫자 순서로 정렬
  - `-k` : 특정 필드를 기준으로 정렬
```bash
# numbers.txt 파일을 숫자 순으로 정렬
sort -n numbers.txt

# data.csv 파일의 3번째 필드를 기준으로 정렬
sort -t',' -k3 data.csv
```

### uniq
- `unique`의 약자로, 정렬된 파일에서 중복된 행을 제거하거나 중복된 행만 출력하는 명령어
- 주요 옵션
  - `-c` : 각 행의 중복 횟수를 함께 출력
  - `-d` : 중복된 행만 출력
```bash
# data.txt에서 중복된 행의 횟수를 세어 출력
sort data.txt | uniq -c
```

### cut
- 파일의 각 행에서 특정 필드(열)나 문자들을 잘라내어 출력하는 명령어
- 주요 옵션
  - `-d` : 구분자 지정
  - `-f` : 필드 선택
  - `-c` : 문자 단위 선택
```bash
# 쉼표(,)로 구분된 data.csv 파일에서 첫 번째 필드를 잘라내어 출력
cut -d',' -f1 data.csv
```

### awk
- 파일이나 표준 입력으로부터 텍스트를 한 줄씩 읽어, 특정 패턴을 찾고 그에 맞는 동작을 수행하는 강력한 텍스트 처리 도구
- 주요 옵션 / 기능
  - `-F` : 구분자 설정
  - `$1`, `$2`, ... : 컬럼 인덱스
```bash
# file.txt의 각 행에서 첫 번째 필드(공백으로 구분)를 출력
awk '{print $1}' file.txt

# log.txt에서 "ERROR"가 포함된 행의 3번째 필드를 출력
awk '/ERROR/ {print $3}' log.txt
```

## 프로세스 및 시스템 정보

### ps
- `process status`의 약자로, 현재 실행 중인 프로세스들의 상태를 보여주는 명령어
- 주요 옵션
  - `aux` : 모든 사용자의 모든 프로세스를 자세히 보여줌
  - `-ef` : 모든 프로세스를 풀 포맷으로 보여줌
```bash
# 모든 사용자의 프로세스를 자세한 정보와 함께 보여줌
ps aux

# 'python'이라는 이름의 프로세스를 필터링
ps aux | grep 'python'
```

### df
- `disk free`의 약자로, 파일 시스템의 디스크 공간 사용량을 보여주는 명령어
- 주요 옵션
  - `-h` : 사람이 읽기 쉬운 형태(GB, MB)로 출력
  - `-T` : 파일 시스템 타입을 함께 출력
```bash
# 디스크 공간 사용량을 사람이 읽기 쉬운 형태와 파일 시스템 타입과 함께 보여줌
df -hT
```

### history
- 이전에 사용했던 명령어들의 목록을 보여주는 명령어
```bash
history 10 # 최근에 사용한 명령어 10개를 보여줌
!100 # 100번째 명령어 재실행
```

## 네트워크

### curl
- `Client for URLs`의 약자로, 다양한 프로토콜을 사용하여 URL로부터 데이터를 전송하거나 가져오는 명령어
- 주요 옵션
  - `-O` : 원격 파일 이름 그대로 저장
  - `-L` : 리다이렉트가 있을 경우 따라감
  - `-X` : HTTP 요청 메소드 지정 (GET, POST 등)
  - `-H` : 요청 헤더 지정
  - `-d` : POST 요청 시 데이터 지정
```bash
# example.com에서 file.zip을 다운로드하여 현재 디렉토리에 저장
curl -O https://example.com/file.zip

# JSON 데이터를 POST 요청으로 전송
curl -X POST -H "Content-Type: application/json" -d '{"name":"test"}' https://api.example.com/users
```

### ssh
- `Secure Shell`의 약자로, 원격 컴퓨터에 안전하게 접속하고 명령을 실행하는 프로토콜 또는 그 프로그램을 지칭
```bash
# 'user'라는 사용자로 'hostname'이라는 원격 서버에 접속
ssh user@hostname

# 특정 포트(2222)로 접속
ssh -p 2222 user@hostname
```

### scp
- `secure copy`의 약자로, SSH 프로토콜을 기반으로 원격 호스트와 파일을 안전하게 복사하는 명령어
```bash
# 로컬의 file.txt를 'hostname' 서버의 /remote/path/ 디렉토리로 복사
scp file.txt user@hostname:/remote/path/

# 원격 서버의 파일을 로컬로 복사
scp user@hostname:/remote/path/file.txt .
```

## 권한 및 환경

### sudo
- `substitute user do` 또는 `superuser do`의 약자로, 다른 사용자(기본적으로 root)의 권한으로 명령어를 실행
```bash
# root 권한으로 패키지 목록을 업데이트 (Debian/Ubuntu 계열)
sudo apt-get update
```

### chmod
- `change mode`의 약자로, 파일이나 디렉토리의 접근 권한(읽기, 쓰기, 실행)을 변경하는 명령어
```bash
# script.sh 파일에 소유자는 읽기/쓰기/실행, 그룹과 다른 사용자는 읽기/실행 권한을 부여 (숫자 방식)
chmod 755 script.sh

# user에게 실행 권한 추가 (기호 방식)
chmod u+x script.sh
```

### export
- 환경 변수를 설정하여 현재 쉘 세션과 자식 프로세스에서 사용할 수 있게 하는 명령어
```bash
# MY_VAR이라는 환경 변수에 "my_value"라는 값을 설정
export MY_VAR="my_value"
```

### alias
- 기존의 명령어를 조합하여 새로운 명령어를 만들거나, 긴 명령어를 짧게 줄여 별칭(alias)으로 사용하는 명령어
```bash
# 'ls -al' 명령어를 'll'이라는 별칭으로 지정
alias ll='ls -al'
```

## 명령어 및 스크립트 실행

### man
- `manual`의 약자로, 터미널에서 사용되는 명령어의 매뉴얼을 출력해주는 명령어
```bash
man ls # ls 명령어의 매뉴얼을 확인
```

### echo
- 터미널 화면에 텍스트를 출력하는 명령어
```bash
echo "Hello, World!"
```

### clear
- 터미널 화면을 깨끗하게 지우는 명령어
```bash
clear
```

### bash
- `Bourne-Again SHell`의 약자로, 쉘 스크립트를 실행하거나 상호작용적인 커맨드 라인 인터페이스를 제공
```bash
# script.sh 라는 쉘 스크립트 파일을 실행
bash script.sh
```

### python
- 파이썬 인터프리터를 실행하여 파이썬 코드를 실행하거나 대화형 세션을 시작하는 명령어
```bash
# my_app.py 파이썬 스크립트를 실행
python my_app.py
```

### nohup
- `no hang up`의 약자로, 터미널 세션이 끊어져도 프로세스가 계속 실행되도록 하는 명령어
```bash
# 터미널이 종료되어도 my_script.py를 계속 실행 (백그라운드 실행)
nohup python my_script.py &
```

## 입출력 리디렉션 및 파이프

### Redirection & Pipe
- `>`: 표준 출력을 파일로 리디렉션 (덮어쓰기)
- `>>`: 표준 출력을 파일로 리디렉션 (이어쓰기)
- `<`: 파일의 내용을 표준 입력으로 리디렉션
- `|`: 한 명령어의 표준 출력을 다른 명령어의 표준 입력으로 연결
```bash
# ls -l의 결과를 file_list.txt에 덮어쓰기
ls -l > file_list.txt

# new_line.txt의 내용을 file_list.txt에 이어쓰기
cat new_line.txt >> file_list.txt

# file_list.txt의 내용을 정렬하여 출력
sort < file_list.txt

# ls 명령어의 결과 중에서 ".txt"를 포함하는 라인만 필터링
ls | grep ".txt"
```
