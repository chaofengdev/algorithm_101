## 字符串最后一个单词的长度

方法1：反过来打印

```java
import java.util.Scanner;

// 注意类名必须为 Main, 不要有任何 package xxx 信息
public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String str = sc.nextLine();
        int res = 0;
        for(int i = str.length() - 1; i >= 0; i--) {
            if(str.charAt(i) == ' ') {//str.substring(i,i+1).equals(" ")
                break;
            }
            res++;
        }
        System.out.println(res);
    }
}
```

方法2：系统函数

```java
import java.util.Scanner;

// 注意类名必须为 Main, 不要有任何 package xxx 信息
public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String str = sc.nextLine();
        String[] s = str.split(" ");//str.split("\\s+")
        int res = s[s.length - 1].length();
        System.out.println(res);
    }
}
```

