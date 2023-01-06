# gcc
[gcc](https://wiki.kldp.org/KoreanDoc/html/gcc_and_make/gcc_and_make-2.html)   
```
기본
$ gcc helloworld.c
    기본 컴파일 명령어, 오류가 없다면 helloworld.out이라는 파일이 생성됨

$ gcc -o helloworld helloworld.c 
$ gcc helloworld.c -o hellworld // 상동
    출력 파일명을 지정할 수 있음, hellworld이라픈 파일이 생성됨
    파일 싱행은 ./helloworld

$ gcc -c helloworld.c 
    단지 컴파일만 하고싶을 때(링크X), helloworld.o라는 목적파일만 생성됨
    중요한게 테스트한다고 
    $ gcc -o test hellworld.c 하지말고 
    $ gcc -c helloworld.c 로 해서 테스트


소스가 여러개 있을경우 컴파일 방법
$ gcc -o baz foo.c bar.c // 방법1
$ ./baz
$ gcc -c foo.c bar.c     // 방법2
$ gcc -o baz foo.o bar.o
$ ./baz
    gcc는 컴파일하는 놈이 아님
    .c 파일을 던져주면 컴파일을하고
    .o 파일을 던져주면 링크를 함
    단순히 컴파일러와 링커를 호출하는 역할을 해줌

옵션 -I(대문자 i)
: 헤더파일 위치 지정, 유닉스에 있어 포준 헤더 파일 디렉토리는 "/usr/include"
$ gcc -c helloworld.c -I.. -Iinclude
    -I<헤더파일의 위치>
    헤더파일을 하위폴더(..)에서 찾거나 현재 디렉토리에 include라는 디렉토리에서 찾음
    여러번 사용 가능 순서대로 검색, 띄어쓰기 X(-I include)

옵션 -l(소문자 L)과 -L(대문자)
: 라이브러리 관련 옵션
라이브러리를 보통 libxxx.a라고 만드는데 링크할 라이브러리를 명시해줄 때 접두사(lib)과 접미사(.a)를 떼어주고 명시함
$ gcc -o hellworld hellworld.c -lmylib -L.
    -l<접두사접미사를 제외한 라이브러리명>
    접두시 접미사를 제외하고 -l옵션을 써줌 -I와 마찬가지로 띄어쓰기는 X
    -L 옵션은 명시한 라이브러리파일을 어느 디렉토리에서 찾을건지 명시해주는 옵션
    -L<디렉토리명>, 여러번 쓸 수 있다
```
# ar
```
컴파일된 오브젝트 파일들이 하나의 아카이브로 묶여있는 형태, 오브젝트파일을 하나로 묶어줌
기본
$ ar [라이브러리명] [오브젝트 파일들...]

옵션
r : 새로운 오브젝트 파일이면 추가, 기존 파일이면 치환
c : 아카이브(라이브러리 파일) 생성, 존재하지 않는 아카이브를 작성(또는 갱신)하는 경우에도 경고 메시지를 출력하지 않음
u : 오브젝트 파일의 타임스탬프를 비교해 새로운 파일일 경우에만 치환
s : ranlib(1)과 마찬가지로 아카이브 인덱스 생성. 아카이브 인덱스를 생성하지 않으면 링크 속도가 느려지고, 시스템 환경에 따라 에러가 발생할 수도 있음
d : 아카이브 모듈을 삭제, 삭제할 파일이 없다면 동작하지 않음
t : 아카이브에 있는 파일 리스트 출력
v : 자세한 내용을 보여주는 verbose 모드
x : 아카이브에서 오브젝트 파일 추출

예제
------------------------------------------------------
$ gcc -c myfunc.c
$ ar rst libmylib.a myfunc.o
$ gcc -o helloworld helloworld.c -lmylib -L.
------------------------------------------------------
```