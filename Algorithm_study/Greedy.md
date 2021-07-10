# Greedy

- 가장 직관적인 알고리즘 설계 패러다임
- 모든 선택지를 고려하지 않고, 각 단계마다 가장 좋은 방법만 선택
- 시공간 복잡도의 향상된 방법으로 문제 해결 가능
- Greedy는 많은 경우 최적의 정답을 찾아주지 못함

    ⇒ Greedy 알고리즘을 사용할 때, 그 정당성을 증명하는 것이 중요



## Greedy 정당성 증명

1. **탐욕적 선택 속성 (greedy choice property)**
    - 각 단계에서 greedy로 내리는 선택은 항상 최적해로

        ⇒ 즉 greedy한 선택을 해서 손해 볼 일 없다는 것


 2.  **최적 부분 구조(optimal substructure)**

    - 부분 문제의 최적해에서 전체 문제의 최적해를 만들 수 있음
    - 대부분 자명
    - greedy가 아니더라도 성립할 수 있음 (ex.다이나믹 프로그래밍)


- 최적 부분 구조라면, 전체 문제를 부분문제들의 최적해를 구하는 것으로 치환
- 탐욕적 선택 속성을 만족한다면 그리디한 접근을 통해 부분문제의 최적해를 구할 수 있음을 보여주므로 정당성 증명
- 최적해를 얻을 수 있는 접근이 직관적이지 않은 경우도 많기 때문에 정당성 증명 과정을 빼먹지 않고 연습하기!


## 백준

- #1080 행렬
- #1339 단어 수학
- #1700 멀티탭 스케줄링
- #1736 쓰레기 치우기
- #1931 회의실 배정

      **회의가 끝나는 시간을 기준으로 정렬 후, 현재 기준에서 가능한 회의 중 가장 빨리 끝나는 회의를 고르면 된다.**

    ```cpp
    #include<iostream>
    #include<algorithm>
    #include<vector>

    using namespace std; // 표준 namespace 사용
    int n, m, a, b;
    int main() {
    	ios_base::sync_with_stdio(0);
    	cin.tie(0), cout.tie(0);
    // 위 두 줄 왠만하면 넣기. 표준 입출력 속도 향상

    	cin >> n; // n 입력
    	vector<pair<int, int>>v;//pair는 자료형 2개를 담을 수 있음
    	
      for (int i = 0; i < n; i++) {
    		cin >> a >> b;
    		v.push_back({ b,a });
    	} //시작시간과 종료시간을 입력받아 {종료,시작}순으로 vector에

      //sort함수는 pair자료형의 앞에 자료 기준으로 오름차순 정렬
    	sort(v.begin(), v.end());

    //a는 지금까지 회의 중 가장 늦게 끝난 시각. b는 회의 총 개수
    	a = 0, b = 0; 
    	for (int i = 0; i < v.size(); i++) { //v.size()==n
    		if (v[i].second < a)continue;//시작시간(v[i].second)이 a보다 작으면 무시
    		a = v[i].first; //아니라면 이 회의를 선택하고 늦게 끝난 시각 갱신
    		b++; // 회의 개수 증가
    	}
    	cout << b; // 회의 개수 출력
    }
    ```

    ```java
    import java.util.Arrays;
    import java.util.Comparator;
    import java.util.Scanner;

    public class Main {
    	public static void main(String[] args) {
    		Scanner in = new Scanner(System.in);
    		int n = in.nextInt();
    		int v [][] = new int[n][2];
    		
    		for(int i = 0; i < n; i++) {
    			v[i][0] = in.nextInt();
    			v[i][1] = in.nextInt();
    		}
    		
    		Arrays.sort(v, new Comparator<int[]>() {
    			public int compare(int[] e1, int[] e2) {
    				if(e1[1] == e2[1]) {
    					return e1[0] - e2[0];
    				}
    				return e1[1] - e2[1];
    			}
    		});
    		
    		int a = 0, b = 0;
    		
    		for(int i = 0; i < n; i++) {
    			if(v[i][1] < a)continue;
    			a = v[i][0];
    			b++;
    		}
    		
    		System.out.println(b);
    		
    		in.close();
    	}

    }
    ```

    ```python
    n = int(input())
    v = []

    for i in range(n):
        a, b = map(int, input().split())
        v.append([b, a])
    v.sort()

    a = 0; b = 0
    for i in range(n):
        if v[i][1] < a: continue
        a = v[i][0]
        b += 1
    print(b)
    ```

