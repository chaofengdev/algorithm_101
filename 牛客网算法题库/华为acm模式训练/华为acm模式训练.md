## HJ1 字符串最后一个单词的长度

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

## HJ2 计算某字符出现次数

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

## HJ3 明明的随机数

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

## HJ4 字符串分隔

方法1：借助substring()

> 1. 需要输入字符串，用到Scanner和hasNext()。
>    （1）建立 Scanner sc = new Scanner(System.in);
>    （2）判断有无输入用sc.hasNext().接收字符串使用sc.nextLine().
> 2. 一次性接受全部的字符串，对8取余，获知需要补0的位数。使用StringBuilder中的append()函数进行字符串修改，别忘了toString()。
>    字符串缓冲区的建立：StringBuilder sb = new StringBuilder();
> 3. 输出时，截取前8位进行输出，并更新字符串。用到str.substring()函数：
>    （1）str.substring(i)意为截取从字符索引第i位到末尾的字符串。
>    （2）str.substring(i,j)意为截取索引第i位到第（j-1）位字符串。包含i，不包含j。

```java
import java.util.Scanner;

// 注意类名必须为 Main, 不要有任何 package xxx 信息
public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        while(sc.hasNext()) {
            String str = sc.nextLine();
            //int size = str.length();
            //需要补0的位数
            int help = 8 - str.length()%8;
            //建立字符串缓冲区
            StringBuilder sb = new StringBuilder(str);
            while(help > 0 && help < 8) {
                sb.append("0");
                help--;
            }
            //利用substring分割字符串
            String temp = sb.toString();
            while(temp.length() > 0) {
                System.out.println(temp.substring(0,8));//截取从第0位到第7位字符串
                temp = temp.substring(8);//截取从字符串第8位到末尾的字符串
            }
        }
    }
}
```

方法2：手动处理分割逻辑

```java
import java.util.Scanner;

// 注意类名必须为 Main, 不要有任何 package xxx 信息
public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        while(sc.hasNext()) {
            String str = sc.nextLine();
            //需要补0的长度
            int len = 8 - str.length()%8;
            while(len > 0 && len < 8) {
                str += "0";
                len--;
            }
            //System.out.println(str);
            //8个一组输出字符串
            StringBuilder sb = new StringBuilder();
            for(int i = 0; i < str.length(); i++) {
                //int j = 0;
                if(i > 0 && i % 8 == 0) {//i > 0
                    System.out.println(sb.toString());
                    sb.setLength(0);//清空缓冲区
                }
                sb.append(str.charAt(i));
            }
            // 输出最后一组剩余的字符
            System.out.println(sb.toString());
        }
    }
}
```

## HJ5 进制转换

方法1：Integer.parseInt(str,16);

```java
import java.util.Scanner;

// 注意类名必须为 Main, 不要有任何 package xxx 信息
public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        while(sc.hasNext()) {
            String str = sc.nextLine();
            String str_temp = str.substring(2,str.length());
            int res = Integer.parseInt(str_temp,16);
            System.out.println(res);
        }
    }
}
```

方法2：Map集合

> 注意java8新特性，双括号初始化

```java
import java.util.*;

// 注意类名必须为 Main, 不要有任何 package xxx 信息
public class Main {
    public final static int BASE = 16;
    public static Map<Character, Integer> map = new HashMap<Character, Integer>() {//java8新特性–双括号初始化
        {
            put('0', 0);
            put('1', 1);
            put('2', 2);
            put('3', 3);
            put('4', 4);
            put('5', 5);
            put('6', 6);
            put('7', 7);
            put('8', 8);
            put('9', 9);
            put('A', 10);
            put('B', 11);
            put('C', 12);
            put('D', 13);
            put('E', 14);
            put('F', 15);
            put('a', 10);
            put('b', 11);
            put('c', 12);
            put('d', 13);
            put('e', 14);
            put('f', 15);
        }
    };
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        while (sc.hasNext()) {
            String str = sc.nextLine();
            int res = getDecimal(str);
            System.out.println(res);
        }
    }
    public static int getDecimal(String number) {
        int res = 0;
        for (char ch : number.toCharArray()) {
            res = res * BASE + map.get(ch);//本题核心
        }
        return res;
    }
}
```

方法3：通用方法

