### 그래프 (간선리스트 구현)
```c
#include <stdio.h>
#include <stdlib.h>

typedef struct
{
    char vName;
}Vertex;

typedef struct
{
    int eNo;
    int weight;
    Vertex v1, v2;
}Edge;

typedef struct
{
    Vertex v[10];
    Edge e[30];
    int vCount;
    int eCount;
}Graph;

void init(Graph* G)
{
    G->vCount = 0;
    G->eCount = 0;
}

void makeVertex(Graph* G)
{
    G->v[G->vCount].vName = 97 + G->vCount;
    G->vCount++;
}

void insertEdge(Graph* G, int w, Vertex v1, Vertex v2)
{
    G->e[G->eCount].eNo = G->eCount;
    G->e[G->eCount].weight = w;
    G->e[G->eCount].v1 = v1;
    G->e[G->eCount].v2 = v2;
    G->eCount++;
}

int main()
{
    Graph G;
    init(&G);
    
    for(int i = 0; i < 6; i++)
        makeVertex(&G);
    
    insertEdge(&G, 1, G.v[0], G.v[1]);
    insertEdge(&G, 1, G.v[0], G.v[2]);
    insertEdge(&G, 1, G.v[0], G.v[3]);
    insertEdge(&G, 2, G.v[0], G.v[5]);
    insertEdge(&G, 1, G.v[1], G.v[2]);
    insertEdge(&G, 4, G.v[2], G.v[4]);
    insertEdge(&G, 3, G.v[4], G.v[5]);
    
    for(int i = 0; i < G.vCount; i++)
        printf("[%c] ", G.v[i].vName);
    printf("\n");
    
    for(int i = 0; i < G.eCount; i++)
        printf("[%d : %c - %c - %d] ", G.e[i].eNo, G.e[i].v1.vName, G.e[i].v2.vName, G.e[i].weight);
    printf("\n");
}
```

### 그래프 (인접리스트 구현?)
```c
#include <stdio.h>
#include <stdlib.h>

#define N 10

typedef struct Vertex
{
    int no;
    int weight;
    struct Vertex* next;
}Vertex;

typedef struct
{
    int vCount;
    Vertex* v[N];
}Graph;

void init(Graph* G)
{
    G->vCount = 0;
    for(int i = 0; i < N; i++)
        G->v[i] = NULL;
}

void makeVertex(Graph* G)
{
    G->vCount++;
}

void insertEdge(Graph* G, int w, int v1, int v2)
{
    Vertex* p = (Vertex*)malloc(sizeof(Vertex));
    p->weight = w;
    p->no = v1 + 1;
    p->next = G->v[v2];
    G->v[v2] = p;;
    
    Vertex* q = (Vertex*)malloc(sizeof(Vertex));
    q->weight = w;
    q->no = v2 + 1;
    q->next = G->v[v1];
    G->v[v1] = q;
}

void print(Graph* G)
{
    for(int i = 0; i < G->vCount; i++)
    {
        Vertex* v = G->v[i];
        printf("V[%d] : ", i + 1);
        
        while(v != NULL)
        {
            printf("[%d (%d)] ", v->no, v->weight);
            v = v->next;
        }
        printf("\n");
    }
}

void process(Graph* G, int num)
{
    Vertex* p = G->v[num];
    
    while(p != NULL)
    {
        printf("[%d (%d)] ", p->no, p->weight);
        p = p->next;
    }
    printf("\n");
}

void process2(Graph* G, int v1, int v2, int w)
{
    Vertex* p = G->v[v1];
    
    while(p != NULL)
    {
        if(p->no == v2)
        {
            p->weight = w;
            return;
        }
        else
            p = p->next;
    }
}

int main()
{
    Graph G;
    init(&G);
    
    for(int i = 0; i < 6; i++)
        makeVertex(&G);
    
    insertEdge(&G, 1, 0, 1);
    insertEdge(&G, 1, 0, 2);
    insertEdge(&G, 1, 0, 3);
    insertEdge(&G, 2, 0, 5);
    insertEdge(&G, 1, 1, 2);
    insertEdge(&G, 4, 2, 4);
    insertEdge(&G, 3, 4, 5);
    
    print(&G);
    
    return 0;
}
```