- #2839 설탕 배달
- #2847 게임을 만든 동준이

    수열의 수를 하나 선택하여 1씩 감소시킬 수 있을때, 증가수열(오름차순정렬)이 되도록 하는 최소의 감소 횟수를 구하자

    ⇒ 감소연산만 사용할 수 있다는 것이 핵심

    1. 수열의 각 원소를 a0,a1,a2, ... , an-1
    2. an-1을 감소시키지 않았을 때의 최적해를 x라고 하자(an-1을 1감소 시켰을때, 최적해는 x보다 큼)
    3. 연산 이후 0부터 n-2번까지 수열의 값을 b0, ... , bn-2라고 하자.
    4. b0<b1<b2<...<bn-2<an-1을 만족하는 집합을 A

        b0<b1<b2<...<bn-2<(an-1)-1을 만족하는 집합을 B

        ⇒ B⊂A

        ⇒ A에서의 최적해 ≤ B에서의 최적해 Q.E.D

    5. 마지막 원소를 수열에서 제외
    6. 남은 수열의 마지막 원소를 min(제외
- #4796 캠핑
- #11047 동전 0
- #11092 Safe Passage
- #11399 ATM
- #12931 두 배 더하기
- #13904 과제
- #14659 한조서열정리하고옴ㅋㅋ

    naive하게 접근하면, 각 봉우리의 한조들에 대해 활을 쏘며 자기보다 높은 봉우리가 나올 때까지 체크. 시간복잡도 O(n^2). 비효율적 

    ⇒ 자기보다 뒤에 있는 한조인데, 봉우리 높이까지 작거나 같다면 볼 필요가 없음.

    1. 산봉우리의 Max 값을 저장할 변수, 현재 Max가 처치한 적을 저장할 변수 생성
    2. 순서대로 확인하면서 Max보다 작은 산봉우리의 경우 적을 죽임 
    3. Max보다 큰 경우 Max를 업데이트, 처치한 적의 수 0으로 초기화
    4. 지금까지 처치한 적의 수의 최댓값 출력 ⇒ O(n)

    ```cpp

    #include<iostream>
    #include<algorithm>
    #include<vector>

    using namespace std;
    int n, a, cur_max, ans; // 전역변수는 0으로 자동 초기화

    int main() {
    	ios_base::sync_with_stdio(0);
    	cin.tie(0), cout.tie(0);
    	cin >> n;
    	vector<int>v;
    	for (int i = 0; i < n; i++) {
    		cin >> a;
    		v.push_back(a);
    	}

    	a = 0; // 현재까지 처치한 적의 수

    /*만약 현재의 봉우리 높이가 이전까지의 봉우리의 최대 높이보다 크거나 같다면
    높이의 최댓값을 갱신하고, 현재까지 처치한 적의 수와 이전까지의 답 중 더 큰 값을 답으로 갱신
    그렇지 않으면 처치한 적의 수를 한 명 늘림*/
    	for (int i = 0; i < n; i++) {
    		if (cur_max <= v[i]) {
    			cur_max = v[i];
    			ans = max(ans, a);
    			a = 0;
    		}
    		else a++;
    	}
    	ans = max(ans, a);
    	cout << ans;
    }
    ```

    ```java
    import java.util.Scanner;

    public class Main {

    	public static void main(String[] args) {
    		Scanner in = new Scanner(System.in);
    		
    		int n = in.nextInt();
    		int v[] = new int[n];
    		
    		for(int i = 0; i < n; i++) {
    			v[i] = in.nextInt();
    		}
    		int max = 0, ans = 0, a = 0;
    		for(int i = 0 ;i < n; i++) {
    			if(max < v[i]) {
    				max = v[i];
    				ans = ans > a ? ans : a;
    				a = -1;
    			}
    			a++;
    		}
    		ans = ans > a ? ans : a;
    		System.out.println(ans);
    		
    		in.close();
    	}

    }
    ```

    ```python
    n = int(input())
    v = list(map(int, input().split()))
    cur_max = 0; ans = 0; a = 0
    for i in range(n):
        if cur_max < v[i]:
            cur_max = v[i]
            ans = max(ans, a)
            a = -1
        a += 1

    ans = max(ans, a)
    print(ans)
    ```

- #15553 난로
- #16206 롤케이크
- #16288 Passport Control
- #17954 투튜브
- #19582 200년간 폐관수련했더니 PS 최강자가 된 사건에 대하여
- #19590 비드맨
- #19639 배틀로얄
- #20044 Project Teams

    1. 팀의 최소값이 최대가 되도록 팀을 짜야 함.
    2. 정렬한 후 수열의 각 원소를 a0,a1,a2,...,a2n-1이라고 하자.
    3. greedy하게 접근 ⇒ (a0+a2n-1),(a1+a2n-2),...,(an-1+an) 중 min값
    4. 크기순으로 정렬했을때 작은 n개의 원소를 그룹A, 큰 n개의 원소를 그룹B로 묶기
    5. 반드시 A원소 하나, B원소 하나를 팀으로 묶기

    어떤 팀(𝑎𝑥, 𝑏𝑥) (𝑎𝑦, 𝑏𝑦) 에 대해, 만약(𝑎𝑥< 𝑎𝑦)이고, (𝑏𝑥< 𝑏𝑦)라면 (𝑎𝑥, 𝑏𝑦) , (𝑎𝑦, 𝑏𝑥)이 반드시 이득 ⇒ 최소값을 최대화해야 하므로

    ```cpp
    #include<iostream>
    #include<algorithm>
    #include<vector>
    using namespace std;

    int n, a;
    vector<int> v;
    int main() {
    	cin >> n;
    	n *= 2; // 2n개의 input이 들어옴
    	for (int i = 0; i < n; i++) {
    		cin >> a;
    		v.push_back(a);
    	}
    	sort(v.begin(), v.end()); // 오름차순 정렬
    	a = 1234567891; //문제에서 주어진 입력값으로 절대 나올 수 없는 큰 값으로 초기화
    	for (int i = 0; i < n; i++) {
    		a = min(a, v[i] + v[n - 1 - i]);
    	}
    	cout << a;
    }
    ```

    ```java
    package Prob_20044;

    import java.util.Arrays;
    import java.util.Scanner;

    public class Main {

    	public static void main(String[] args) {
    		Scanner in = new Scanner(System.in);
    		int n = in.nextInt();
    		n *= 2;
    		int v [] = new int [n];
    		
    		for(int i = 0; i < n; i++) {
    			v[i] = in.nextInt();
    		}
    		Arrays.sort(v);
    		int a = 1234567891;
    		for(int i = 0; i < n; i++) {
    			a = a < (v[i] + v[n - 1 - i]) ? a : (v[i] + v[n - 1 - i]);
    		}
    		
    		System.out.println(a);
    		
    		in.close();
    	}

    }
    ```

    ```python
    n = int(input())
    n *= 2
    v = list(map(int, input().split()))
    v.sort()
    a = 1234567891
    for i in range(n):
        a = min(a, v[i] + v[n-1-i])
    print(a)
    ```
