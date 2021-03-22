### List
```c
#include <stdio.h>
#include <stdlib.h>

#define SIZE 100

typedef struct
{
    int V[SIZE];
    int n;
}ArrayList;

void init(ArrayList* L)
{
    L->n = 0;
}

void traverse(ArrayList* L)
{
    for(int i = 0; i < L->n; i++)
        printf("[%d]", L->V[i]);
    printf("\n");
}

void add(ArrayList* L, int pos, int item)
{
    if(L->n == SIZE)
    {
        printf("fullListException..\n");
        exit(1);
    }
    
    if(pos < 0 || pos > L->n){
        printf("invalidPosException..\n");
        exit(1);
    }

    for(int i = L->n - 1; i >= pos; i--)
        L->V[i + 1] = L->V[i];
    
    L->V[pos] = item;
    L->n++;
}

int remove1(ArrayList* L, int pos)
{
  //exception - invalid, empty
  
  int item = L->V[pos];
  
  for(int i = pos + 1; i <= L->n - 1; i++)
    L->V[i-1] = L->V[i];
    
  L->n--;
  
  return item;
}

void main()
{
    ArrayList list;
    
    init(&list);
    
    add(&list, 0, 10);
    traverse(&list);
    
    add(&list, 0, 20);
    traverse(&list);
    
    add(&list, 1, 30);
    traverse(&list);
    
    add(&list, 1, 40);
    traverse(&list);
    
    add(&list, 3, 50);
    traverse(&list);
    

    remove1(&list, 1);
    traverse(&list);
    
    remove1(&list, 2);
    traverse(&list);
    
}
```


### LinkedList

```c
#include <stdio.h>
#include <stdlib.h>


typedef struct ListNode
{
    int data;
    struct ListNode* link;
}ListNode;

typedef struct
{
    ListNode* head;
}LinkedListType;

void init(LinkedListType* L)
{
    L->head = NULL;
}

void addFirst(LinkedListType* L, int item)
{
    ListNode* node = (ListNode*)malloc(sizeof(ListNode));
    node->data = item;
    node->link = L->head;
    L->head = node;
}

void add(LinkedListType* L, int pos, int item)
{
    ListNode* node = (ListNode*)malloc(sizeof(ListNode));
    ListNode* before = L->head;
    
    for(int i = 0; i < pos - 1; i++)
        before = before->link;
    
    node->data = item;
    node->link = before->link;
    before->link = node;
}

void addLast(LinkedListType* L, int item)
{
    ListNode* node = (ListNode*)malloc(sizeof(ListNode));
    ListNode* last = L->head;
    
    while(last->link != NULL)
        last = last->link;
        
    node->data = item;
    last->link = node;
}

int removeFirst(LinkedListType* L)
{
    int item = (L->head)->data;
    L->head = (L->head)->link;
    return item;
    
}

int Remove(LinkedListType* L, int pos)
{
    ListNode* before = L->head;
    
    for(int i = 0; i < pos - 1; i++)
        before = before->link;
    
    int item = (before->link)->data;
    before->link = (before->link)->link;
    
    return item;
}

int removeLast(LinkedListType* L)
{
    ListNode* beforeLast = L->head;
    
    while((beforeLast->link)->link != NULL)
        beforeLast = beforeLast->link;
    
    int item = (beforeLast->link)->data;
    beforeLast->link = NULL;
    
    return item;
}

int get(LinkedListType* L, int pos)
{
    ListNode* p = L->head;
    
    for(int i = 1; i < pos; i++)
        p = p->link;
    
    return p->data;
}

void set(LinkedListType* L, int pos, int item)
{
    ListNode* p = L->head;
    
    for(int i = 1; i < pos; i++)
        p = p->link;
    
    p->data = item;
}

void printList(LinkedListType* L)
{
    for(ListNode* p = L->head; p != NULL; p = p->link)
        printf("[%d] -> ", p->data);
    printf("NULL\n");
}

void main()
{
    LinkedListType list;
    init(&list);
    
    addFirst(&list, 10); printList(&list);
    addFirst(&list, 20); printList(&list);
    addFirst(&list, 30); printList(&list);
    
    getchar();

    add(&list, 1, 40); printList(&list);
    add(&list, 2, 50); printList(&list);
    add(&list, 3, 60); printList(&list);
    
    getchar();
    
    addLast(&list, 70); printList(&list);
    addLast(&list, 80); printList(&list);
    addLast(&list, 90); printList(&list);
    
    getchar();
    
    Remove(&list, 5); printList(&list);
    Remove(&list, 3); printList(&list);
    
    getchar();
    
    removeFirst(&list); printList(&list);
    removeFirst(&list); printList(&list);
    
    getchar();
    
    removeLast(&list); printList(&list);
    removeLast(&list); printList(&list);
    
    getchar();

    int pos;
    printf("\n몇 번 노드의 값을 반환할까요? ");
    scanf("%d", &pos);
    printf("%d번 노드의 값은 %d\n", pos, get(&list, pos));
    
}

```


