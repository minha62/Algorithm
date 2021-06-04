### 강연결 검사
```c
#include <stdio.h>
#include <stdlib.h>

#define SIZE 10
#define TRUE 10
#define FALSE 10

typedef struct
{
    int n, m;
    int adj_mat[SIZE][SIZE];
}GraphType;

void init(GraphType* g)
{
    for(int row = 0; row < SIZE; row++)
        for(int col = 0; col < SIZE; col++)
            g->adj_mat[row][col] = 0;
}

void insert_edge(GraphType* g, int start, int end)
{
    if((start >= g->n) || (end >= g->n))
    {
        printf("간선을 추가할 수 없습니다.\n");
        return;
    }
    
    g->adj_mat[start][end] = 1;
    //g->adj_mat[end][start] = 1; - 이 코드 추가하면 무방향
}

void print_adj_mat(GraphType* g)
{
    printf("\n");
    for(int row = 0; row < g->n; row++)
    {
        printf("|");
        
        for(int col = 0; col < g->n; col++)
            printf(" %d ", g->adj_mat[row][col]);
            
        printf("|\n");
    }
}

void dfs_mat(GraphType* g, int visited[], int v)
{
    visited[v] = TRUE;
    printf("정점[%d] ", v + 1);
    
    for(int w = 0; w < g->n; w++)
        if(g->adj_mat[v][w] && !visited[w])
            dfs_mat(g, visited, w);
}

void rev_mat(GraphType* g, int v)
{
    GraphType r;
    init(&r);
    r.n = g->n;
    int visited[SIZE] = {FALSE};
    
    for(int row = 0; row < g->n; row++)
        for(int col = 0; col < g->n; col++)
            r.adj_mat[col][row] = g->adj_mat[row][col];
    
    print_adj_mat(&r);
    printf("\n");
    
    dfs_mat(&r, visited, v - 1);
}

void main()
{
    GraphType g;
    init(&g);
    int n, m, v;
    int start, end;
    
    printf("정점과 간선의 개수를 입력하세요. \n");
    scanf("%d %d", &g.n, &g.m);
    
    for(int i = 0; i < g.m; i++)
    {
        scanf("%d %d", &start, &end);
        insert_edge(&g, start - 1, end - 1);
    }
    
    print_adj_mat(&g);
    
    int visited[SIZE] = {FALSE};
    printf("\n깊이 우선 탐색을 시작할 정점 입력 : ");
    scanf("%d", &v);
    printf("\n");
    
    dfs_mat(&g, visited, v - 1);
    printf("\n");
    
    rev_mat(&g, v);
    printf("\n");
    
}
```

### Floyd-Warshall
```c
#include <stdio.h>
#include <stdlib.h>

#define SIZE 10
#define INF 1000000

typedef struct GraphType
{
    int n;
    int weight[SIZE][SIZE]; // 인접행렬
}GraphType;

int A[SIZE][SIZE];

void printA(GraphType* g)
{
    int i, j;
    printf("=============================\n");
    
    for(i = 0; i < g->n; i++)
    {
        for(j = 0; j < g->n; j++)
        {
            if(A[i][j] == INF)
                printf("  * ");
            else
                printf("%3d ", A[i][j]);
        }
        printf("\n")    ;
    }
    
    printf("=============================\n");
}

void floyd(GraphType* g)
{
    int i, j, k;
    
    for(i = 0; i < g->n; i++)
        for(j = 0; j < g->n; j++)
            A[i][j] = g->weight[i][j];
    
    printA(g);
    
    for(k = 0; k < g->n; k++){
        for(i = 0; i < g->n; i++)
            for(j = 0; j < g->n; j++)
                if(A[i][k] == 1 && A[k][j] == 1)
                    A[i][j] = 1;
           //0,1(경로 유무)가 아니라 가중치값이 저장되어 있을때 최적경로 찾기
                //if(A[i][k] + A[k][j] < A[i][j])
                //  A[i][j] = A[i][k] + A[k][j];
                
        printA(g);
    }
}

void main()
{
    GraphType g = {5,
    {{0, 1, INF, 1, INF},
    {INF, 0, INF, INF, INF},
    {1, INF, 0, INF, INF},
    {INF, INF, INF, 0, 1},
    {1, INF, INF, INF, 0}}
    };
    
    floyd(&g);
}
```

### 경로찾기
```c
#include <stdio.h>
#include <stdlib.h>

#define M 100
#define N 100

int map[M][N];
int path[M][N];

int num_path(int m, int n) // 재귀
{
    if(map[m][n] == 0)
        return 0;
    
    if(m == 0 && n == 0)
        return 1;
    
    return ((m > 0) ? num_path(m - 1, n) : 0) + ((n > 0) ? num_path(m, n - 1) : 0);
}

int calc_path(int m, int n)
{
    int i, j;
    path[0][0] = map[0][0];
    
    for(i = 1; i < m; i++)
        if(map[i][0] == 0)
            path[i][0] = 0;
        else
            path[i][0] = path[i - 1][0];
    
    for(j = 1; j < n; j++)
        if(map[0][j] == 0)
            path[0][j] = 0;
        else
            path[0][j] = path[0][j - 1];
    
    for(i = 1; i < m; i++)
        for(j = 1; j < n; j++)
            if(map[i][j] == 0)
                path[i][j] = 0;
            else
                path[i][j] = path[i - 1][j] + path[i][j - 1];
    
    return path[m - 1][n - 1];
}

void print(int m, int n)
{
    for(int i = 0; i < m; i++)
    {
        for(int j = 0; j < n; j++)
            printf("%d ", path[i][j]);
        printf("\n");
    }
}

void main()
{
    int m, n;
    scanf("%d %d", &m, &n);
    
    for(int i = 0; i < m; i++)
        for(int j = 0; j < n; j++)
            scanf("%d", &map[i][j]);
    
    //printf("%d\n", num_path(m - 1, n - 1));
    printf("%d\n", calc_path(m, n));
    print(m, n);
}
```

### 가중치가 있는 경로찾기(가장 높은 선호도)
최고의 선호도 값이 나오는 경로 찾는 코드 추가해주기
```c
#include <stdio.h>
#include <stdlib.h>

#define M 100
#define N 100

#define max(x, y) ((x) > (y) ? (x) : (y))

int map[M][N];
int joy[M][N];

void calc_joy(int m, int n)
{
    int i, j;
    joy[0][0] = map[0][0];
    
    for(i = 1; i < m; i++)
       joy[i][0] = joy[i - 1][0] + map[i][0];
    
    for(j = 1; j < n; j++)
        joy[0][j] = joy[0][j - 1] + map[0][j];
    
    for(i = 1; i < m; i++)
        for(j = 1; j < n; j++)
            joy[i][j] = max(joy[i - 1][j], joy[i][j - 1]) + map[i][j];
}

void print(int m, int n)
{
    for(int i = 0; i < m; i++)
    {
        for(int j = 0; j < n; j++)
            printf("%02d ", joy[i][j]);
        printf("\n");
    }
}

void main()
{
    int m, n;
    scanf("%d %d", &m, &n);
    
    for(int i = 0; i < m; i++)
        for(int j = 0; j < n; j++)
            scanf("%d", &map[i][j]);
    
    calc_joy(m, n);
    printf("\n");
    print(m, n);
}
```
