# Chap1. 알고리즘 분석


## Outline

1.1 실행시간

1.2 의사코드

1.3 실행시간 측정과 표기

1.4 전형적인 함수들의 증가율

1.5 알아야 할 수학적 배경

1.6 응용문제

---

### 알고리즘 분석

- 알고리즘 : 주어진 문제를 유한한 시간 내에 해결하는 단계적 절차
- 데이터구조 : 데이터를 조직하고 접근하는 체계적 방식

=⇒ "좋은" 알고리즘과 데이터구조를 설계하는 것이 목표

- "좋은"의 척도
    - 알고리즘과 데이터구조 작업에 소요되는 실행시간 ⇒ 시간복잡도
    - 기억장소 사용량 ⇒ 공간복잡도


### 실행시간(running time)

- 알고리즘은 입력을 출력으로 변환
- 알고리즘의 실행시간은 입력의 크기와 함께 성장
- 평균실행시간(average case running time)이 아닌 최악실행시간(worst case running time)에 집중 ⇒ 분석이 비교적 용이


### 실행시간 구하기: 실험적 방법 vs 이론적 방법

### 1. 실험적 방법

- 알고리즘을 구현하는 프로그램을 작성
- 시스템콜을 사용하여 실제 실행시간을 정확히 측정
- 결과를 도표로 작성

    ### 실험의 한계

    - 실험 결과는 실험에 포함되지 않은 입력에 대한 실행시간을 제대로 반영하지 않을 수도 있음
    - 두 알고리즘을 비교하기 위해선, 동일한 하드웨어와 소프트웨어 환경 필요
        - HW : processor, clock rate, memory, disk  ....
        - SW : OS, programming language, compiler  .....
    - 알고리즘을 완전한 프로그램으로 구현해야 함

### 2. 이론적 방법

- 모든 입력 가능성을 고려
- 하드웨어나 소프트웨어와 무관하게 알고리즘 속도 평가 가능
    - 실행시간을 입력 크기, n의 함수로 규정
- 알고리즘을 구현한 프로그램 대신, 고급언어인 의사코드로 표현된 알고리즘을 사용


---


### 알고리즘의 정의와 조건

- 입력(well defined input) : 모호하지 않고 잘 정의된 0개 이상의 입력
- 출력(well defined output) : 명확하게 정의된 1개 이상의 출력
- 명확성(clear and unambiguous) : 모호하지 않고 명확한 명령어
- 유한성(finite) : 한정된 수의 단계 후 반드시 종료. 무한루프(X)
- 유효성(feasible) : 명령어들은 현재 실행 가능한 연산이어야 함


### 알고리즘의 기술 방법

- 자연어
- 흐름도(Flowchart)
- 의사코드(pseudo-code)
- 프로그래밍 언어(C, Python...)


### 의사코드(pseudo-code)

: 알고리즘을 설명하기 위한 고급언어

- 컴퓨터가 아닌, 인간에게 읽히기 위해 작성됨
- 저급의 상세 구현 내용이 아닌, 고급 개념을 소통하기 위해 작성됨
- 자연어 문장보다 더 구조적, 프로그래밍 언어보다 덜 상세
- 알고리즘을 설명에 선호되는 표기법



---



### 임의접근기계 모델 (random access machine, RAM)

- 하나의 중앙처리(CPU)
- 무제한의 메모리셀(memory cell), 각각의 셀은 임의의 수나 문자 데이터를 저장
- 메모리셀들은 순번으로 나열되며, 어떤 셀에 대한 접근이라도 동일한(즉, 상수) 시간단위가 소요됨


### 원시작업 (primitive operation)

- 알고리즘에 의해 수행되는 기본적인 계산들
- 의사코드로 표현 가능
- 프로그래밍 언어와는 대체로 무관
- 정밀한 정의는 중요하지 않음
- 임의접근기계 모델에서 수행시 상수시간이 소요된다고 가정
- ex) 산술식/논리식의 평가(EXP), 변수에 특정값을 치환(ASS), 배열원소 접근(IND), 메소드 호출(CAL), 메소드로부터 반환(RET)


### 원시작업 수 세기

- 의사코드를 조사함으로써, 알고리즘에 의해 실행되는 원시작업의 최대 개수를 입력크기의 함수 형태로 결정할 수 있음


