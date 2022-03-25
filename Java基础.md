## Java转义符

- /n 换行
- /t 制表符
- \\\\ 一个斜杠
- \\\'  一个 '
- \\\" 一个 “
- \r 回车符

## 字符类型

两个字节，与Unicode码对应

## java输入 

使用扫描器 java.util.Scanner

```java
import java.util.Scanner;
//简单的文本扫描器
public class Input{
	public static void main(String args[]){
		Scanner scanner = new Scanner(System.in);
		//接受用户输入
		System.out.println("请输入用户名字");
		String name = scanner.next();
		int age = scanner.nextInt();
		double salary = scanner.nextDouble();
		System.out.println("名字：" + name + ",薪水：" + salary + ",年龄：" + age);
	}
}
```

## 可变参数

数据类型... 变量

- 代表零个或多个参数，本质是数组
- 可变参数的实参可以是数组
- 可变参数可以和其他参数一起放到形参列表，但只能在形参列表的最后
- 一个形参列表中只能有一个可变参数

```java
public class VarParameter{
	public static void main(String[] args){
		int a=1,b=2,c=3,d=4;
		VarParameter varParameter = new VarParameter();
		System.out.println(varParameter.sum(a,b,c,d));
	}

	int sum(int... nums){
		int res=0;
		for(int i=0;i<nums.length;++i){
			res+=nums[i];
		}
		return res;
	}
}
```



