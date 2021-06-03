### 분리연쇄법
```c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

#define M 13

typedef struct HashNode
{
    int key;
    struct HashNode* next;
}HashType;

void init(HashType* HT)
{
    for(int i = 0; i < M; i++)
    {
        HT[i].key = 0;
        HT[i].next = NULL;
    }
}

int h(int key)
{
    return key % M;
}

void insertItem(HashType* HT, int key)
{
    int v = h(key);
    HashType* node = (HashType*)malloc(sizeof(HashType));
    node->key = key;
    node->next = HT[v].next;
    HT[v].next = node;
}

int removeElement(HashType* HT, int key)
{
    int v = h(key);
    int count = 0;
    HashType* p = &HT[v];
    HashType* q;
    
    while(p->next)
    {
        if(p->next->key == key)
        {
            count++;
            q = p->next;
            p->next = q->next;
            free(q);
        }
        else
            p = p->next;
    }
    return count;
}

int findElement(HashType* HT, int key)
{
    int v = h(key);
    int count;
    HashType* p;
    
    for(p = HT[v].next; p != NULL; p = p->next)
        if(p->key == key)
            count++;
    
    return count;
}

void printHash(HashType* HT)
{
    HashType* p;
    
    for(int i = 0; i < M; i++)
    {
        printf("HT[%02d] : ", i);
        
        for(p = HT[i].next; p != NULL; p = p->next)
            printf(" (%d) ", p->key);
        printf("\n");
    }
}

int main()
{
    HashType HT[M];
    init(HT);
    
    srand(time(NULL));
    
    for(int i = 0; i < 20; i++)
        insertItem(HT, rand() % 90 + 10);
    printHash(HT);
    
    printf("\n삭제할 키 입력 : ");
    int key;
    scanf("%d", &key);
    
    printf("\n키 값 %d가 %d개 삭제되었습니다. \n\n",key, removeElement(HT, key));
    printHash(HT);
    
    return 0;
}
```

### 개방주소법
```c
#include <stdio.h>
#include <stdlib.h>

#define M 13

typedef struct
{
    int key;
    int probeCount;
}Bucket;

typedef struct
{
    Bucket A[M];
}HashType;

void initHash(HashType* HT)
{
    for(int i = 0; i < M; i++)
    {
        HT->A[i].key = 0;
        HT->A[i].probeCount = 0;
    }
}

int h(int key)
{
    return key % M;
}

int h2(int key) // for 이중해싱
{
    return 11 - (key % 11);
}

int getNextBucketLinear(int v, int i) // 선형조사법
{
    return (v + i) % M;
}

int getNextBucketQuadratic(int v, int i) // 2차조사법
{
    return (v + i * i) % M;
}

int getNextBucketDouble(int v, int i, int key) // 이중해싱
{
    return (v + i * h2(key)) % M;
}

int isEmpty(HashType* HT, int b)
{
    return HT->A[b].key == 0;
}

void insertItem(HashType* HT, int key)
{
    int v = h(key);
    int i = 0;
    int count = 0;
    
    while(i < M)
    {
        count++;
        //int b = getNextBucketLinear(v, i);
        //int b = getNextBucketQuadratic(v, i);
        int b = getNextBucketDouble(v, i, key);
        
        if(isEmpty(HT, b))
        {
            HT->A[b].key = key;
            HT->A[b].probeCount = count;
            return;
        }
        else
            i++;
    }
}

int findElement(HashType* HT, int key)
{
    int v = h(key);
    int i = 0;

    while(i < M)
    {
        //int b = getNextBucketLinear(v, i);
        //int b = getNextBucketQuadratic(v, i);
        int b = getNextBucketDouble(v, i, key);
        
        if(isEmpty(HT, b))
            return 0;
        else if(HT->A[b].key == key)
            return key;
        else
            i++;
    }
    return 0;
}

int removeElement(HashType* HT, int key)
{
    int v = h(key);
    int i = 0;

    while(i < M)
    {
        //int b = getNextBucketLinear(v, i);
        //int b = getNextBucketQuadratic(v, i);
        int b = getNextBucketDouble(v, i, key);
        
        if(isEmpty(HT, b))
            return 0;
        else if(HT->A[b].key == key)
        {
            HT->A[b].key = 0;
            HT->A[b].probeCount = 0;
            return key;
        }
        else
            i++;
    }
    return 0;
}

void printHashTable(HashType* HT)
{
    printf("Bucket Key Probe\n");
    printf("========================\n");
    
    for(int i = 0; i < M; i++)
        printf("HT[%02d] : %2d   %d\n", i, HT->A[i].key, HT->A[i].probeCount);
}

int main()
{
    HashType HT;
    initHash(&HT);
    
    insertItem(&HT, 25);
    insertItem(&HT, 13);
    insertItem(&HT, 16);
    insertItem(&HT, 15);
    insertItem(&HT, 7);
    insertItem(&HT, 28);
    insertItem(&HT, 31);
    insertItem(&HT, 20);
    insertItem(&HT, 1);
    insertItem(&HT, 38);
    
    printHashTable(&HT);
    
    int key;
    printf("\n탐색할 키 입력 : ");
    scanf("%d", &key);
    
    if(findElement(&HT, key))
        printf("\n키 값 %d이(가) 존재합니다.\n", key);
    else
        printf("\n키 값 %d이(가) 없습니다.\n", key);
        
        
    printf("\n삭제할 키 입력 : ");
    scanf("%d", &key);
    
    if(removeElement(&HT, key))
        printf("\n키 값 %d이(가) 삭제되었습니다.\n", key);
    else
        printf("\n키 값 %d이(가) 없습니다.\n", key);    
    printf("\n");
    
    printHashTable(&HT);
    return 0;
}
```