### LinkedQueue
```c
#include <stdio.h>
#include <stdlib.h>


typedef struct QueueNode
{
    int data;
    struct QueueNode* link;
}QueueNode;


typedef struct
{
    QueueNode* front;
    QueueNode* rear;
}LinkedQueue;


void init(LinkedQueue* queue)
{
    queue->front = queue->rear = NULL;
}


int is_empty(LinkedQueue* queue)
{
    return queue->front == NULL;
}

void enqueue(LinkedQueue* queue, int data)
{
    QueueNode* temp = (QueueNode*)malloc(sizeof(QueueNode));
    temp->data = data;
    temp->link = NULL;
    
    if(is_empty(queue))
    {
        queue->front = temp;
        queue->rear = temp;
    }
    else
    {
        queue->rear->link = temp;
        queue->rear = temp;
    }
}

int dequeue(LinkedQueue* queue)
{
    QueueNode* temp = queue->front;
    int data;
    
    if(is_empty(queue))
    {
        fprintf(stderr, "Queue is empty\n");
        exit(1);
    }
    
    else
    {
        data = temp->data;
        queue->front = temp->link;
        
        if(queue->front == NULL) // 원소가 하나만 있었을 경우 제거하면 아무것도 없으니까 queue->rear = null
            queue->rear = NULL;
            
        free(temp);
        return data;
    }
}

void print_queue(LinkedQueue* queue)
{
    QueueNode* p;
    for(p = queue->front; p != NULL; p = p->link)
        printf("|%d| -> ", p->data);
    printf("|NULL|\n");
}



void main()
{
    LinkedQueue queue;
    init(&queue);
    
    enqueue(&queue, 10);
    print_queue(&queue);
    
    enqueue(&queue, 20);
    print_queue(&queue);
    
    enqueue(&queue, 30);
    print_queue(&queue);
    
    dequeue(&queue);
    print_queue(&queue);
    
    dequeue(&queue);
    print_queue(&queue);
    
    dequeue(&queue);
    print_queue(&queue);
    
}
```

### 케이크 문제

생일케이크에 n  0개의 불켜진 양초가 원형으로 빙둘러 서있다
첫번째 양초부터 시작하여, k  0개의 양초를 건너뛰어 나타나는 양초의 불을 끄고 뽑아낸다
그리고는 다음 양초로부터 시작하여 k개의 양초를 건너뛰어 나타나는 양초의 불을 끄고 뽑아낸다
원을 돌면서 양초가 하나만 남을 때까지 촛불 끄고 뽑아내기를 계속
이 마지막 양초는 내부에 특수장치가 설치되어 있어서 불이 꺼짐과 동시에 멋진 축하쇼를 펼치도록 되어 있다
n과 k를 미리 알 경우, 원래 양초들의 원형 배치에서 특수 양초의 위치를 어디로 해놓아야 마지막까지 남을지 알고 싶다


모의실행을 통해 특수 양초의 위치를 나타내는 양의 정수를 반환하는 알고리즘을, 원형의 양초 리스트를 다음 두 가지 데이터구조로 각각 구현하고자 한다
A. 배열
B. 원형연결리스트

위 A, B 각각의 선택에 대해 아래 알고리즘을 의사코드로작성하라
 candle(n, k): buildList(n)을 호출한 후 runSimulation(S, n, k)를 수행
 buildList(n): 요구된 데이터구조를 사용하여 크기 n의 초기 리스트 S를 구축
 runSimulation(S, n, k): 크기 n의 리스트 S에 대해 k를 사용하여 마지막 양초만 남을 때까지 불끄기를 모의실행하고 마지막 양초의 위치를 반환


### A.배열1

