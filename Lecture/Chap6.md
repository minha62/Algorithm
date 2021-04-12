### 힙정렬
```c
#include <stdio.h>
#include <stdlib.h>

#define MAX_ELEMENT 100

typedef struct
{
    int heap[MAX_ELEMENT];
    int heap_size;
}HeapType;

void init(HeapType *h)
{
    h->heap_size = 0;
}

void upHeap(HeapType *h)
{
    int i = h->heap_size;
    int key = h->heap[i];
    
    while((i != 1) && (key < h->heap[i / 2])) // root가 아니고 부모의 키보다 작을때
    {
        h->heap[i] = h->heap[i / 2];
        i /= 2;
    }
    h->heap[i] = key;
}

void downHeap(HeapType *h)
{
    int temp = h->heap[1];
    int parent = 1, child = 2;
    
    while(child <= h->heap_size)
    {
        if((child < h->heap_size) && (h->heap[child] > h->heap[child + 1]))
            child++;
            
        if(temp <= h->heap[child])
            break;
        
        h->heap[parent] = h->heap[child];
        parent = child;
        child *= 2;
    }
    h->heap[parent] = temp;
}

void insertItem(HeapType *h, int key)
{
    h->heap_size += 1;
    h->heap[h->heap_size] = key;
    upHeap(h);
}

int removeMin(HeapType *h)
{
    int key = h->heap[1];
    h->heap[1] = h->heap[h->heap_size];
    h->heap_size -= 1;
    downHeap(h);
    
    return key;
}

void printHeap(HeapType *h)
{
    for(int i = 1; i <= h->heap_size; i++)
        printf("[%d] ", h->heap[i]);
    printf("\n");
}

void heapSort(HeapType *h, int list[])
{
    HeapType heap;
    init(&heap);
    
    for(int i = 1; i <= h->heap_size; i++)
    {
        heap.heap[i] = h->heap[i];
        heap.heap_size++;
    }
    
    for(int i = 1; i <= h->heap_size; i++)
        list[i] = removeMin(&heap);
}

void printArray(int list[], int n)
{
    for(int i = 1; i <= n; i++)
        printf("[%d] ", list[i]);
    printf("\n");
}

void main()
{
    HeapType heap;
    init(&heap);
    
    int list[MAX_ELEMENT] = {0};
    
    insertItem(&heap, 5);
    insertItem(&heap, 3);
    insertItem(&heap, 7);
    insertItem(&heap, 4);
    insertItem(&heap, 1);
    insertItem(&heap, 4);
    insertItem(&heap, 8);
    insertItem(&heap, 2);
    
    printHeap(&heap);
    
    heapSort(&heap, list);
    printArray(list, heap.heap_size);
    
    //printf("deleted key : %d\n", removeMin(&heap));
    //printHeap(&heap);
}
```

### 제자리 힙 정렬 알고리즘
```c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

#define MAX_ELEMENT 100

typedef struct
{
    int heap[MAX_ELEMENT];
    int heap_size;
}HeapType;

void init(HeapType *h)
{
    h->heap_size = 0;
}

void upHeap(HeapType *h)
{
    int i = h->heap_size;
    int key = h->heap[i];
    
    while((i != 1) && (key < h->heap[i / 2])) // root가 아니고 부모의 키보다 작을때
    {
        h->heap[i] = h->heap[i / 2];
        i /= 2;
    }
    h->heap[i] = key;
}

void downHeap(HeapType *h)
{
    int temp = h->heap[1];
    int parent = 1, child = 2;
    
    while(child <= h->heap_size)
    {
        if((child < h->heap_size) && (h->heap[child] > h->heap[child + 1]))
            child++;
            
        if(temp <= h->heap[child])
            break;
        
        h->heap[parent] = h->heap[child];
        parent = child;
        child *= 2;
    }
    h->heap[parent] = temp;
}

void insertItem(HeapType *h, int key)
{
    h->heap_size += 1;
    h->heap[h->heap_size] = key;
    upHeap(h);
}

int removeMin(HeapType *h)
{
    int key = h->heap[1];
    h->heap[1] = h->heap[h->heap_size];
    h->heap_size -= 1;
    downHeap(h);
    
    return key;
}

void printHeap(HeapType *h)
{
    for(int i = 1; i <= h->heap_size; i++)
        printf("[%d] ", h->heap[i]);
    printf("\n");
}

void heapSort(HeapType *h, int list[])
{
    HeapType heap;
    init(&heap);
    
    for(int i = 1; i <= h->heap_size; i++)
    {
        heap.heap[i] = h->heap[i];
        heap.heap_size++;
    }
    
    for(int i = 1; i <= h->heap_size; i++)
        list[i] = removeMin(&heap);
}

void inPlaceHeapSort(HeapType *h) // 내림차순
{
    int size = h->heap_size;
    int key;
    
    for(int i = 0; i < size; i++)
    {
        key = removeMin(h);
        h->heap[h->heap_size + 1] = key;
    }
}

void printArray(int list[], int n)
{
    for(int i = 1; i <= n; i++)
        printf("[%d] ", list[i]);
    printf("\n");
}

void main()
{
    HeapType heap;
    int list[MAX_ELEMENT] = {0};
    
    srand(time(NULL));
    init(&heap);
    
    for(int i = 0; i < 15; i++)
        insertItem(&heap, rand() % 100);
    
    printHeap(&heap);
    getch();
    
    inPlaceHeapSort(&heap);
    for(int i = 1; heap.heap[i] > 0; i++)
        printf("[%d] ", heap.heap[i]);
}
```

