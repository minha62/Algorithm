### 선형탐색
```c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>
#define SIZE 15

typedef struct
{
    int key;
    char value[10];
}element;

typedef struct
{
    element dict[SIZE];
    int size;
}DictType;

void init(DictType* d)
{
    d->size = 0;
}

void insertKey(DictType* d)
{
    for (int i = 0; i < SIZE; i++)
    {
        d->dict[i].key = rand() % 30 + 1;

        for (int j = 0; j < i; j++)
            if (d->dict[i].key == d->dict[j].key)
                i--;
    }
}

void insertValue(DictType* d)
{
    for (int i = 0; i < SIZE; i++)
    {
        for (int j = 0; j < 5; j++)
            d->dict[i].value[j] = rand() % 26 + 97;

        d->size++;
    }
}

void makeDict(DictType* d)
{
    insertKey(d);
    insertValue(d);
}

void insertion_sort(DictType* d)
{
    int i, j;
    element item;

    for (i = 1; i < SIZE; i++)
    {
        item = d->dict[i];

        for (j = i - 1; j >= 0 && d->dict[j].key > item.key; j--)
            d->dict[j + 1] = d->dict[j];

        d->dict[j + 1] = item;
    }
}

void printDict(DictType* d)
{
    printf("key value \n==================\n");

    for (int i = 0; i < d->size; i++)
    {
        printf("%2d ", d->dict[i].key);

        for (int j = 0; j < 5; j++)
            printf("%c", d->dict[i].value[j]);
        printf("\n");
    }
}

int main()
{
    DictType d;
    init(&d);
    srand(time(NULL));
    makeDict(&d);
    printDict(&d);

    getchar();
    printf("\n");

    insertion_sort(&d);
    printDict(&d);
}
```

### 이진탐색 재귀&반복, 삭제
```c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>
#define SIZE 15

typedef struct
{
    int key;
    char value[10];
}element;

typedef struct
{
    element dict[SIZE];
    int size;
}DictType;

void init(DictType* d)
{
    d->size = 0;
}

void insertKey(DictType* d)
{
    for (int i = 0; i < SIZE; i++)
    {
        d->dict[i].key = rand() % 30 + 1;

        for (int j = 0; j < i; j++)
            if (d->dict[i].key == d->dict[j].key)
                i--;
    }
}

void insertValue(DictType* d)
{
    for (int i = 0; i < SIZE; i++)
    {
        for (int j = 0; j < 5; j++)
            d->dict[i].value[j] = rand() % 26 + 97;

        d->size++;
    }
}

void makeDict(DictType* d)
{
    insertKey(d);
    insertValue(d);
}

void insertion_sort(DictType* d)
{
    int i, j;
    element item;

    for (i = 1; i < SIZE; i++)
    {
        item = d->dict[i];

        for (j = i - 1; j >= 0 && d->dict[j].key > item.key; j--)
            d->dict[j + 1] = d->dict[j];

        d->dict[j + 1] = item;
    }
}

int rFindElement(DictType* d, int key, int l, int r) // 이진탐색 재귀
{
    int mid;
    
    if (l <= r)
    {
        mid = (l + r) / 2;

        if (key == d->dict[mid].key)
            return mid;

        else if (key < d->dict[mid].key)
            return rFindElement(d, key, l, mid - 1);

        else
            return rFindElement(d, key, mid + 1, r);
    }
    return -1;
}

int findElement(DictType* d, int key, int l, int r) // 이진탐색 반복
{
    int mid;

    while (l <= r)
    {
        mid = (l + r) / 2;

        if (key == d->dict[mid].key)
            return mid;

        else if (key < d->dict[mid].key)
            r = mid - 1;

        else
            l = mid + 1;
    }
}

element removeElement(DictType* d, int key)
{
    int index = findElement(d, key, 0, d->size - 1);

    if (index == -1)
    {
        printf("삭제할 요소가 없습니다.\n");
        return;
    }
    else
    {
        element item = d->dict[index];

        for (int i = index; i < d->size; i++)
            d->dict[i] = d->dict[i + 1];
        d->size--;

        return item;
    }
}

void printDict(DictType* d)
{
    printf("key value \n==================\n");

    for (int i = 0; i < d->size; i++)
    {
        printf("%2d ", d->dict[i].key);

        for (int j = 0; j < 5; j++)
            printf("%c", d->dict[i].value[j]);
        printf("\n");
    }
}

int main()
{
    DictType d;
    init(&d);
    srand(time(NULL));
    makeDict(&d);
    printDict(&d);

    getchar();
    printf("\n");

    insertion_sort(&d);
    printDict(&d);
    getchar();

    int key;
    printf("\n검색할 키 값을 입력 : ");
    scanf_s("%d", &key);

    int index = rFindElement(&d, key, 0, SIZE - 1);

    if (index == -1)
        printf("\n검색에 실패하였음\n");
    else
    {
        printf("\n위치 %d에서 키 : %d, 값 : ", index + 1, key);

        for (int j = 0; j < 5; j++)
            printf("%c", d.dict[index].value[j]);
        printf("이 검색되었음\n");
    }

    printf("\n삭제할 키 값을 입력 : ");
    scanf_s("%d", &key);

    removeElement(&d, key);
    printDict(&d, key);
}
```

