## 16회 기출문제

### 12-2
신입생 오리엔테이션에 참석할 학생들에게 (1번부터 시작되는) 번호표를 주었다. 이 번호표는 학생들이 머무르는 방의 번호를 결정하고, (게임 등을 하기 위해) 같은 방을 쓰는 학생들에게도 내부적으로 일련번호를 주기 위해 쓰인다. 각 방은 15명씩 배정되고, 같은 방에 배정된 학생들의 경우는 번호표 순으로 방안에서의 번호가 결정된다. 학생에게 줄 번호표를 입력받아 그 학생의 방 번호와 방안 에서의 번호를 결정해 주는 프로그램을 작성하시오.
[입력 형식] - 번호표 n을 입력한다. ( ≤  ≤ )
[출력 형식] - 방 번호와 방안에서의 번호를 공백으로 구분하여 순서대로 출력한다.

```java
import java.util.Scanner;

public class pm12_2 {

	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		
		int n = sc.nextInt();
		int room = 0;
		int num = 0;
		
		if(n%15 == 0) {
			room = n/15;
			num = 15;
		}
		
		else {
			room = n/15 + 1;
			num = n%15;
		}
		
		System.out.println(room + " " + num);
	}

}
```

### 15-5
"0"과 1로만 이루어진 비트열의 경우에는 같은 비트가 연속해서 등장하는 횟수들을 기록하여 저장 공간을 줄일 수 있다. 예) "00011110" => "CDA", 비트열이 "1"로 시작하는 경우에는 저장 공간의 제일 앞에 "1"을 붙여 혼돈을 방지한다. 예) "110100" => "1BAAB", "111100100011" => "1DBACB"
```java
import java.util.Scanner;

public class pm15_5 {
	
	public static void bit_to_char(String str) {
		
		String bit = "";
		int cnt = 0;
		
		if(str.charAt(0) == '1')
			bit += '1';
		
		for(int i = 0; i < str.length() - 1; i++) {
			if(str.charAt(i) == str.charAt(i + 1))
				cnt++;
			else {
				bit += (char)('A' + cnt);
				cnt = 0;
			}
		}
		
		bit += (char)('A' + cnt);
		
		System.out.println(bit);

	}
	
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		
		String str = sc.next();
		bit_to_char(str);
		
		sc.close();
	}

}
```

### 11-7
두 수의 평균만 구할 수 있는 단순 계산기가 있다. n개의 점수 목록이 주어졌을 때, 단순 계산기는 다음과 같은 방법으로 평균을 구한다.
1. 처음에는 두 점수를 목록에서 선택해, 두 점수의 평균 m을 구한다. (선택한 두 점수는 목록에서 제거)
2. 목록에 남아 있는 점수 중 하나를 선택해 앞서 구한 평균 m과의 평균을 구한다.(새롭게 구한 평균이 다음 점수와 계산할 평균 m이 된다.)
3. 목록에 남은 점수가 없을 때까지 (2)를 반복한다. (2에서 선택한 점수는 제거)

목록에서 점수를 선택하는 순서에 따라 평균이 달라진다. n개의 점수가 주어졌을 때, 가장 큰 평균값을 구하는 프로그램 작성
[입력] 첫째 줄에 n을 입력(1<=n<=20), n개의 줄에 걸쳐 점수 x를 입력(1<=x<=5)
[출력] 가장 큰 평균값 출력(소수 이하 6째 자리까지)

```java
import java.util.Scanner;

public class pm11_7 {
	
	public static void insertion_sort(int arr[], int n) {
		int i, j, key;
		
		for(i = 1; i < n; i++) {
			key = arr[i];
			
			for(j = i - 1; j > 0 && arr[j] > key; j--)
				arr[j + 1] = arr[j];
			
			arr[j + 1] = key;
		
		}
	}
	
	public static double average(int arr[], int n) {
		if(n == 1)
			return arr[0];
		
		double avg = (arr[0] + arr[1]) / 2.0;
		
		for(int i = 2; i < n; i++)
			avg = (avg + arr[i]) / 2.0;
		
		return avg;
	}

	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		
		int n = sc.nextInt();
		int arr[] = new int[n];
		double avg = 0.0;
		
		for(int i = 0; i < n; i++) {
			arr[i] = sc.nextInt();
		}
		
		insertion_sort(arr, n);
		avg = average(arr, n);
		
		System.out.println(String.format("%.6f", avg));
		
		sc.close();

	}

}
```
##### 가장 작은 수부터 연쇄적으로 평균을 구할 때 가장 큰 평균 ==> 숫자 배열 오름차순 정렬
##### 배열 원소 1개인 경우도 고려