---

 

### 실행시간의 증가율

- 하드웨어나 소프트웨어 환경을 변경하면
    - T(n)에 상수 배수 만큼의 영향을 줌
    - T(n)의 증가율을 변경하지 않음

⇒ 선형의 증가율(growth rate)을 나타내는 실행시간 T(n)은 arrayMax의 고유한 속성


### Big-Oh와 증가율

- Big-Oh 표기법은 함수의 증가율의 상한(upper bound)을 나타냄
    - 주어진 두 개의 함수 f(n)와 g(n)에 관해, 만약 모든 정수 n≥n0에 대해 f(n)≤cg(n)가 성립하는 실수의 상수 c>0 및 정수의 상수 n0≥1가 존재하면 "f(n)=O(g(n))"
- "f(n) = O(g(n))" == "f(n)의 증가율은 g(n)의 증가율을 넘지 않음"
- 증가율에 따라 함수들을 서열화 가능


### Big-Oh 계산 요령

- f(n)이 상수이면 f(n) = O(1)
- f(n)이 차수 d의 다항식이면, f(n)=O(n^d)
- 최소한의 함수계열 사용
- 함수계열 중 가장 단순한 표현 사용


### 점근분석 (asymptotic analysis)

- 알고리즘을 점근분석함으로써 bog-oh 표기법에 의한 실행시간을 구할 수 있음
    - 1) 최악의 원시작업 수행회수를 입력크기의 함수로서 구한다
    - 2) 이 함수를 big-oh 표기법으로
- 상수계수와 낮은 차수의 항들은 결국 탈락되므로, 원시작업 수를 계산할 때부터 무시


### 분석의 지름길

- 다중의 원시작업
    - 하나의 식에 나타나는 여러 개의 원시작업을 하나로 계산

- 반복문
    - 반복문의 실행시간 * 반복횟수

- 중첩 반복문
    - 반복문의 실행시간 * ㅠ 각 반복문의 크기

- 연속문
    - 각 문의 실행시간을 합산, 이들 중 최대값 선택

- 조건문
    - 조건검사의 실행시간에 if-else 절의 실행시간 중 큰 것을 합산


### Big-Omega

- n≥n0 에 대해 f(n)≥cg(n)이 성립하는 상수 c>0 및 정수의 상수 n0≥1가 존재하면 "f(n)=Ω(g(n))"
- Big-Oh가 함수의 증가율의 상한(upper bound)을 나타내는데 반해, Big-Omega는 함수의 증가율의 하한(lower bound)을 나타냄


### Big-Theta

- n≥n0에 대해 c'g(n)≤f(n)≤c^n*g(n) 이 성립하는 상수 c'>0, c^n>0 및 정수의 상수 n0≥1가 존재하면 "f(n)=Θ(g(n))"
- "f(n)=O(g(n))"인 동시에 "f(n)=Ω(g(n))"이면, "f(n)=Θ(g(n))"
- Big-Theta는 함수의 증가율의 상한과 하한을 모두 나타내므로 동일함수를 나타냄



### 점근표기에 관한 직관

1. Big-Oh
    - 점근적으로 f(n)≤g(n)이면, "f(n)=O(g(n))"
2. Big-Omega
    - 점근적으로 f(n)≥g(n)이면, "f(n)=Ω(g(n))"
3. Big-Theta
    - 점근적으로 f(n)=g(n)이면, "f(n)=Θ(g(n))"

 

---



### 응용문제 1. 행렬에서 특정 원소 찾기

- n x n 배열 A의 원소들 중 특정 원소 x를 찾는 알고리즘 findMatrix를 작성하고자 한다.
- 알고리즘 findMatrix는 A의 행들을 반복하며, x를 찾거나 또는 A의 모든 행들에 대한 탐색을 마칠 때까지, 각 행에 대해 알고리즘 findRow를 호출한다.

A. 위에 의도한 바와 일치하도록 알고리즘 findMatrix를 의사코드로 작성

B. findMatrix의 최악실행시간 n을 구해라

C. 이는 선형시간 알고리즘인가? 왜 그런지 or 왜 아닌지 설명


