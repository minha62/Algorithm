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
