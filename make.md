# make
```
The purpose of the make utility is to determine automatically which pieces of a large program need to be recompiled, and issue the commands to recompile them.
소스 한 두개로 이루어진 C/C++이 아니라면 gcc로 관리하는 것은 매우 어려움
효율적으로 관리하고 일관성있게 관리하기 위해 Makefile이라는 형식을 사용하고 make라는 유틸리티를 사용함
make라는 유틸리티는 보통 현재 디렉토리에 있는 makefile이라는 일정한 규칙을 준수하여 만든 파일의 내욜을 읽어서 목표파일을 만들어 낸다

Makefile 만듦기
$ vi Makefile
------------------------------------------------------
all: foo
foo: foo.o bar.o 
    gcc -o foo foo.o bar.o

foo.o: foo.c
    gcc -c foo.c

bar.o: bar.c
    gcc -c bar.c

.c.o:
    gcc -c ${CFLAGS} $<
    // .c를 입력받고 .o파일을 만듦

clean: 
    rm -f foo foo.o bar.o

목표(Target): 목표를 만드는데 필요한 구성요소들(Dependancy, 반드시 공백으로 구분)...
    목표를 만들기 위한 명령1(Command, 반드시 맨 앞에 TAB만 추가할 것)
    목표를 만들기 위한 명령1
    ...
------------------------------------------------------
    다 만들고 난 후

$ make
또는
$ make foo
    make는 변화를 감지해서 변화가 있다면 삭제하고 다시만듦

$ make clean
    Makefile에 지정해둔 명령어가 실행되어 파일이 삭제됨


예제
------------------------------------------------------
OBJS = main.o read.o write.o // 매크로 지정(Macro makes Makefile happy)

test : $(OBJS)
gcc -o test $(OBJS)
------------------------------------------------------

예제
------------------------------------------------------
OBJECTS = main.o read.o write.o // 매크로
SRCS = main.c read.c write.c // 없어도 됨

CC = gcc // gcc 로 세팅
CFLAGS = -g -c // gcc 의 옵션에 -g 추가

TARGET = test // 매크로

$(TARGET): $(OBJECTS)
    $(CC) -o $(TARGET) $(OBJECTS)

clean : // 레이블
    rm -rf $(OBJECTS) $(TARGET) core 

main.o : io.h main.c <- (1)
read.o : io.h read.c
write.o: io.h write.c
------------------------------------------------------

예제
------------------------------------------------------
.SUFFIXES : .c .o 
// 확장자 규칙
// 파일의 확장자를 보고 그에 따라 적절한 연산을 수행시키는 규칙
// .SUFFIXESS를 입력하면 make파일에게 주의 깊게 처리할 파일들의 확장자를 등록해 준다고 이해하면 될 것임
// 아래의 루틴이 자동적으로 실행됨
// .c.o : 
//     $(CC) $(CFLAGS) -c $< -o $@

OBJECTS = main.o read.o write.o
SRCS = main.c read.c write.c

CC = gcc 
CFLAGS = -g -c

TARGET = test

$(TARGET) : $(OBJECTS)
    $(CC) -o $(TARGET) $(OBJECTS)

clean : 
    rm -rf $(OBJECTS) $(TARGET) core 

main.o : io.h main.c 
read.o : io.h read.c
write.o: io.h write.c
------------------------------------------------------

에제
------------------------------------------------------
.SUFFIXES : .c .o 

OBJECTS = main.o read.o write.o
SRCS = main.c read.c write.c

CC = gcc 
CFLAGS = -g -c 
INC = -I/home/raxis/include # include 패스 추가

TARGET = test

$(TARGET) : $(OBJECTS)
                $(CC) -o $(TARGET) $(OBJECTS)

.c.o : # 우리가 확장자 규칙을 구현
    $(CC) $(INC) $(CFLAGS) $<-

clean : 
    rm -rf $(OBJECTS) $(TARGET) core 

main.o : io.h main.c
read.o : io.h read.c
write.o : io.h write.c
------------------------------------------------------

유용한 문법
------------------------------------------------------
줄바꿈
OBJS = main.o \n
read.o\n
write.o

매크로 부분 수정하기
OBJS = main.o read.o write.o
SRCS = $(OBJS:.o=.c) 
// $(MACRO_NAME:OLD=NEW)
// 내용을 일부만 수정하고 싶을경우 사용
// SRCS는 main.c read.c write.c가 된다
------------------------------------------------------

옵션들
-C dir: 위에서도 밝혔듯이 Makefile을 계속 읽지 말고 우선은 dir로 이동하라는 것이다. 순환 make에 사용된다.
-d: Makefile을 수행하면서 각종 정보를 모조리 출력해 준다. (-debug) 출력량이 장난이 아님... 결과를 파일로 저장해서 읽어보면 make 의 동작을 대충 이해할 수 있다.
-h: 옵션에 관한 도움말을 출력한다. (-help)
-f file: file 에 해당하는 파일을 Makefile로써 취급한다. (-file)
-r: 내장하고 있는 각종 규칙(Suffix rule 등)을 없는 것으로 (-no-builtin-rules)간주한다. 따라서 사용자가 규칙을 새롭게 정의해 주어야 한다.
-t: 파일의 생성 날짜를 현재 시간으로 갱신한다. (-touch)
-v: make의 버전을 출력한다. (전 GNU make 3.73 을 씁니다.) (-version)
-p: make에서 내부적으로 세팅되어 있는 값들을 출력한다. (-print-data-base)
-k: make는 에러가 발생하면 도중에 실행을 포기하게 되는데 (-keep-going) -k 는 에러가 나더라도 멈추지 말고 계속 진행하라는 뜻

내부 매크로
$@: 현재 목표 파일의 이름
$*: 확장자를 제외한 현재 목표 파일 이름
$<: 현재 필수 조건 파일 중 첫 번째 파일 이름
$?: 현재 대상보다 최근에 변경된 필수 조건 파일 ㅣㅇ름
$^: 현재 모든 필수 조건 파일들
```