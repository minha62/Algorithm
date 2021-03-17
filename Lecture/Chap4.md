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
    //exception - node가 null이냐, 제대로 처리됐는가
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

//addlast - for
//remove, remove first, remove last

int get(LinkedListType* L, int pos)
{
    ListNode* p = L->head;
    
    for(int i = 1; i < pos; i++)
        p = p->link;
    
    return p->data;
}

void set(LinkedListType* L, int pos, int iten)
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
    
    addFirst(&list, 10);
    printList(&list);
    
    addFirst(&list, 20);
    printList(&list);
    
    addFirst(&list, 30);
    printList(&list);
    
    getchar();
    
    add(&list, 1, 40);
    printList(&list);
    
    add(&list, 2, 50);
    printList(&list);
    
    add(&list, 3, 60);
    printList(&list);
    
    int pos;
    printf("\n몇 번 노드의 값을 반환할까요? ");
    scanf("%d", &pos);
    printf("%d번 노드의 값은 %d\n", pos, get(&list, pos));
    
}

```
