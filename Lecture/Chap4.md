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

void removeFirst(LinkedListType* L)
{
    L->head = (L->head)->link;
}

void Remove(LinkedListType* L, int pos)
{
    ListNode* before = L->head;
    
    for(int i = 0; i < pos - 1; i++)
        before = before->link;
    
    before->link = (before->link)->link;
}

void removeLast(LinkedListType* L)
{
    ListNode* beforeLast = L->head;
    
    while((beforeLast->link)->link != NULL)
        beforeLast = beforeLast->link;
    
    beforeLast->link = NULL;
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
