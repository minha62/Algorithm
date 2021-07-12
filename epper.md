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