### 상향식 힙 생성
```c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

#define MAX_ELEMENT 100

struct Heap
{
    int H[MAX_ELEMENT];
    int n;
}_Heap;

void downHeap(int i)
{
    if(i * 2 > _Heap.n) // left child 
        return;
        
    if(_Heap.H[i] < _Heap.H[i * 2] || _Heap.H[i] < _Heap.H[i * 2 + 1])
    {
        if(_Heap.H[i * 2] > _Heap.H[i * 2 + 1])
        {
            int temp;
            temp = _Heap.H[i];
            _Heap.H[i] = _Heap.H[i * 2];
            _Heap.H[i * 2] = temp;
            downHeap(i * 2);
        }
        
        else
        {
            int temp;
            temp = _Heap.H[i];
            _Heap.H[i] = _Heap.H[i * 2 + 1];
            _Heap.H[i * 2 + 1] = temp;
            downHeap(i * 2 + 1);
        }
    }
    
    else
    {
        return;
    }
}

void buildHeap()
{
    for(int i = _Heap.n / 2; i >= 1; i--)
        downHeap(i);
}

void rBuildHeap(int i)
{
    if(i > _Heap.n)
        return;
    
    if(i * 2 <= _Heap.n)
        rBuildHeap(i * 2);
    
    if(i * 2 + 1 <= _Heap.n)
        rBuildHeap(i * 2 + 1);
    
}

int removeMax()
{
    int key = _Heap.H[1];
    _Heap.H[1] = _Heap.H[_Heap.n--];
    downHeap(1);
    
    return key;
}

void inPlaceHeapSort() // 오름차순
{
    int size = _Heap.n;
    int key;
    
    for(int i = 0; i < size; i++)
    {
        key = removeMax();
        _Heap.H[_Heap.n + 1] = key;
    }
}

void printHeap()
{
    for(int i = 1; i <= _Heap.n; i++)
        printf("[%d] ", _Heap.H[i]);
    printf("\n");
}

void printArray()
{
    for(int i = 1; _Heap.H[i] > 0; i++)
        printf("[%d] ", _Heap.H[i]);
    printf("\n");
}

void main()
{
    _Heap.n = 0;
    srand(time(NULL));
    
    printf("입력할 원소의 개수 : ");
    scanf("%d", &_Heap.n);
    
    for(int i = 1; i <= _Heap.n; i++)
        _Heap.H[i] = rand() % 100;
        
    //buildHeap();
    rBuildHeap(1);
    printHeap();
    
    inPlaceHeapSort();
    printArray(); // n이 0이 되기 때문에 printArray()로 출력해야 함
}
```

### 힙의 마지막 노드
```c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>
#include <stdbool.h>

#define MAX_ELEMENT 100

typedef struct
{
    int heap[MAX_ELEMENT];
    int heap_size;
}Heap;

void initHeap(Heap* h)
{
    h->heap_size = 0;
}

void upHeap(Heap* h)
{
    int i = h->heap_size;
    int key = h->heap[i];

    while ((i != 1) && (key < h->heap[i / 2]))
    {
        h->heap[i] = h->heap[i / 2];
        i /= 2;
    }
    h->heap[i] = key;
}

void insertItem(Heap* h, int key)
{
    h->heap_size += 1;
    h->heap[h->heap_size] = key;
    upHeap(h);
}

void printHeap(Heap* h)
{
    for (int i = 1; i <= h->heap_size; i++)
        printf("[%d] ", h->heap[i]);
    printf("\n");
}

typedef struct
{
    int array[MAX_ELEMENT];
    int top;
}Stack;

void initStack(Stack* s)
{
    s->top = -1;
}

bool isEmpty(Stack* s)
{
    if (s->top == -1)
        return true;
    else
        return false;
}

void push(Stack* s, int data)
{
    s->array[++s->top] = data;
}

int pop(Stack* s)
{
    int key;
    key = s->array[s->top];
    s->top--;

    return key;
}

int binaryExpansion(int n, Stack* s)
{
    while (n >= 2)
    {
        push(s, n % 2);
        n /= 2;
    }

    push(s, n);

    return;
}


int findLastNode(Heap *h, int n)
{
    int v = 1;

    Stack s;
    initStack(&s);

    binaryExpansion(n, &s);
    pop(&s);

    while(!isEmpty(&s))
    {
        int bit = pop(&s);

        if (bit == 0)
            v = 2 * v;

        else
            v = 2 * v + 1;
    }

    return h->heap[v];
}


void main()
{
    int lastNode;

    Heap heap;
    initHeap(&heap);

    srand(time(NULL));

    for (int i = 0; i < 10; i++)
        insertItem(&heap, rand() % 100);

    printHeap(&heap);

    lastNode = findLastNode(&heap, heap.heap_size);

    printf("힙의 마지막 노드 = %d\n", lastNode);
}
```
