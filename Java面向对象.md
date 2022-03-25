# Java面向对象笔记

## 构造方法/构造器constructor

- 构造方法名和类名相同
- 完成对对象的初始化，系统自动调用，并不是创建对象，创建对象时系统会自动调用构造方法
- 没有返回值
- 一个类可以定义多个构造器，构造器可以重载
- 若程序员没有定义构造器，系统会自动生成默认的无参构造器（javap可以反编译）
- 一旦自己定义构造器后，系统就不会自动生成无参构造器，需要自己显式声明

## 对象创建流程分析

1. 方法区加载类的信息（只会加载一次）
2. 堆中开空间，默认初始化，然后显式初始化
3. 构造器对对象初始化（String指向常量池中的数据地址）
4. 返回对象在堆中的地址

## this关键字

1. this关键字可以访问本类的属性，方法，构造器
2. this用于区分当前类和局部变量
3. 访问成员方法：this.方法名(参数列表)
4. 访问构造器方法，this(参数列表) 只能在其他构造器中使用，且必须放在第一条语句
5. this不能在类定义的外部使用，只能在类定义的方法中使用

## 访问修饰符

- public 对外公开
- protected 对子类和同一个包中的类公开
- 默认： 没有修饰服号，对同一个包公开
- private 只有类本身可以访问，不对外公开

## 封装encapsulation

把抽象出的数据和对数据的操作封装在一起，数据被保护在内部，程序的其他部分只有通过被授权的操作才能对数据进行操作

封装好处：

1. 隐藏实现的细节
2. 对数据进行验证，保证安全可合理

实现步骤：

1. 将属性进行私有化private 不能直接修改属性
2. 提供公共的public 的set方法，用于对属性判断并赋值
3. 提供公共的get方法，用于获取属性的值

## 继承

1. 子类继承多有的属性和方法，但是私有属性不能再子类中直接访问，要通过公共的方法去访问
2. 子类必须调用父类的构造器，完成父类的初始化
3. 创建子类对象时，默认调用父类的无参构造器，若父类中不存在无参构造器，必须使用super指定父类构造器
4. super使用时必须放在第一行，与this调用构造器不能共存
5. Java所有类都是Object的子类，父类构造器会一直向上追溯到Object
6. Java时单继承机制
7. 继承中子类父类属性相同？

## 继承本质

返回属性时进行查找，首先查找子类的属性，若有则返回信息，若没有，则向上找，直到Object。

## super使用

1. 访问父类的属性，不能访问父类的private属性
2. 访问父类的方法，不能访问父类的私有方法
3. 访问父类的构造器方法，必须放在开头
4. 子类与父类属性方法重名时用可用super区分

## 方法重写 Override

子类有一个方法，和父类的方法名称，返回类型，参数都一样，就说子类覆盖了父类的方法

1. 子类与父类中的方法名与参数列表完全一样
2. 子类方法的返回类型与父类的一样，或者是父类返回类型的子类
3. 子类方法不能缩小父类方法的访问权限

## 多态polymorphic

### 方法多态：

重写与重载体现多态

### 对象多态（多态的核心）

1. 一个对象的编译类型和运行类型可以不一致
2. 编译类型在定义对象时就确定了，不能改变
3. 运行类型时可以变化的
4. 编译类型看定义时 = 号的左边，运行类型看 = 号的右边

```java
Animal animal = new Dog(); //编译类型时Animal，运行类型时Dog
animal = new Cat(); //运行类型编程了Cat。编译类型仍然时Animal
```

多态前提：两个对象存在继承关系

#### 多态向上转型：

1. 本质：父类的引用指向了子类的对象
2. 特点：编译类型看左边，运行类型看右边，可以调用父类中所有成员（在访问权限下），不能调用子类中的特有成员（编译器无法识别），最终运行效果看子类中的具体实现

#### 多态向下转型

1. 语法：子类类型  引用名  =  （子类类型）父类引用
2. 只能强制转换父类的引用，不能强转父类的对象
3. 要求父类的引用必须指向当前目标类型的对象
4. 当向下转型后，可以调用子类类型中所有的成员

```java
Animal animal = new Cat();
animal.cry();
animal.eat();
Cat cat = (Cat) animal;
cat.catchMouth();
Dog dog = (Dog) animal;//运行时产生异常
```

#### 注意事项