### 13-2 
거스름돈을 가장 적은 화폐 개수로 내어 주는 프로그램을 작성(단, 화폐입력은 10원 단위, 화폐종류는 5만, 1만, 5천, 1천, 5백, 1백, 50, 10원권 총 8종)
- [입력] 첫 번째 줄에 지불한 금액 m을 입력(10<=m<=100000), 두 번째 줄에 물품가격 n을 입력(10<=n<=m<=100000)
- [출력] 거스름돈으로 지불한 화폐 종류 개수와 총 화폐 개수를 공백으로 구분하여 출력
##### 그리디 알고리즘
```java
import java.util.Scanner;

public class pm13_2 {

	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		
		int money[] = {50000, 10000, 5000, 1000, 500, 100, 50, 10};
		
		int m = sc.nextInt();
		int n = sc.nextInt();
		int change = m - n;
		int cnt = 0, sort = 0;
		boolean isUsed;
		
		for(int i = 0; i < 8; i++) {
			isUsed = false;
			
			while(change >= money[i]) {
				cnt++;
				change -= money[i];
				isUsed = true;
			}
			
			if(isUsed)
				sort++;
		}
		
		System.out.println(sort + " " + cnt);
		sc.close();
	}

}
```
### 13-2 (재귀)
n개의 요소를 갖는 배열인 회문(palindrome; 앞으로 읽으나 뒤로 읽으나 동일)을 만들자. 회문이 아닌 배열은 다음 규칙에 따라 수정해 회문으로 만든다. 수정 규칙은 인접한 요소끼리 합해서 n-1개의 요소를 갖는 배열로 만드는 방법이다. ex) {1, 3, 5, 7} => {4, 5, 7}

회문이 아닌 배열이 주어졌을 때, 몇 번의 수정을 통해 회문이 만들어지는지 횟수를 구하는 프로그램을 작성하시오.

[입력]
- 첫 번째 줄에 배열을 구성하는 요소의 개수 n을 입력 (1<=n<=10)
- 두 번째 줄에 n개의 숫자 Xi를 공백으로 구분하여 입력 (1<=Xi<=100)

[출력]
- 회문이 만들기까지 최소한의 배열 수정 횟수

```java
import java.util.Scanner;

public class pm13_7 {
	static int cnt;
	
	public static int palindrome(int x[], int start, int end) {
		if(start >= end)
			return cnt;
		
		if(x[start] == x[end]) 
			return palindrome(x, start + 1, end - 1);
		else {
			if(x[start] < x[end]) {
				x[start + 1] += x[start];
				cnt++;
				
				return palindrome(x, start + 1, end);
			}
			
			else {
				x[end - 1] += x[end];
				cnt++;
				
				return palindrome(x, start, end - 1);
			}
		}
	}

	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		
		int n = sc.nextInt();
		int x[] = new int[n];
		int start = 0, end = n - 1;
		int cnt;
		
		for(int i = 0; i < n; i++) {
			x[i] = sc.nextInt();
		}
		
		cnt = palindrome(x, start, end);
		System.out.println(cnt);
		
		sc.close();

	}

}
```

### 11-9 (재귀)
두 쌍의 괄호로 만들 수 있는 괄호 배열은 "(())", "()()" 두 가지다. 세 쌍의 괄호로 만들 수 있는 올바른 괄호 배열은 "((()))", "(()())", "(())()" , "()(())", "()()()" 다섯 가지다. 양의 정수 n을 입력받아서 n쌍의 괄호로 만들 수 있는 올바른 괄호 배열의 개수를 구하는 프로그램을 작성하시오.

[입력]
- 양의 정수 n을 입력 (1<=n<=10)

[출력]
- n쌍으로 가능한 올바른 괄호 배열의 개수 출력

```java
import java.util.Scanner;

public class pm11_9 {	
	public static int sol(int l, int r) {
		if(l == 0 && r == 0) //종료조건
			return 1; 
		
		else {
			if(l == 0) //오른쪽 괄호만 남음
				return 1; //남은 오른쪽 괄호 전부 사용
			
			else if(l < r) // l > r은 올바른 괄호 배열이 아니므로 고려 안함
				return sol(l - 1, r) + sol(l, r - 1); // 어느쪽 괄호를 사용해도 괜찮
			
			else // l == r
				return sol(l - 1, r); // 왼쪽 괄호만 사용 가능
		}
		
	}

	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		
		int n = sc.nextInt();
		int cnt;
		
		cnt = sol(n, n); //2n개 괄호 사용
		System.out.println(cnt);
	}

}
```
