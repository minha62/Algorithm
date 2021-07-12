### 16회 기출문제

12-2
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