### 교차선분
```c
#include <stdio.h>
#include <stdlib.h>

#define SIZE 6

typedef struct
{
    int num;
    float s, e;
}Segment;

typedef struct
{
    float coor;
    char code;
    int id;
}Event;

typedef struct
{
    int id1, id2;
}interSegment;

void insertion_sort(Event ev[], int n)
{
    int i, j;
    Event item;
    
    for(i = 1; i < n; i++)
    {
        item = ev[i];
        
        for(j = i - 1; j >= 0 && ev[j].coor > item.coor; j--)
            ev[j + 1] = ev[j];
        ev[j + 1] = item;
    }
}

void findIntersectingSegment(Event ev[])
{
    int openSegments[SIZE];
    interSegment is[SIZE * 2];
    int oCnt = 0;
    int iCnt = 0;
    
    for(int i = 0; i < SIZE * 2; i++)
    {
        if(ev[i].code == 'S')
        {
            for(int j = 0; j < oCnt; j++)
                if(openSegments[j] != 0)
                {
                    is[iCnt].id1 = openSegments[j];
                    is[iCnt].id2 = ev[i].id;
                    iCnt++;
                }
            openSegments[oCnt++] = ev[i].id;
        }
        
        else
        {
            for(int j = 0; j < oCnt; j++)
                if(openSegments[j] == ev[i].id)
                openSegments[j] = 0;
        }
    }
    
    for(int i = 0; i < iCnt; i++)
        printf("(%d, %d) ", is[i].id1, is[i].id2);
    printf("\n");
}

int main()
{
    Segment lines[] = {{1, 1.0, 3.2}, {2, 0.8, 3.0}, {3, 0.6, 2.8}, {4, 1.1, 2.0}, {5, 5.4, 7.0}, {6, 2.9, 5.0}};
    Event ev[SIZE * 2];
    
    for(int i = 0; i < SIZE; i++)
        printf("%d, (%.1f ~ %.1f)\n", lines[i].num, lines[i].s, lines[i].e);
    printf("\n");
    getchar();
    
    for(int i = 0, j = 0; i < SIZE * 2; i++, j++)
    {
        ev[i].coor = lines[j].s;
        ev[i].code = 'S';
        ev[i].id = lines[j].num;
        ev[i + 1].coor = lines[j].e;
        ev[i + 1].code = 'E';
        ev[++i].id = lines[j].num;
    }
    
    for(int i = 0; i < SIZE * 2; i++)
        printf("((%.1f, %c), %d)\n", ev[i].coor, ev[i].code, ev[i].id);
    printf("\n");
    getchar();
    
    insertion_sort(ev, SIZE * 2);
    for(int i = 0; i < SIZE * 2; i++)
        printf("((%.1f, %c), %d)\n", ev[i].coor, ev[i].code, ev[i].id);
    printf("\n");
    getchar();
    
    findIntersectingSegment(ev);
}
```

### 배열의 두 수 덧셈
```c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

#define SIZE 8

typedef struct
{
    int elem;
    int idx;
}Dict;

void sort(Dict D[])
{
    int elem, i, j, idx;

    for (i = 1; i < SIZE; i++)
    {
        elem = D[i].elem;
        idx = D[i].idx;

        for (j = i - 1; j >= 0 && D[j].elem > elem; j--)
        {
            D[j + 1].elem = D[j].elem;
            D[j + 1].idx = D[j].idx;
        }

        D[j + 1].elem = elem;
        D[j + 1].idx = idx;
    }
}

int findElement(Dict D[], int v)
{
    for (int i = 0; i < SIZE; i++)
    {
        if (D[i].elem == v)
            return D[i].idx;
    }
    return -1;
}

void findIndexPair(Dict D[], int A[], int s)
{
    int j;

    for (int i = 0; i < SIZE; i++)
    {
        int v = s - A[i];
        j = findElement(D, v);

        if (i != j && j != -1)
        {
            printf("(%d, %d)\n", i, j);
            break;
        }
    }
    if (j == -1)
        printf("Not Found\n");
}

void buildDict(Dict D[], int A[])
{
    for (int i = 0; i < SIZE; i++)
    {
        D[i].elem = A[i];
        D[i].idx = i;
    }

    sort(D);
}

void main()
{
    int A[SIZE] = { 2, 21, 8, 3, 5, 1, 13, 1 };
    Dict D[SIZE];

    buildDict(D, A);

    for (int i = 0; i < SIZE; i++)
        printf("(%d, %d) ", D[i].elem, D[i].idx);
    printf("\n");

    findIndexPair(D, A, 24);
}
```
