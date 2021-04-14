### 퀵정렬
```c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

#define MAX_SIZE 15
#define SWAP(x, y, t) ((t) = (x), (x) = (y), (y) = (t))

int partition(int list[], int left, int right)
{
    int pivot, temp, low, high;
    
    low = left;
    high = right + 1;
    pivot = list[left];
    
    do
    {
        do
            low++;
        while(list[low] < pivot);
        
        do
            high--;
        while(list[high] > pivot);
        
        for(int i = 0; i < MAX_SIZE; i++)
            printf("[%d] ", list[i]);
        printf("\nlow = %d, high = %d\n", low, high);
        
        if(low < high)
            SWAP(list[low], list[high], temp);
    }while(low < high);
    
    SWAP(list[left], list[high], temp);
    
    return high;
}

void quick_sort(int list[], int left, int right)
{
    if(left < right)
    {
        int q = partition(list, left, right);
        
        quick_sort(list, left, q - 1);
        quick_sort(list, q + 1, right);
    }
}


void main()
{
    int list[MAX_SIZE];
    srand(time(NULL));
    
    for(int i = 0; i < MAX_SIZE; i++)
        list[i] = rand() % 100;
    
    for(int i = 0; i < MAX_SIZE; i++)
        printf("[%d] ", list[i]);
    printf("\n\n");
    
    quick_sort(list, 0, MAX_SIZE - 1);
    
    for(int i = 0; i < MAX_SIZE; i++)
    {
        printf("[%d] ", list[i]);
        //Sleep(500);
    }
    printf("\n");
}
```

### 무작위 & 제자리 퀵정렬
```c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

#define MAX_SIZE 15
#define SWAP(x, y, t) ((t) = (x), (x) = (y), (y) = (t))

int partition(int list[], int left, int right, int k)
{
    int pivot, temp, low, high;
    
    pivot = list[k];
    SWAP(list[k], list[right], temp);
    
    printf("Pivot = %d\n", pivot);
    
    for(int i = 0; i < MAX_SIZE; i++)
        printf("[%d] ", list[i]);
    printf("\n\n");
    
    low = left - 1;
    high = right;
    
    do
    {
        do
            low++;
        while(list[low] < pivot);
        
        do
            high--;
        while(list[high] > pivot);
        
        //for(int i = 0; i < MAX_SIZE; i++)
        //    printf("[%d] ", list[i]);
        //printf("\nlow = %d, high = %d\n", low, high);
        
        if(low < high)
            SWAP(list[low], list[high], temp);
    }while(low < high);
    
    SWAP(list[low], list[right], temp);
    
    return low;
}

void quick_sort(int list[], int left, int right)
{
    if(left < right)
    {
        int k = rand() % (right - left) + left + 1;
        int q = partition(list, left, right, k);
        
        quick_sort(list, left, q - 1);
        quick_sort(list, q + 1, right);
    }
}


void main()
{
    int list[MAX_SIZE];
    srand(time(NULL));
    
    for(int i = 0; i < MAX_SIZE; i++)
        list[i] = rand() % 100;
    
    for(int i = 0; i < MAX_SIZE; i++)
        printf("[%d] ", list[i]);
    printf("\n\n");
    
    quick_sort(list, 0, MAX_SIZE - 1);
    
    for(int i = 0; i < MAX_SIZE; i++)
        printf("[%d] ", list[i]);
    
    printf("\n");
}
```

### 응용문제 : 색 분리
n개의 원소로 이루어진 리스트 L이 있다 - 여기서 원소는 각각 빨강 or 파랑으로 색칠되어 있다
L이 배열로 표현되었다고 전제하고, L의 모든 파랑 원소들이 빨강 원소들의 앞에 오도록 재배치하는 제자리, O(n)-시간메소드를 설명하라.

sol) 빨강과 파랑 원소에 대해 다음을 수행
1. 배열의 왼쪽과 오른쪽 양쪽 끝에서 각각 포인터 출발
2. 왼쪽 포인터가 파랑 원소를 가리키는 동안 첨자를 계속 증가
3. 마찬가지로, 오른쪽 포인터가 빨강 원소를 가리키는 동안 점자를 계속 감소
4. 왼쪽 포인터가 빨강 원소에 이르고 오른쪽 포인터가 파랑 원소에 이르면, 두 원소를 맞교환
5. 두 포인터가 만날 때까지 포인터를 계속 진행하면서 원소를 맞교환
6. 두 포인터가 만난 시점에 배열 원소의 재배치 완료
=> O(n) 시간 소요

```c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

#define MAX_SIZE 15
#define SWAP(x, y, t) ((t) = (x), (x) = (y), (y) = (t))

void partition(char list[], int left, int right)
{
    int low = left - 1;
    int high = right + 1;
    char temp;
    
    do
    {
        do
            low++;
        while(list[low] == 'B');
        
        do 
            high--;
        while(list[high] == 'R');
        
        if(low < high)
            SWAP(list[low], list[high], temp);
            
        printf("\nlow = %d, high = %d\n", low, high);
        
        for(int i = 0; i < MAX_SIZE; i++)
            printf("[%c] ", list[i]);
        printf("\n");
    }while(low < high);
}

void main()
{
    char list[MAX_SIZE];
    srand(time(NULL));
    
    for(int i = 0; i < MAX_SIZE; i++)
    {
        if(rand() % 2 == 0)
            list[i] = 'B';
        else
            list[i] = 'R';
    }
    
    for(int i = 0; i < MAX_SIZE; i++)
        printf("[%c] ", list[i]);
    printf("\n");
    
    partition(list, 0, MAX_SIZE - 1);
    printf("\n\n");
    
    for(int i = 0; i < MAX_SIZE; i++)
        printf("[%c] ", list[i]);
    printf("\n");
}
```
