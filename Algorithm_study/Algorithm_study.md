## 알고리즘이란?

- 어떠한 문제를 해결하기 위해 정해진 일련의 절차나 방법을 공식화한 형태로 표현한 것
- 문제 해결 방식 / 문제 풀이 패러다임
- 시간복잡도/공간복잡도를 지표로 사용하여 알고리즘의 성능을 평가



## 빅-오 표기법 (Big-O notation)

- 시간/공간복잡도를 표현하는 점근적 표기 방식
- 최악의 경우 계산
- 시간/공간복잡도의 가장 영향력 있는 항으로 표현. 계수는 무시



## 공간 복잡도 (space complexity)

- 입력 크기와 사용한 메모리 크기의 함수 관계
- 프로그램 실행 및 완료 시 필요한 저장공간의 양
- 총 공간 요구 = 고정 공간 요구 + 가변 공간 요구 : S(P) = c + Sp(n)
    - 고정 공간 : 입력과 관계없음 (상수취급)
    - 가변 공간 : 입력과 관계있음 (알고리즘의 공간 복잡도 계산)


```c
int factorial(int n){
	if(n>1) return n*factorial(n-1);
	else return 1;
}
```

재귀적으로 parameter에 1이 들어올 때까지 쌓이다가 종료 ⇒ O(n)


```c
int factorial(int n){
	int fac = 1;
	for(int i=1; i<=n; i++)
		fac *= i;
	return fac;
}
```

상수 크기의 저장공간만 필요 ⇒ O(1)



## 시간 복잡도 (time complexity)

- 입력 크기와 실행시간 함수 관계
- 프로그램 실행 및 완료 시 필요한 연산의 양
- 구상한 알고리즘의 시간복잡도를 계산하는 것이 문제풀이에 앞서 매우 중요


```c
int sum(int n){
	int ret = 0;
	for(int i=1; i<=n; i++)
		ret += i;
	return ret;
}
```

for 반복문의 연산 n번 ⇒ O(n)


```c
int sum(int n){
	return n*(n+1)/2;
}
```

바로 sum을 구함 ⇒ O(1)



---


[1. Greedy](Greedy.md)

[2. Bruteforce & Backtracking](Brute.md)

### 3. Dynamic Programming

### 4. Sorting, Binary Search

### 5. Two Pointer

### 6. Stack, Queue&Deque

### 7. Graph(DFS, BFS)

### 8. Tree

### 9. Priority Queue, Dijkstra

### 10. Bellman-Ford, Floyd-Warshall

### 11. Map, Set

### 12. 동적 계획법

### 13. MST, Disjoint Set

### 14. 분할 정복

### 15. Segment Tree

### 16. LCA, Sparse Table

### 17. 최대 유량

### 18. 게임 이론

### 19. Randomized Algorithm


