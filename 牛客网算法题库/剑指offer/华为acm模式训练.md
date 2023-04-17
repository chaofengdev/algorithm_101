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

## 计算某字符出现次数

方法1：系统函数

```java
import java.util.Scanner;

// 注意类名必须为 Main, 不要有任何 package xxx 信息
public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String input1 = sc.nextLine().toLowerCase();
        String input2 = sc.nextLine().toLowerCase();
        int res = input1.length() - input1.replaceAll(input2,"").length();
        System.out.println(res);
    }
}
```

方法2：计数+转换为小写

因为不区分大小写，所以都转换为小写。

```java
import java.util.Scanner;

// 注意类名必须为 Main, 不要有任何 package xxx 信息
public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String str = sc.nextLine().toLowerCase();
        char c = sc.nextLine().toLowerCase().charAt(0);
        int count = 0;
        for(int i = 0; i < str.length(); i++) {
            if(str.charAt(i) == c) {
                count++;
            }
        }
        System.out.println(count);
    }
}
```

## 明明的随机数

方法1：利用TreeSet进行排序和去重

> TreeSet的用法和迭代器的相关用法

```java
import java.util.Scanner;

import java.util.*;
// 注意类名必须为 Main, 不要有任何 package xxx 信息
public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int num = sc.nextInt();
        //创建TreeSet进行去重排序
        TreeSet set = new TreeSet();
        for(int i = 0; i < num; i++) {
            set.add(sc.nextInt());
        }
        Iterator iterator = set.iterator();
        while(iterator.hasNext()) {
            System.out.println(iterator.next());
        }
    }
}
```

方法2：空间换时间

> 定义一个数组，大小为501，输入一个1-501的树时，将数组对应下标的改为1；
>
> 然后再从小到大循环数组中值为1的下标输出，因为下标本身有序所以不用排序。

```java
import java.util.Scanner;

// 注意类名必须为 Main, 不要有任何 package xxx 信息
public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int[] arr = new int[501];
        int num = sc.nextInt();
        //将数据放入数组
        for(int i = 0; i < num; i++) {
            int n = sc.nextInt();
            arr[n] = 1;
        }
        //打印数组中的部分元素
        for(int i = 0; i < arr.length; i++) {
            if(arr[i] == 1) {
                System.out.println(i);
            }
        }
    }
}
```

方法3：

> 先将读入的随机数排序，然后计数，比较发现当前数字与上一数字相同，则跳过该数字。

```java
import java.util.Scanner;

import java.util.*;
// 注意类名必须为 Main, 不要有任何 package xxx 信息
public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        while(sc.hasNext()) {
            int count = sc.nextInt();
            int[] data = new int[count];
            for(int i = 0; i < count; i++) {
                data[i] = sc.nextInt();
            }
            Arrays.sort(data);
            System.out.println(data[0]);
            for(int i = 1; i < count; i++) {//去重
                if(data[i] != data[i - 1]) {
                    System.out.println(data[i]);
                }
            }
        }
    }
}
```

