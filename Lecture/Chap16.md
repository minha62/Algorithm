### Prim-Jarnik
```c
#include <stdio.h>
#include <stdlib.h>

#define N 5
#define INF 10000

typedef struct Edge
{
    int vNum1;
    int vNum2;
    int weight;
    struct Edege* next;
}Edge;

typedef struct IncidentEdge
{
    int adjVertex;
    int weight;
    Edge* e;
    struct IncidentEdge* next;
}IncidentEdge;

typedef struct Vertex
{
    int num;
    int isFresh;
    struct Vertex* next;
    IncidentEdge* top;
}Vertex;

Vertex* vHead = NULL;
Edge* eHead = NULL;
int vCount;
int eCount;
int dist[N];

void makeVertices()
{
    Vertex* p = (Vertex*)malloc(sizeof(Vertex));
    p->num = ++vCount;
    p->top = NULL;
    p->next = NULL;
    p->isFresh = 0;
    
    Vertex* q = vHead;
    if(q == NULL)
        vHead = p;
    else
    {
        while(q->next != NULL)
            q = q->next;
        q->next = p;
    }
}

void insertIncidentEdge(Vertex* v, int av, Edge* e)
{
    IncidentEdge* p = (IncidentEdge*)malloc(sizeof(IncidentEdge));
    p->adjVertex = av;
    p->weight = e->weight;
    p->e = e;
    p->next = NULL;
    
    IncidentEdge* q = v->top;
    if(q == NULL)
        v->top = p;
    else
    {
        while(q->next != NULL)
            q = q->next;
        q->next = p;
    }
}

Vertex* findVertex(int v)
{
    Vertex* p = vHead;
    while(p->num != v)
        p = p->next;
    return p;
}

void insertEdges(int v1, int v2, int w)
{
    Edge* p = (Edge*)malloc(sizeof(Edge));
    p->vNum1 = v1;
    p->vNum2 = v2;
    p->weight = w;
    p->next = NULL;
    
    Edge* q = eHead;
    if(q == NULL)
        eHead = p;
    else
    {
        while(q->next != NULL)
            q = q->next;
        q->next = p;
    }
    
    Vertex* v = findVertex(v1);
    insertIncidentEdge(v, v2, p);
    
    v = findVertex(v2);
    insertIncidentEdge(v, v1, p);
}

int getMinVertex()
{
    int v;
    Vertex* p = vHead;
    
    for(; p != NULL; p = p->next)
        if(p->isFresh == 0)
        {
            v = p->num - 1;
            break;
        }
        
    for(; p != NULL; p = p->next)
        if(p->isFresh == 0 && (dist[p->num - 1] < dist[v]))
            v = p->num - 1;
    
    return v;
}

void prim(Vertex* v)
{
    IncidentEdge* q;
    Vertex* p;
    int u;
    
    for(int i = 0; i < N; i++)
        dist[i] = INF;
    
    dist[v->num - 1] = 0;
    
    for(int i = 0; i < N; i++)
    {
        u = getMinVertex();
        p = findVertex(u + 1);
        p->isFresh = 1;
        printf("[%d] ", p->num);
        
        for(q = p->top; q != NULL; q = q->next)
        {
            p = findVertex(q->adjVertex);
            
            if(p->isFresh == 0)
                dist[q->adjVertex - 1] = q->weight;
        }
    }
}

void print()
{
    Vertex* p = vHead;
    IncidentEdge* q;
    
    for(; p != NULL; p = p->next)
    {
        printf("정점 %d : ", p->num);
        for(q = p->top; q != NULL; q = q->next)
            printf("[%d (%d)] ", q->adjVertex, q->weight);
        printf("\n");
    }
}

void main()
{
    for(int i = 0; i < N; i++)
        makeVertices();
        
    insertEdges(1, 2, 1);
    insertEdges(1, 4, 2);
    insertEdges(1, 5, 4);
    insertEdges(2, 3, 6);
    insertEdges(2, 5, 7);
    insertEdges(3, 5, 5);
    insertEdges(4, 5, 3);
    
    print();
    prim(vHead);
}
```

### 배낭채우기 using 부분집합
```c
#include <stdio.h>
#include <stdlib.h>

#define N 100

int weight[N], value[N], cap;
int maxSet[N], maxSetSize = 0, maxValue = 0;

/*
void subset(int arr[], int setSize, int n, int idx) // 부분집합
{
    if(idx == n)
    {
        printArr(arr, setSize);
        return;
    }
    
    arr[setSize] = idx;
    
    subset(arr, setSize + 1, n, idx + 1); // 왼쪽으로
    subset(arr, setSize, n, idx + 1); // 오른쪽으로
}
*/

void eval_knapsack(int arr[], int setSize)
{
    int currWeight = 0, currValue = 0;
    
    for(int i = 0; i < setSize; i++)
    {
        currWeight += weight[arr[i]];
        currValue += value[arr[i]];
    }
    
    if(currWeight > cap)
        return;
    
    if(currValue > maxValue)
    {
        maxValue = currValue;
        maxSetSize = setSize;
        
        for(int i = 0; i < setSize; i++)
            maxSet[i] = arr[i];
    }
}

void subset_knapsack(int arr[], int setSize, int n, int idx)
{
    if(idx == n)
    {
        eval_knapsack(arr, setSize);
        return;
    }
    
    arr[setSize] = idx;
    
    subset_knapsack(arr, setSize + 1, n, idx + 1); 
    subset_knapsack(arr, setSize, n, idx + 1); 
}

void printArr(int arr[], int n)
{
    for(int i = 0; i < n; i++)
        printf("[%d] ", arr[i]);
    printf("\n");
}


void main()
{
    int arr[N], n;
    scanf("%d %d", &n, &cap);
    
    for(int i = 0; i < n; i++)
        scanf("%d", &value[i]);
    
    for(int i = 0; i < n; i++)
        scanf("%d", &weight[i]);
    
    subset_knapsack(arr, 0, n, 0);
    
    printf("Max value : %d\n", maxValue);
    printArr(maxSet, maxSetSize);
}
```

### 부분집합의 합 (동적프로그래밍)
```c
#include <stdio.h>
#include <stdlib.h>

int c[15][150];

void calculate_subset_sum(int s[], int n, int m) // 동적프로그래밍
{
    int i, j;
    
    for(i = 0; i <= n; i++)
        c[i][0] = 1;
    
    for(i = 1; i <= m; i++)
        c[0][i] = 0;
    
    for(i = 1; i <= n; i++) 
    {
        for(j = 1; j <= m; j++)
        {
            c[i][j] = 0;
            
            if(j >= s[i - 1]) // 현재 j가 이전 가치보다 크고
                if(c[i - 1][j - s[i - 1]] == 1)
                    c[i][j] = 1;
            
            if(c[i - 1][j] == 1)
                c[i][j] = 1;
        }
    }
}

void main()
{
    int m, n;
    printf("input m, n : ");
    scanf("%d %d", &m, &n);
    
    int* s = (int*)malloc(sizeof(int) * n);
    
    for(int i = 0; i < n; i++)
        scanf("%d", &s[i]);
        
    calculate_subset_sum(s, n, m);
    
    if(c[n][m])
        printf("Possible");
    else
        printf("Not Possible");
}
```