```java
import java.util.Scanner;

// 注意类名必须为 Main, 不要有任何 package xxx 信息
public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        // 注意 hasNext 和 hasNextLine 的区别
        while (sc.hasNext()) { // 注意 while 处理多个 case
            String str = sc.nextLine();
            int count = 0;
            for(int i = 0; i < str.length() - 2; i++) {
                char tc = str.charAt(i + 2);//由于前面两位是'0x'，故从第三位开始
                int t = 0;//记录字母转换成的数值
                //将字母转换成数字
                if(tc >= '0' && tc <= '9') {
                    t = tc - '0';
                }else if(tc >= 'A' && tc <= 'F') {
                    t = tc - 'A' + 10;
                }else if(tc >= 'a' && tc <= 'f') {
                    t = tc - 'a' + 10;
                }
                //计算加和
                count = count * 16 + t;// count = count + t * Math.pow(16,s.length()-i-3);这个比较难想到
            }
            System.out.println(count);
        }
    }
}
```

## HJ6 质数因子

方法1：除法

```java
import java.util.Scanner;

// 注意类名必须为 Main, 不要有任何 package xxx 信息
public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        while(sc.hasNext()) {
            long num = sc.nextLong();
            long len = (long) Math.sqrt(num);//减少时间复杂度
            for(long i = 2; i <= len; i++) {
                while(num % i == 0) {//这里是while不是if，因为质数因子可以重复
                    System.out.print(i + " ");
                    num = num / i;
                }
            }
            System.out.println(num == 1 ? "": num+" ");//补上可能大于Math.sqrt(num)的质数因子
        }
    }
}
```

## HJ7 取近似值

方法1：暴力

```java
import java.util.Scanner;

// 注意类名必须为 Main, 不要有任何 package xxx 信息
public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        while(sc.hasNext()) {
            double num = sc.nextDouble();
            double help = num - (int) num;//小数
            int res = (int) num;//整数
            if(help >= 0.5) {
                System.out.println(res+1);
            }else {
                System.out.println(res);
            }
        }
    }
} 
```

方法2：取巧

```java
import java.util.Scanner;

// 注意类名必须为 Main, 不要有任何 package xxx 信息
public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        while(sc.hasNext()) {
            double num = sc.nextDouble();
            System.out.println((int)(num + 0.5));
        }
    }
} 
```

## HJ8 合并表记录

方法1：利用哈希表

```java
import java.util.*;

// 注意类名必须为 Main, 不要有任何 package xxx 信息
public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int size = sc.nextInt();
        Map<Integer,Integer> table = new HashMap<>(size);//能够排序的关键在于建立与数据量一样大的哈希表
        //采用hash表存放数据 利用底层数据结构排序
        for(int i = 0; i < size; i++) {
            int key = sc.nextInt();
            int value = sc.nextInt();
            if(table.containsKey(key)) {
                table.put(key,table.get(key) + value);
            }else {
                table.put(key,value);
            }
        }
        //输出已经有序的记录
        for(Integer key : table.keySet()) {
            System.out.println(key + " " + table.get(key));
        }
    }
}
```

方法2：利用TreeMap

```java
import java.util.*;

// 注意类名必须为 Main, 不要有任何 package xxx 信息
public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        Map<Integer,Integer> map = new TreeMap<Integer,Integer>(new Comparator<Integer>() {
            //@override
            public int compare(Integer o1, Integer o2) {
                return o1 - o2;
            }
        });
        while(sc.hasNext()) {
            int len = sc.nextInt();
            //将元素存放到map中
            for(int i = 0; i < len; i++) {
                int key = sc.nextInt();
                int value = sc.nextInt();
                if(map.containsKey(key)) {
                    map.put(key,map.get(key) + value);
                }else {
                    map.put(key,value);
                }
            }
            //输出排序后的元素
            for(Integer key : map.keySet()) {
                System.out.println(key + " " + map.get(key));
            }
            map.clear();
        }
    }
}
```

## HJ9 提取不重复的整数

方法1：利用整数除法性质和HashSet去重

```java
import java.util.*;

// 注意类名必须为 Main, 不要有任何 package xxx 信息
public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        while(sc.hasNext()) {
            HashSet<Integer> set = new HashSet<>();
            int target = sc.nextInt();
            while(target != 0) {
                int temp = target % 10;
                if(!set.contains(temp)) {
                    set.add(temp);
                    System.out.print(temp);
                }
                target /= 10;
            }
            System.out.println();
        }
    }
}
```

方法2：利用str.indexOf(ch)==i