```c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

#define ROWS 8
#define COLS 8

void makeArray(int A[][COLS])
{
    for(int r=0; r<ROWS; r++)
        for(int c=0; c<COLS; c++)
            A[r][c] = rand() % 100; 
}

void printArray(int A[][COLS])
{
    for(int r=0; r<ROWS; r++)
    {
        for(int c=0; c<COLS; c++)
            printf("%2d ", A[r][c]);
        printf("\n");
    }
    printf("\n");
}

int findRow(int A[], int key)
{
    for(int c=0; c<COLS; c++)
        if(A[c] == key)
            return c;
    return -1;
}

void findMatrix(int A[][COLS], int key)
{
    int r = 0;
    int index;
    
    while(r < ROWS)
    {
        index = findRow(A[r], key);
        if(index >= 0)
        {
            printf("%d행 %d열에서 %d 발견\n", r, index, key);
            return;
        }
        else
            r++;
    }
    printf("Not Found\n");
}

void main()
{
    int A[ROWS][COLS];
    srand(time(NULL)); // srand() 안해주면 매번 같은 값 발생
    makeArray(A);
    printArray(A);
    
    int key;
    printf("Input a key value : ");
    scanf("%d", &key);
    
    findMatrix(A, key);
}
```



### 2. 비트행렬에서 최대 1행 찾기 비트행렬에서 최대 1행 찾기

```c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

#define ROWS 8
#define COLS 8

void makeArray(int A[][COLS])
{
    for(int r=0; r<ROWS; r++)
    {
        int count = rand() % 8;
        
        for(int i = 0; i < count; i++)
            A[r][i] = 1;
        
        for(int j = count; j < COLS; j++)
            A[r][j] = 0;
    }
}

void printArray(int A[][COLS])
{
    for(int r=0; r<ROWS; r++)
    {
        for(int c=0; c<COLS; c++)
            printf("%2d ", A[r][c]);
        printf("\n");
    }
    printf("\n");
}

void mostOneButSlow(int A[][COLS]) 
{
    int jmax = 0;
    int i, j, row;
    
    for(i = 0; i < ROWS; i++)
    {
        j = 0;
        
        while(j < COLS && A[i][j] == 1)
            j++;
        
        if(j > jmax)
        {
            row = i;
            jmax = j;
        }
    }
    printf("%d행에 %d개의 1이 최대값임\n", row, jmax);
}

int mostOnes(int A[][COLS]) // 2차원 배열을 전달할 때 행은 생략해도 가능하지만 열 생략은 불가능. 둘 다 써도 됨
{ 
    int i = 0, j = 0;
    int row;
    
    while(1)
    {
        while(A[i][j] == 1)
        {
            j++;
            if(j == COLS-1)
                return i;
        }
        
        row = i;
        
        while(A[i][j] == 0)
        {
            i++;
            if(i == ROWS-1)
                return row;
        }
    }
}

void main()
{
    int A[ROWS][COLS];
    srand(time(NULL)); // srand() 안해주면 매번 같은 값 발생
    makeArray(A);
    printArray(A);
    //getchar();
    
    mostOneButSlow(A);  // O(n^2)
    
    printf("최대 1행은 %d행입니다.\n", mostOnes(A));  // O(n)
    
}
```



### 3. 누적평균

```c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

#define SIZE 8

void makeArray(int A[])
{
    for(int i = 0; i < SIZE; i++)
        A[i] = rand() % 50 + 50; // 50~99

}

void printArray(int A[])
{
    for(int i = 0; i < SIZE; i++)
        printf("[%d] ", A[i]);
    printf("\n");
}

void prefixAvg1(int A[])
{
    int X[SIZE];
    int sum;
    
    for(int i = 0; i < SIZE; i++)
    {
        sum = 0;
        
        for(int j = 0; j <= i; j++)
            sum += A[j];
        X[i] = sum / (i + 1);
    }
    
    printArray(X);
}

void prefixAvg2(int A[])
{
    int X[SIZE];
    int sum = 0;
    
    for(int i = 0; i < SIZE; i++)
    {
        sum += A[i];
        X[i] = sum / (i + 1);
    }
    
    printArray(X);
}

void main()
{
    int A[SIZE];
    srand(time(NULL));
    makeArray(A);
    printArray(A);
    
    prefixAvg1(A); // O(n^2)
    prefixAvg2(A); // O(n)
}
```