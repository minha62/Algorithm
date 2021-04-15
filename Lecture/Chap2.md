### Outline

1. 재귀 알고리즘
2. 재귀의 작동원리
3. 재귀의 기본 규칙
4. 응용문제


---



### 분할정복

- 비재귀적(nonrecursive) or 반복적(iterative) 알고리즘
- 재귀적(recursive) or 분할정복(divide and conquer) 알고리즘



### 재귀 알고리즘

- 알고리즘 자신을 사용하여 정의된 알고리즘
- ↔ 비재귀적, 반복적
- 재귀의 요소
    - 재귀 케이스(recursion):
        - 차후의 재귀호출은 작아진 부문제들(subproblems)을 대상으로 이루어짐
    - 베이스 케이스(base case):
        - 부문제들이 충분히 작아지면, 알고리즘은 재귀를 사용하지 않고 직접 해결



### 작동 원리

- 보류된 재귀호출(즉, 시작했지만 완료하지 않고 대기중인 호출들(을 위한 변수들에 관련된 저장/복구는 컴퓨터에 의해 자동적으로 수행됨



### 기본 규칙

- 베이스 케이스
    - 베이스 케이스를 항상 가져야 하며, 이는 재귀 없이 해결될 수 있어야 함

- 진행 방향
    - 재귀적으로 해결되어야 할 경우, 재귀호출은 항상 베이스 케이스를 향하는 방향으로 진행되어야 함

- 정상작동 가정
    - 모든 재귀호출이 제대로 작동한다고 가정

- 적절한 사용
    - 꼭 필요할 때만 사용. bc 저장/복구 때문에 성능이 저하됨



### 나쁜 재귀

- 잘못 설계된 재귀
    - 베이스 케이스가 없거나 도달 불능
    - 재귀 케이스가 베이스 케이스를 향해 작아진 부문제를 대상으로 재귀하지 않음

- 나쁜 재귀 사용의 영향
    - 부정확한 결과
    - 미정지(nontermination)
    - 저장을 위한 기억장소 고갈
 
 
 
 ```c
#include <stdio.h>
#include <stdlib.h>

void rPrintDigits(int n)
{
    if(n < 10)
        printf("%d\n", n);
    else
    {
        rPrintDigits(n / 10);
        printf("%d\n", n % 10);
    }
}

void printDigits()
{
    int n;
    
    printf("Enter a number : ");
    scanf("%d", &n);
    
    if(n < 0)
        printf("Negative!\n");
    else
        rPrintDigits(n);
}

void main()
{
    printDigits();
}
```