```c
#include <stdio.h>
#include <stdlib.h>
#define SIZE 100
typedef struct
{
    int V[SIZE];
    int n;
}ArrayList;
void init(ArrayList* L)
{
    L->n = 0;
}
void buildList(ArrayList* L, int n) // 요구된 데이터구조를 사용하여 크기 n의 초기 리스트 L를 구축
{
    L->n = n;
    for (int i = 0; i < n; i++)
        L->V[i] = i + 1;
}
// 크기 n의 리스트 L에 대해 k를 사용하여 마지막 양초만 남을 때까지 불끄기를 모의실행하고 마지막 양초의 위치를 반환
int runSimulation1(ArrayList* L, int k)
{
    int i, r = 0;
    int remains = L->n;
    while (remains > 1)
    {
        i = 0;
        while (i < k)
        {
            r = (r + 1) % L->n;
            if (L->V[r] != 0)
                i++;
        }
        L->V[r] = 0;
        remains--;
        while (L->V[r] == 0) 
            r = (r + 1) % L->n;
            
    }
    return L->V[r];
}
int candle(ArrayList* L, int n, int k) // buildList(n)을 호출한 후 runSimulation(S, n, k)를 수행
{
    buildList(L, n);
    return runSimulation1(L, k);
}
void main()
{
    ArrayList list;
    init(&list);
    printf("마지막 양초의 위치는 %d 입니다.\n", candle(&list, 7, 3));
}
```


### A.배열2

```c
#include <stdio.h>
#include <stdlib.h>
#define SIZE 100
typedef struct
{
    int V[SIZE];
    int n;
}ArrayList;
void init(ArrayList* L)
{
    L->n = 0;
}
int remove1(ArrayList* L, int pos)
{
    int item = L->V[pos];
    for (int i = pos + 1; i <= L->n - 1; i++)
        L->V[i - 1] = L->V[i];
    L->n--;
    return item;
}
void buildList(ArrayList* L, int n) // 요구된 데이터구조를 사용하여 크기 n의 초기 리스트 L를 구축
{
    L->n = n;
    for (int i = 0; i < n; i++)
        L->V[i] = i + 1;
}
// 크기 n의 리스트 L에 대해 k를 사용하여 마지막 양초만 남을 때까지 불끄기를 모의실행하고 마지막 양초의 위치를 반환
int runSimulation2(ArrayList* L, int k)
{
    int r = 0;
    int remains = L->n;
    while(remains > 1)
    {
        r = (r + k) % remains;
        remains--;
        remove1(L, r);
    }
    return L->V[0];
}
int candle(ArrayList* L, int n, int k) // buildList(n)을 호출한 후 runSimulation(S, n, k)를 수행
{
    buildList(L, n);
    return runSimulation2(L, k);
}
void main()
{
    ArrayList list;
    init(&list);
    printf("마지막 양초의 위치는 %d 입니다.\n", candle(&list, 7, 3));
}

```

### B.원형연결리스트

```c
#include <stdio.h>
#include <stdlib.h>
#define SIZE 100
typedef struct ListNode
{
    int data;
    struct ListNode* link;
}ListNode;
typedef struct
{
    ListNode* head;
}LinkedListType;
void init(LinkedListType* L)
{
    L->head = NULL;
}
ListNode* getNode() {
    ListNode* node = (ListNode*)malloc(sizeof(ListNode));
    return node;
}
LinkedListType* buildList(LinkedListType* L, int n)
{
    ListNode* p = getNode();
    L->head = p;
    p->data = 1;
    for (int i = 2; i <= n; i++) {
        p->link = getNode();
        p = p->link;
        p->data = i;
    }
    p->link = L->head;
    return L;
}
int runSimulation(LinkedListType* L, int k)
{
    ListNode* p = L->head;
    ListNode* pnext;
    while (p != p->link) {
        for (int i = 1; i < k; i++)
            p = p->link;
        pnext = p->link;
        p->link = (p->link)->link;
        free(pnext);
        p = p->link;
    }
    return p->data;
}
int candle(LinkedListType* L, int n, int k)
{
    LinkedListType* list = buildList(L, n);
    return runSimulation(list, k);
}
void main()
{
    LinkedListType list;
    init(&list);
    printf("마지막 양초의 위치는 %d 입니다.\n", candle(&list, 7, 3));
}

```