1. 属性没有重写之说，属性直接看编译类型
2. instanceOf 比较操作符 判断对象的类型是否为某类型或某类型的子类（判断运行类型）

## Java动态绑定机制（非常重要）

1. 当调用对象方法的时候，该方法会和该对象的内存地址/运行类型绑定
2. 当调用对象属性时，没有动态绑定机制，哪里声明的，就在哪里使用

## 多态应用

1. 多态数组：数组的定义类型为父类类型，里面保存的实际元素类型为子类类型
2. 多态参数：方法定义的形参类型为父类类型，实参类型允许为子类类型

## Object类详解

### == 与 equals方法

==是比较运算符

1. 既可以判断基本类型，游可以判断引用类型
2. 判断基本类型，判断值是否相等
3. 判断引用类型，是判断地址是否相等，即判断是不是指向同一个对象

equals方法

1. 只能判断引用类型
2. 默认比较对象的地址是否相同，即判断两个对象是不是同一个对象
3. 重写后变成比较两个对象的值是否相等

### hashcode方法

1. 提高具有哈希结构的容器效率
2. 两个引用，指向同一个对象，哈希值一定是一样的
3. 两个引用如果指向不同对象，哈希值大概率是不一样的
4. 哈希值主要根据地址号来的，不能完全将哈希值等价与地址

### toString方法

将对象打印输出时会默认调用toString方法

### finalize方法

垃圾回收器确定不存在该对象的引用时，系统自动运行，释放资源

## 类变量和类方法

使用static关键字修饰

访问修饰符 static 数据类型 变量名；

1. 当需要多个对象共享一个变量时
2. 类变量是所有对象共享，实例变量是每个对象独享的
3. 类加载后类变量和类方法就可以使用
4. 开发工具类时可以把方法做成静态的
5. 类方法中无this参数，普通方法隐藏this
6. 类方法只能访问静态成员
7. 普通方法可以访问普通成员和静态成员

## main方法

1. main方法是虚拟机调用，必须是public
2. 调用main方法不需要创建对象，故设置为static
3. 参数String数组是执行时传入的，如 java Hello 参数一 参数二。。。

## 代码块

[修饰符]{代码};

1. 修饰符可选，只能选static
2. 代码块分为静态代码块，普通代码块
3. 相当于另一种形式的构造器，可以做初始化操作
4. 如果多个构造器中有重复语句可以抽取到代码块中
5. static代码块在类加载时执行，非static在对象创建时执行
6. 如果只是用静态成员，普通代码块不会执行
7. 构造器前隐含了super与普通代码块
8. 优先级，静态代码块>普通代码块>构造器
9. 静态代码块只能调用静态成员
10. 普通代码块可以调用任意成员

## 类什么时候加载（重要）

1. 创建对象实例（new）时
2. 创建子类的对象示例，父类也会加载
3. 使用类的静态成员时（静态属性，静态方法）

## final关键字

### 使用：

1. 不希望类被继承，使用final修饰
2. 不希望父类的某个方法被子类覆盖/重写，使用final关键字
3. 不希望类的某个值被修改，使用final
4. 不希望某个局部变量被修改，使用final修饰

### 注意事项：

1. 定义常量，一般XX_XX命名

2. final定义的属性必须赋初值，而且不能修改，可在一下位置赋初值：

   1. 定义时
   2. 构造器中
   3. 代码块中

3. 如果final修饰的属性时静态的，初始化：

   1. 定义时
   2. 静态代码块中，构造器中不能赋值

4. final类不能继承，可以实例化对象

5. 普通类中有final方法，方法不能被重写，可以被继承

6. final不能修饰构造方法

7. final和static往往搭配使用，效率更高，编译器做了优化处理，不会导致类的加载

   ```java
   class BBB{
       public final static int num = 1000;
       static{
           System.out.println("静态代码块执行")；
       }
   }
   ```

   

8. 包装类（Integer，Double，Float等）都是final，String也是final类

## 抽象类

当父类的某些方法，需要声明，但又不确定如何实现时，可以声明为抽象方法，这个类就是抽象类

一般来说抽象类会被继承，方法由子类实现

```java
abstract class Animal {
    private String name;

    public Animal(String name) {
        this.name = name;
    }
    //父类方法的不确定性
    //将该方法设计为抽象方法abstract
    //抽象方法就是没有实现的方法，没有方法体
    public abstract void eat();
}
```

