Download Link :https://programming.engineering/product/cse4100-multicore-programming/


# CSE4100-Multicore-Programming
CSE4100: Multicore Programming
배경지식

Network programming

Echo 서버 프로그래밍

동시 주식 서버 (Concurrent Stock Server) 설계 및 구현 (각 30점 + 보고서 10점)

Task1: Event-driven Approach

Task2: Thread-based Approach

Task3: 성능 평가 및 분석

제출 방법

부록

프로젝트 목표

“여러 client들의 동시 접속 및 서비스를 위한 Concurrent stock server 을 구축”

주식 서버

주식 정보를 저장하고 있고 여러 client들과 통신하여, 주식 정보 List, 판매, 구매의 동작을 수행

주식 클라이언트

각 client는 server에 주식 사기, 팔기, 가격과 재고 조회 등의 요청을 함

배경지식: Echo Server Program

echoclient 와 echoserveri와 사이의 통신(예제)

client server

주식 서버 “Client” 동작 설명

동시 주식 서버 (Concurrent Stock Server) 필요성

주식 서버 설계 (3)

• 효율적인 데이터 관리를 위해서 Binary Tree 사용

• 트리의 각 노드는 <ID, 잔여 주식, 주식 단가, mutex lock 변수> 등을 선언 (뒤 페이지 그림 참고) • 주식 ID는 1~MAX에 random unique id로 발급

struct item{

int ID;

주식 종목 (item)

int left_stock;

int price;

int readcnt;

sem_t mutex;

};<예: 노드구조>

Readers-Writers Problem

어떤 client가 i 종목 주식 읽을 때, 다른 client가 i 값을 update하게 되면 올바르게 동작 X

Readers-writers problem solution을 고려한 노드 단위의 관리 (fine-grained locking) 필요

Task1: Event-driven Approach (30점)

client

client

client

client

client

client

• 각각의 client는 fd를 trigger하고, 서버는 fd들을 monitoring하던 select에 의해서 동작을 시작함.

Server 시작과 함께 table(file)의 내용을 읽어 메모리에 적재 후, 요청을 처리

Server의 종료 시 업데이트 된 변경사항을 파일에 정해진 형식에 따라 저장

. . .

struct item{

int ID;

int left_stock;

int price;

int readcnt;

sem_t mutex;

};

<Example structure>

11

Task2: Thread-based Approach (30점)

Thread-based Concurrent Stock Server

Pool of

worker threads

Task3: 성능 평가 및 분석 (30점)

성능 평가

Client 실행파일 내에 configuration들을 바꿔가면서 두 가지 동작 방식(event-driven, thread)의 elapse time을 측정하여 분석

이 경우에 한해서는 client process의 개수를 제한하지 않고 10, 20개 이상 띄우면서 실험 가능 (Task 1-2 수행 시에는 4개 이하로 진행할 것!)

분석 내용은 보고서에 최대한 상세히 기재

제출 방법

프로젝트 주의 사항

Process Management에 유의해주세요

ps -aux 명령어 혹은 top 명령어를 통해 백그라운드로 남아 있는 프로세스를 수시로 확인해주세요

백그라운드 프로세스가 죽지 않거나 서버가 동작하지 않을 경우 조교 이메일로 곧장 연락주세요

(1분반_정경환 조교 : siblue202@gmail.com, 2분반_박규리 조교 : kyu99park@gmail.com )

• 각 분반에 맞는 실습 서버에서 실습을 진행해주세요!

1분반은 Client(cspro), Server(cspro9)서버에서 실습

2분반은 Client(cspro), Server(cspro10)서버에서 실습

• 제출 파일 양식 우측 이미지 참조

task_3의 경우에는 보고서에 해당 내용 실행 결과 캡처와 함께 자세히 기재


부록: 추가 설명

Multiclient 실행 파일(1)

Multiclient 실행 파일(2)

• 앞서 말한 #define configuration을 적절히 설정 후 실행

stockserver를 먼저 실행하고 (./stockserver 1119), 아래와 같이 multiclient를 실행

./multiclient를 입력하면 사용법이 나옴 (stockclient 실행 방법과 비슷함)

./multiclient [IP_addr] [port #] [client_num[ (ex. ./multiclient 172.30.10.10 6xxxx 4) 아래와 같이 4개의 client process가 configuration에 따라 적절한 동작을 수행

실험 시에는 client process를 4개 이하로 띄워서 실험할 것!

zombie나 orphan이 발생하지 않도록 주의할 것!

child process4개 생성 

Multiclient 실행 파일(3)

client 1 request

client 2 request

client 3 request

client 4 request

서버 구현 상세 (1)

서버 구현 상세 (2)

Response 양식 (오른쪽 그림 참고)

[buy] success : buy request가 정상적으로 처리되었을 경우

[sell] success : sell request가 정상적으로 처리되었을 경우

Not enough left stocks : buy request에 대해서 잔여 주식이 부족한 경우

Recommendation : 이때는 client가 아닌 stockclient를 실행시키고 command를

하나씩 입력하고 제대로 처리되는지 확인하면서 진행

Persistency를 위해서 connection이 모두 종료되면 stock.txt 에 update

내용이 반영되어야 함

stock.txt에 update 내용이 제대로 반영되었는지 확인함으로써 채점을 진행할 예정