```java
import java.util.*;

// 注意类名必须为 Main, 不要有任何 package xxx 信息
public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String temp = sc.nextLine();
        StringBuilder sb = new StringBuilder(temp).reverse();
        String str = sb.toString();
        StringBuilder res = new StringBuilder();
        for(int i = 0; i < str.length(); i++) {
            char ch = str.charAt(i);
            if(str.indexOf(ch) == i) {//本题精彩之处
                res.append(ch);
            }
        }
        System.out.println(res.toString());
    }
}
```

## HJ10 字符个数统计

方法1：利用HashSet去重

```java
import java.util.*;

// 注意类名必须为 Main, 不要有任何 package xxx 信息
public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        while(sc.hasNext()) {
            String str = sc.nextLine();
            HashSet<Character> set = new HashSet<>();
            //int res = 0;
            for(int i = 0; i < str.length(); i++) {
                char ch = str.charAt(i);
                // if(!set.contains(ch)) {
                //     set.add(ch);
                //     res++;
                // }
                set.add(ch);
            }
            System.out.println(set.size());
        }
    }
}
```

方法2：位图 不要求掌握。

## HJ11 数字颠倒

```java
import java.util.Scanner;

// 注意类名必须为 Main, 不要有任何 package xxx 信息
public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String str = sc.nextLine();
        System.out.println(new StringBuilder(str).reverse().toString());
    }
}
```

## HJ12 字符串反转

```java
import java.util.Scanner;

// 注意类名必须为 Main, 不要有任何 package xxx 信息
public class Main {
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        String str = in.nextLine();
        //System.out.println(new StringBuilder(str).reverse().toString());
        StringBuilder sb = new StringBuilder();
        for(int i = str.length() - 1; i >= 0; i--) {
            sb.append(str.charAt(i));
        }
        System.out.println(sb.toString());
    }
}
```

## HJ13 句子逆序

```java
import java.util.*;

// 注意类名必须为 Main, 不要有任何 package xxx 信息
public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String str = sc.nextLine();
        String[] strArr = str.split("\\s+");
        Stack<String> stack = new Stack<>();
        for (int i = 0; i < strArr.length; i++) {
            stack.push(strArr[i]);
        }
        //stack.pop();
        StringBuilder sb = new StringBuilder();
        while (!stack.isEmpty()) {
            sb.append(stack.pop()).append(" ");
        }
        System.out.println(sb.toString());
    }
}
```

更简单的写法：

```java
import java.util.*;

// 注意类名必须为 Main, 不要有任何 package xxx 信息
public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String str = sc.nextLine();
        String[] strArr = str.split("\\s+");
        for(int i = strArr.length - 1; i >= 0; i--) {
            System.out.print(strArr[i] + " ");
        }
    }
}
```

## HJ23 字符串排序

> 1.nextInt()和next()、nextFloat()、nextDouble()都是只读取有效字符的，不会读取空格键、Tab键和回车键，当它在输入有效字符前碰到这些无效字符时会自动跳过，在输入有效字符后碰到时便结束读取，并把没能读取的字符留在缓冲区。
>
> 2.而nextLine()除了回车啥都能读取，也就是碰到回车时结束读取，但不会把回车留在缓冲区。

```java
import java.util.*;

// 注意类名必须为 Main, 不要有任何 package xxx 信息
public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int len = sc.nextInt();
        String[] arr = new String[len];
        for(int i = 0; i < len; i++) {
            arr[i] = sc.next();
        }
        Arrays.sort(arr);
        for(String str : arr) {
            System.out.println(str);
        }
    }
}
```



## HJ22 汽水瓶

方法1：找规律

```java
import java.util.Scanner;

// 注意类名必须为 Main, 不要有任何 package xxx 信息
public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        while(sc.hasNextInt()) {
            int num = sc.nextInt();
            if(num == 0) {
                break;
            }
            System.out.println(num / 2);
        }
    }
}
```

方法2：模拟

> 讲真不是很难，但是为什么每次都要想很久。

```java
import java.util.Scanner;

// 注意类名必须为 Main, 不要有任何 package xxx 信息
public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        while(sc.hasNextInt()) {
            int num = sc.nextInt();
            if(num == 0) {
                break;
            }else {
                countCola(num);
            }
            
        }
    }
    //计算num个空瓶可以兑换多少瓶汽水
    public static void countCola(int num) {
        if(num == 1) System.out.println(0);
        if(num == 2) System.out.println(1);
        int count = 0;
        while(num / 3 > 0) {
            count = count + num / 3;
            //还剩下多少个瓶盖
            num = num / 3 + num % 3;
            //可以向老板借一个
            if(num == 2) {
                num = num + 1;
            }
        }
        System.out.println(count);
    }
}
```