### 注意事项

1. 抽象类不能被实例化
2. 抽象类不一定包含abstract方法，抽象类可以没有抽象方法
3. 一旦类包含抽象方法，必须声明为抽象类
4. abstract只能修饰类和方法
5. 如果一个类继承了抽象类，他必须实现抽象类中的所有抽象方法，除非自己也声明为abstract

## 接口

inferface{}

给出一些没有实现的方法，抽象方法可以省略abstract，封装到一起，某个类使用的时候，根据具体情况把方法写出来

Jdk7.0前，接口里的所有方法都没有方法体，都是抽象方法

Jdk8.0后，接口可以有静态方法，默认方法，也就是接口中可以有方法的具体实现，需要使用default关键字修饰

```java
public interface InterfaceTest {
    public int n = 100;
    public void hello();
    public default void say(){
        System.out.println("say hi");
    }
    public static void cry(){
        System.out.println("cry...");
    }
}
```

### 注意事项

1. 接口不能被实例化
2. 接口中所有的方法都是public方法，默认就是public方法，接口中的抽象方法可以不用abstract修饰
3. 一个普通类实现接口，就必须将接口的所有方法都实现
4. 抽象类实现接口，可以不用实现接口的方法
5. 一个类可以实现多个接口
6. 接口中的属性只能时final的，而且是public static final修饰符，必须初始化
7. 接口中属性访问形式：接口名.属性名
8. 一个接口不能继承其他的类，但可以继承多个接口
9. 接口的修饰符只能时public和默认

## 内部类（非常重要）

类的五大成员：属性，方法，构造器，代码块，内部类

内部类的最大特点时可以直接访问私有属性

```java
class Outer{ //外部类
    private int n1 = 100;
    public void m1(){
        System.out.println("m1()");
    }
    {
        System.out.println("代码块");
    }
    public Outer(){}

    class Inner{ //内部类

    }
}
```

内部类有四种：

- 定义在外部类的局部位置上（比如方法内，代码块中）
  1. 局部内部类（有类名）
  2. 匿名内部类（重点，没有类名）

- 定义在外部类的成员位置上：
  1. 成员内部类（没用static修饰）
  2. 静态内部类（使用static修饰）

### 局部内部类

1. 局部内部类可以访问外部类是所有成员，包括私有的
2. 不能使用访问修饰符，以为局部变量也不能添加访问修饰符，可以用final修饰
3. 作用域：仅仅在定义它的方法或代码块中
4. 局部内部类可以直接访问外部类的成员
5. 外部类在方法中可以创建局部内部类的对象，然后调用方法即可
6. 外部其他类不能访问局部内部类，应为局部内部类只是一个局部变量
7. 外部类和局部内部类的成员重名时，默认遵循就近原则，如果要访问外部类的成员，可以使用（外部类名.this.成员）进行访问

```java
package com.ljd.innerclass_;

public class LocalInnerClass {
    public static void main(String[] args) {
        Outer02 outer02 = new Outer02();
        outer02.m1();
        System.out.println("outer02:" + outer02);
    }
}
class Outer02{ //外部类
    private int n1 = 100;
    private void m2(){
        System.out.println("m2()");
    }
    public void m1(){
        //局部内部类在外部类的局部，通常在方法内
        class Inner02{//局部内部类
            private int n1 = 1000;
            public void f1(){
                //局部内部类可以直接访问私有属性
                //重名遵循就近原则，访问外部类使用 类名.this.属性
                //Outer02.this本质就是
                System.out.println("外部类n1=" + Outer02.this.n1);
                System.out.println("局部内部类n1=" + n1);
                System.out.println("Outer02.this:" + Outer02.this);
                m2();
            }

        }
        Inner02 inner02 = new Inner02();
        inner02.f1();
    }

}
```

### 匿名内部类（重要！！！）

本质是一个类，是一个内部类，该类没有名字（系统分配名字），匿名内部类同时是一个对象

定义在外部类的局部位置，比如方法中，代码块中

基本语法：

```java
new 类或接口(参数列表){
	类体
}
```

1. 匿名累不累使用一次就不能使用了
2. jdk底层创建时会创建一个类
3. 可以不用声明对象名，直接调用方法，应为匿名内部类本身会产生一个对象
4. 可以访问外部类中的任何成员

#### 例子：

```java
package com.ljd.innerclass_;

public class AnonymousInnerClass {
    public static void main(String[] args) {
        Outer04 outer04 = new Outer04();
        outer04.m1();
    }
}
class Outer04{
    private int n1 = 100;
    private void m2(){

    }
    public void m1(){
        //基于接口的匿名内部类
        //需求：像使用接口A，并创建对象
        //传统方式：写一个类并实现接口，并创建对象
        //希望对象只使用一次，以后再也不适用
        //基于接口的匿名内部类Outer04$1
        IA tiger = new IA(){

            @Override
            public void cry() {
                System.out.println("老虎叫唤");
            }
        };
        tiger.cry();
        System.out.println(tiger);

        //基于类的匿名内部类
        //father编译类型：Father
        //father运行类型：Outer04$2
        //同时也直接放回了匿名内部类的对象
        //参数列表会传递给父类的构造器
        Father father = new Father("jack"){
            @Override
            public void test() {
                System.out.println("匿名内部类重写test");
            }
        };
        System.out.println("father对象的运行类型" + father.getClass());
        father.test();

        //基于抽象类的匿名内部类
        Animal animal = new Animal(){
            @Override
            void eat() {
                System.out.println("匿名内部类吃东西");
            }
        };
        System.out.println("animal运行类型" + animal.getClass());
        animal.eat();
    }
}

interface IA{
    public void cry();
}

class Father{
    private String name;

    public Father(String name) {
        System.out.println("Father构造器:" + name);
        this.name = name;
    }

    public void test(){}
}
abstract class Animal{
    abstract void eat();
}
```

#### 匿名内部类使用场景

把匿名内部类当作实参直接传递

```java
package com.ljd.innerclass_;

public class InnerClassExercises01 {
    public static void main(String[] args) {
        //匿名内部类方法实现
        f1(new IB() {
            @Override
            public void show() {
                System.out.println("神奇的匿名内部类");
            }
        });
    }

    public static void f1(IB ib){
        ib.show();
    }

}

interface IB {
    void show();
}
```

### 成员内部类

1. 在外部类的成员位置
2. 可以加任何访问修饰符，因为类的成员也可以添加任何访问修饰符
3. 作用域与外部类的其他成员一样，为整个类体
4. 成员内部类访问外部类：直接访问
5. 外部类访问成员内部类：创建对象再访问
6. 外部其他类放为成员内部类：
   1. 使用外部类的一个对象去创建内部类的对象
   2. 在外部类中写一个返回内部类对象的方法
7. 内部类与外部类重名是，就近原则，若要访问外部类，使用 （外部类.this.属性）

#### 示例代码：

```java
package com.ljd.innerclass_;

public class MemberInnerClass01 {
    public static void main(String[] args) {
        Outer05 outer05 = new Outer05();
        outer05.t1();
        //使用外部类的对象创建内部类的对象
        Outer05.Inner inner = outer05.new Inner();
        inner.say();
        //外部类中写一个方法，返回内部类的对象示例
        Outer05.Inner innerInstance = outer05.getInnerInstance();
        innerInstance.say();
    }
}

class Outer05{
    private int n1 = 100;
    public String name = "hhd";
    //成员内部类，可以使用任意的访问修饰符，因为本身是成员
    class Inner{
        public void say(){
            //直接访问外部类
            System.out.println("n1 = " + n1 + " name = " + name);
        }
    }

    public void t1(){
        Inner inner = new Inner();
        inner.say();
    }

    public Inner getInnerInstance(){
        return new Inner();
    }

}

```

### 静态内部类

1. 可以直接访问外部类中的所有静态成员，包括私有的，不能直接访问非静态成员
2. 可以添加任意访问修饰符
3. 作用域为整个类体
4. 外部其他类放为内部类：
   1. 通过外部类直接访问内部类，因为为静态成员，可以直接访问（满足访问权限）
   2. 使用外部类的方法返回内部类的对象，可以是静态方法

#### 示例

```java
package com.ljd.innerclass_;

public class StaticInnerClass01 {
    public static void main(String[] args) {
        Outer06 outer06 = new Outer06();
        outer06.m1();

        //外部其他类使用静态内部类
        //静态内部类可以通过类名直接访问（需要满足访问权限）
        Outer06.Inner inner = new Outer06.Inner();
        inner.say();
        //外部类方法返回静态内部类的示例对象
        Outer06.Inner inner1 = outer06.getInner();
        inner1.say();
        Outer06.Inner innerStatic = Outer06.getInnerStatic();
        innerStatic.say();
    }
}

class Outer06{
    private int n1 = 100;
    private static String name = "张三";
    //静态内部类
    //使用static修饰
    static class Inner{
        public void say(){
            System.out.println(name);
        }
    }

    public void m1(){
        Inner inner = new Inner();
        inner.say();
    }

    public Inner getInner(){
        return new Inner();
    }

    public static Inner getInnerStatic(){
        return new Inner();
    }

}
```

## 枚举（enum）

### 自定义自定义类实现枚举

1. 枚举对象不需要提供set方法，因为枚举对象值通常为只读
2. 对枚举对象/属性使用 final + static 共同修饰，实现底层优化
3. 枚举对象名通常全部用大写，遵循常量命名规范
4. 枚举对象根据需要，也可以有多个属性
5. 构造器私有化
6. 内部创建一组对象

#### 栗子

```java
package com.ljd.enum_;

public class Enumeration01 {
    public static void main(String[] args) {
        System.out.println(Season.SPRING);
        System.out.println(Season.SUMMER);
        System.out.println(Season.AUTUMN);
        System.out.println(Season.WINTER);
    }
}
//自定义枚举实现
class Season{
    private String name;
    private String desc;

    public static final Season SPRING = new Season("春天","温暖");
    public static final Season WINTER = new Season("冬天","寒冷");
    public static final Season AUTUMN = new Season("秋天","凉爽");
    public static final Season SUMMER = new Season("夏天","炎热");

    //构造器私有化，防止直接new
    //去掉set相关方法，防止属性被修改
    //在Season内部直接创建固定的对象
    //优化，可以加入 final 修饰符
    private Season(String name, String desc) {
        this.name = name;
        this.desc = desc;
    }
    public String getName() {
        return name;
    }

    public String getDesc() {
        return desc;
    }

    @Override
    public String toString() {
        return "Season{" +
                "name='" + name + '\'' +
                ", desc='" + desc + '\'' +
                '}';
    }
}
```

### enum关键字实现枚举类

1. 使用enum替换class关键字
2. 使用 常量名（实参列表） 的形式定义常量
3. 常量需要写在最前面
4. 多个常量使用 逗号(,) 分割

#### 栗子

```java
package com.ljd.enum_;

public class Enumeration02 {
    public static void main(String[] args) {
        System.out.println(Season02.SPRING);
        System.out.println(Season02.SUMMER);
        System.out.println(Season02.AUTUMN);
        System.out.println(Season02.WINTER);
    }
}
//使用enum关键字实现
enum Season02{
    //使用了枚举关键字
    //使用enum关键字替换class
    //使用 常量名（实参列表）的形式表示常量
    //多个常量使用,间隔
    //使用enum实现枚举，需要将常量写在最前面
    SPRING("春天","温暖"),
    WINTER("冬天","寒冷"),
    AUTUMN("秋天","凉爽"),
    SUMMER("夏天","炎热");
    private String name;
    private String desc;
    private Season02(String name, String desc) {
        this.name = name;
        this.desc = desc;
    }
    public String getName() {
        return name;
    }

    public String getDesc() {
        return desc;
    }

    @Override
    public String toString() {
        return "Season{" +
                "name='" + name + '\'' +
                ", desc='" + desc + '\'' +
                '}';
    }
}
```

#### 注意事项

1. 使用enum关键字开发枚举类时，默认继承Enum类，而且是一个final类，使用javap反编译工具可查看
2. 传统的 public static final Season SPRING = new Season("春天","温暖"); 简化为： SPRING("春天","温暖"); 这里需要与相应的构造器对应
3. 如果使用无参构造器创建枚举对象，实参列表和括号都可以省略
4. 有多个枚举对象使用 ， 分割，最后使用；结尾
5. 枚举对象必须放在枚举类的行首
6. enum声明的类无法继承，因为会隐式继承Enum类，可以实现接口

### Enum类方法

#### name()

返回枚举对象的名字

#### ordinal()

返回枚举对象的次序/编号，从0开始编号

#### values()

返回所有的枚举对象，是一个数组

#### valueOf()

将字符串传唤为枚举对象，要求字符串必须为已有的常量名，否则报异常

#### conpareTo()

比较两个枚举常量，比较的是位置号，比较是二者相减
