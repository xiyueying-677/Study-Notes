## javeSE笔记

## 快捷键

psvm + tap  快捷创建main函数

Ctrl + Alt + T  选择多行代码并用for循环包裹

Ctrl + D 复制这行代码到下一行

Ctrl + Alt + M 将选择代码分离出来作为一个方法

Ctrl + F12 找类的方法

Fn + Alt + F12 快捷创建成员的set，get函数

Ctrl + Alt + V  在创建对象时自动生成左边内容

## 1. 对象

**封装**：对象代表什么，必须封装相应的数据，且提供**对应的行为**

**就近原则**：在方法中，出现重名变量时，谁近用谁
**this关键字**：代表所在方法调用者的地址值
**super关键字**：代表父类的存储空间

**标准javaBean类**：用来描述一类事物的类

- 类名要见名知意
- 成员变量使用private修饰
- 至少提供两个构造方法（无参和全参）
- 提供每一个成员变量的setXxx() 和 getXxx()方法
  （Fn + Alt + F12 快捷创建成员的set，get函数）

**测试类**：检查其他类是否正确，带有main方法，是程序的入口

**工具类**：不用来描述一类事物，而是帮我们做一些事情

- 类名要见名知意
- 构造方法私有化
- 方法定义为静态

## 2. 字符串String

### 2.1 字符串的比较

- 对基本数据类型“\==”比较数值
  对引用数据类型（String）“==”比较地址

- new创建的String在堆内存里，普通的String在串池中，创建时会在串池中找是否已存在字符串（控制台输入的也为new）
- 若想比较相同的字符串，使用s1.equals（s2），返回boolean类型，若忽略大小写，则使用equalsIgnoreCase方法

### 2.2 字符串的方法

- **char charAt(i)**：返回下标为 i 的字符
- **String substing(left, right)**：截取[left，right )字符串
- **String replace(s1,s2)**：将字符串中的 s1 替换成 s2
- **char[]  toCharArray()**：将字符串变成字符数组

### 2.3 StringBuilder类

**append(任意类型)**：在字符串后添加元素

**reverse()**：使字符串反转

**int length()**：获取字符串长度

**String toString()**：转化为字符串类型

​	s.append().append().··· 链式编程

### 2.4 StringJoiner类

构造方法：

**StringJoint(间隔符号)**：之后的拼接均会加上间隔符号

**StringJoint(间隔符号**，开始符号，结束符号)：

成员方法：

**add()**：添加元素

**int length()**：返回所有字符总个数（加上了间隔、开始和结束符号）

**String toString()**：转化为字符串类型

### 2.5 字符串相关的底层原理

2.5.1 **String的拼接**

拼接时有变量，会创建StringBuilder类型，拼接后再回到String类型

```java
String s1 = "abc";
String s2 = "ab";
String s3 = s2 + "c";
String s4 = "ab" + "c";
//结果 s1 != s3; s1 == s4
```

2.5.2 **StringBuilder 的容量**

​	默认16，超出时 容量*2 + 2，扩容后仍超出以实际长度为标准

​	capital() 能获得它的容量

## 3. 集合Arraylist

### 3.1 创建ArrayList的对象

`ArrayList<引用数据类型>  list  =  new  ArrayList<>();`

泛型：<>限定集合中存储数据的类型

基本数据类型对应的包装类：

- char	 对应	Character
- int		对应	Interger
- 其他的均为首字母大写

### 3.2方法

**(boolean)  add(E e)**：添加元素

**(boolean)  remove(E e)**：删除元素

**set(e1, e2)**：修改元素

**get(E e)**：查询元素

**int  size()**：返回集合元素个数

## 4. 面向对象进阶

### 4.1 static

4.1.1 **静态变量**：被static修饰的成员变量

- 特点：
  - 被该类所有对象共享
  - 不属于对象，属于类
  - 随着类的加载而加载，优先于对象存在
- 调用方式：类名调用（推荐）；对象名调用

4.1.2 **静态方法**：被static修饰的成员方法

- 特点：
  - 多用再测试类和工具类中
  - javabean类中很少用
- 调用方式：类名调用（推荐）；对象名调用

4.1.3 **注意事项**

1. 静态方法中只能访问静态方法/变量，访问实例方法要new对象

2. 非静态方法可访问所有方法
2. 静态方法无this关键字
   （实例方法之间之所以可以互相调用是因为有隐藏的this.前缀）
2. 方法调用规则：
   静态：类名 . 方法         ；        实例：对象 . 方法

4.1.4 **重新认识main方法**

- public：被JVM调用，访问权限足够大
- static：被JVM调用，不用创建对象，直接类名访问
                因为main方法是静态的，所以测试类其他方法也是静态的
- void：被JVM调用，不需要给JVM返回值
- main：通用名称，虽然不是关键字，但可被JVM识别
- String[]  args：数组名为args类型为String的数组（已淘汰）

### 4.2 继承

4.2.1 **继承的概述**

- extends用于让一个类与另一个类建立起继承关系
- 子类可以得到并使用父类的属性和行为
- 子类可以在父类继承上新增其他功能

4.2.2 **继承的特点**

- java只支持单继承，不支持多继承，但支持多层继承
- 每一个类度直接或间接的继承于Object类

4.2.3 **子类继承的内容**

- 构造方法无法继承
- 成员变量都能继承
- 成员方法没有private前缀可以继承

### 4.3 方法的重写

**使用场景：** 当父类方法不能满足子类需求，因重写这个方法

**Override**：放在重写方法上，让系统检验子类重写语法是否正确

**注意事项**：

- 方法的参数，名称必须相同
- 子类方法访问权限必须大于等于父类
- 子类的返回值类型必须小于等于父类
- static，private，final，构造方法不能被重写

### 4.4 子类的构造方法

子类构造方法的第一行语句默认是super()，默认先访问父类中的无参构造，再执行自己

主动写super(参数)，可以调用父类的无参构造

this()可以访问自己的构造方法

### 4.5 多态

**4.5.1 表现形式**：父类类型  对象名称  =  子类对象；

**4.5.2 多态的前提**：

- 有继承关系
- 有父类引用指向子类
- **有方法重写**

**4.5.3 多态的特点**：成员变量使用父类的变量，方法使用子类重写方法

**4.5.4 多态的优缺点**：

- 优点：使用父类做方法的参数是，可接收所有子类
- 缺点：无法使用子类特有方法，只能使用重写方法
  （可以在方法外使用子类创建对象，而方法的参数使用父类）

**4.5.5 多态的类型转换**：a  instanceof  B，判断a是否是B类型的对象；用于将多态（基本类型为父类的子类）转化为自己类型的类

```java
//a 为以Animal父类创建的子类对象
//为将a转化为原对象，可使用下面代码将a转化为d
if(a instanceof Dog){
    Dog d = (Dog)a;
    d.eat();
}	
//下面为新的特性
if(a instanceof Dog d){
    d.eat();
}
```

### 4.6 包

包名的规则：域名反写+包的作用

使用其他类的规则：

- 使用同一个包或java.lang包时，不需要导包
- 使用两个包中的同名类时，需要使用全类名

### 4.7 final

final修饰：

- 方法：最终方法，不能被重写
- 类：最终类，不能被继承
- 基本数据类型：常量，必须被赋值
  （常量：不可变内容，全部大写，单词间用__分隔）
- 引用数据类型：地址不变，内容可变

### 4.8 权限修饰符，代码块

**private**：只能在同类

**（default）**：在本包中

**protected**：在本包和其他包的子类中

**public**：所有地方都可用

**构造代码类**：在成员变量里写  {  代码  } ，在构造方法前执行

**静态代码类**：相似于构造代码块，但是只执行一次

### 4.9 抽象类，接口

**4.9.1 抽象类与抽象方法的特点**

- 抽象类用关键字abstract来定义
- 抽象类不一定有抽象方法，但包含抽象方法的类一定是抽象类
- 抽象类不能实例化，但可以有构造方法
- 子类必须重写抽象方法 （ 子类是抽象类不用）

**4.9.2 接口的定义和使用**：

- 接口用关键字interface来定义
- 接口不能实例化
- 接口和类之间是实现关系，用implement
- 接口的子类必须重写抽象方法 / 子类是抽象类

**4.9.3 接口中成员的特点**：

- 成员变量：只能是常量；默认修饰符：public （static final）
- 无构造方法
- 成员变量只能是抽象方法；默认修饰符：（public abstract）

**4.9.4 接口新增的方法：**

**默认方法：**

格式：public default 为前缀

注意事项：

- 默认方法不强制被重写。如果重写，应去掉 default 关键字
- 如果实现了多个接口，且存在相同的默认方法，则实现类必须重写默认方法

**静态方法：**

格式：public static 为前缀

注意事项：静态方法只能通过接口名调用，不能通过实现类名和对象名调用

**私有方法：**

格式：private 和 private static  为前缀

作用：解决默认和静态方法中重复使用代码合为一个方法

### 4.10 内部类

**定义**：在类的里面定义一个内部类

**使用场景**：

- 内部类表示的事物是外部类的一部分
- 内部类单独出现没有任何意义

**访问特点**：内部类可以访问外部类的任何方法；外部类要创建对象才能访问内部类

#### 4.10.1 成员内部类

- 写在成员位置的，属于外部类的成员

- 在成员内部类里面，JDK16之前不能定义静态变量，JDK16开始才可以定义静态变量。

**获取成员内部类对象的两种方式**：

方式一：外部直接创建成员内部类的对象

```java
外部类.内部类 变量 = new 外部类（）.new 内部类（）;
//Outer.Inner oi = new Outer().new Inner();
```

方式二：在外部类中定义一个方法提供内部类的对象

```Java
public class Outer {
    private class Inner{
        static int a = 10;
    }
    public Inner getInstance(){
        return new Inner();
    }
}

public class Test {
    public static void main(String[] args) {
        Outer o = new Outer();
        Object inner = o.getIstance();
        //Outer.Inner inner2 = o.getIstance();// Inner 私有时不可用
    }
}
```

**成员内部类获取成员外部类变量的方法**

```java
class Outer {	// 外部类
    private int a = 30;
    // 在成员位置定义一个类
    class inner {
        private int a = 20;

        public void method() {
            int a = 10;
            System.out.println(???);	// 10   答案：a
            System.out.println(???);	// 20	答案：this.a
            System.out.println(???);	// 30	答案：Outer.this.a
        }
    }
}
```


#### 4.10.2 静态内部类

静态内部类只能访问外部类的静态方法和静态变量，访问非静态需要创建对象

**静态内部类对象的创建格式**：

```java
外部类.内部类  变量 = new  外部类.内部类构造器;
```

**调用方法的格式：**

* 调用非静态方法的格式：先创建对象，用对象调用
* 调用静态方法的格式：外部类名.内部类名.方法名();

#### 4.10.3 局部内部类

定义在方法里面的类

不能用public，protected 修饰局部内部类

#### 4.10.4 匿名内部类

内部类的简化写法。他是一个隐藏了名字的内部类

```
new 类名或者接口名() {
     重写方法;
};
举例：
new Inter(){
	public void show(){
	}
};
//整体为匿名内部类的对象
//{}的内容才为匿名内部类
//{}与前面类名/接口名 为继承关系，实现了方法重写
```

## 5.设计拼图游戏

### 5.1 JFrame

```java
//导包
import javax.swing.*;

//1.召唤主界面
JFrame jFrame = new JFrame();

//2.设置主界面的大小 
jFrame.setSize(514,595);

//3.让主界面显示出来
jFrame.setVisible(true);

//4.设置界面标题
jFrame.setTitle("拼图单价版 v1.0");

//5.设置界面置顶
jFrame.setAlwaysOnTop(true);

//6.设置界面到屏幕的正中央
jFrame.setLocationRelativeTo(null);

//7.设置关闭模式
jFrame.setDefaultCloseOperation(3);

//8.取消默认的居中放置
jFrame.setLayout(null);
```

设置关闭模式

一共有四个模式

- 0 ：不关闭
- 1：默认
- 2：关闭所有界面才关闭虚拟机（所有界面均设置为2）
- 3：关闭一个界面就退出虚拟机

### 5.2 JMenuBar

```java
private void initJMenuBar() {
        //创建菜单对象
        JMenuBar jMenuBar = new JMenuBar();

        //创建菜单上面的选项对象
        JMenu functionJMenu = new JMenu("功能");
        JMenu aboutJMenu = new JMenu("关于我们");

        //创建菜单选项的条目对象
        JMenuItem replayItem = new JMenuItem("重新游戏");
        JMenuItem reLoginItem = new JMenuItem("重新登录");
        JMenuItem closeItem = new JMenuItem("关闭游戏");

        JMenuItem accountItem = new JMenuItem("公众号");

        //组合在一起
        functionJMenu.add(replayItem);
        functionJMenu.add(reLoginItem);
        functionJMenu.add(closeItem);

        aboutJMenu.add(accountItem);

        jMenuBar.add(functionJMenu);
        jMenuBar.add(aboutJMenu);

        //给整个界面设置菜单
        this.setJMenuBar(jMenuBar);
    }
```

### 5.3 添加图片

```java
//创建一个图片ImageIcon的对象
        ImageIcon icon = new ImageIcon("image/animal/animal3/3.jpg");
        //创建一个JLabel的对象（管理容器）
        JLabel jLabel = new JLabel(icon);
        //指定位置
        jLabel.setBounds(0,0,105,105);
        //在界面添加管理容器
        this.getContentPane().add(jLabel);

		//清空已有图片
        this.getContentPane().removeAll();
        //刷新界面（一定要刷新，要不然加载不出来）
        this.getContentPane().repaint();
```

先添加的图片在背景的上方

给图片添加边框

`jLabel.setBorder(new BevelBorder(0);`

- 0：图片凸，边框凹
- 1：图片凹，边框凸

### 5.4 事件

* 事件源： 按钮 图片 窗体... 
* 事件：对事件源进行的操作
* 绑定监听：当事件源上发生了某个事件，则执行某段代码 

**常见的事件监听**

* 键盘监听 KeyListener
  - keyPressed：按下不松时调用（反复执行）
  - keyReleased：松开时调用
  - keyTyped：键入键时调用（很少用了）
  - int code = e.getKeyCode()； 键盘按下按键的编号
* 鼠标监听 MouseListener
  - mouseClicked ：单击（按下并释放，在松开按钮后）
  - mousePressed：按下不松
  - mouseReleased：松开按钮
  - mouseEntered：划入动作
  - mouseExited：划出按钮
* 动作监听 ActionListener：监听鼠标的点击 和 键盘的空格
  * `Object source = e.getSource();` 获取当前被点击的条目对象

**添加事件的方法**

**1、匿名内部类**

```java
        //创建一个按钮对象
        JButton jtb = new JButton("点我啊");
        //设置位置和宽高
        jtb.setBounds(0,0,100,50);
        //给按钮添加动作监听
        jtb.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                System.out.println("按钮被点击了");
            }
        });
        //将按钮添加至界面
        jFrame.getContentPane().add(jtb);
```

**2、实现类接口**

```java
public class MyActionListener implements ActionListener {
    @Override
    public void actionPerformed(ActionEvent e) {
        System.out.println("按钮被点击了");
    }
}
pvsm
         //创建一个按钮对象
        JButton jtb = new JButton("点我啊");
        //设置位置和宽高
        jtb.setBounds(0,0,100,50);
        //给按钮添加动作监听
        jtb.addActionListener(new MyActionListener());
        //把按钮添加到界面当中
        jFrame.getContentPane().add(jtb);

```

**3、本类实现接口**

```java
public class MyJFrame extends JFrame implements ActionListener {
    
    //创建按钮对象
    JButton jtb1 = new JButton("点我啊");
    JButton jtb2 = new JButton("点我啊");
    
		public class MyJFrame(){
	    //给按钮设置位置和宽高
        jtb1.setBounds(0,0,100,50);
        jtb1.addActionListener(this);
        //给按钮设置位置和宽高
        jtb2.setBounds(100,0,100,50);
        jtb2.addActionListener(this);
        //那按钮添加到整个界面当中
        this.getContentPane().add(jtb1);
        this.getContentPane().add(jtb2);
        }
    
	@Override
    public void actionPerformed(ActionEvent e) {
        //对当前的按钮进行判断

        //获取当前被操作的那个按钮对象
        Object source = e.getSource();

        if(source == jtb1){
            jtb1.setSize(200,200);
        }else if(source == jtb2){
            Random r = new Random();
            jtb2.setLocation(r.nextInt(500),r.nextInt(500));
        }
    }

}
```

### 5.5 添加弹窗

```java
            //创建弹窗对象
            JDialog jDialog = new JDialog();
            //创建管理图片的容器对象
            JLabel jLabel = new JLabel(new ImageIcon("image/about.png"));
            //把图片添加至弹框
            jDialog.getContentPane().add(jLabel);
            //给弹窗设置大小
            jDialog.setSize(344,344);
            //让弹窗顶置
            jDialog.setAlwaysOnTop(true);
            //让弹窗居中
            jDialog.setLocationRelativeTo(null);
            //让弹窗不关闭无法操作下面的界面
            jDialog.setModal(true);
			//取消界面默认的居中放置
        	jDialog.setLayout(null);
            //设置弹窗为可见
            jDialog.setVisible(true);
```

### 5.6 JTextField

输入框：JTextField（明文显示的输入框）

```java
//设置位置和宽高
setBounds(x,y,宽,高);
//返回输入框中用户输入的数据
//细节：如果用户没有输入，返回的是一个长度为0的字符串
getText();
//修改数据
setText(要修改的内容);
```

输入框：JPasswordField（密文显示的输入框） 

按钮：JButton ：可用setIcon设置背景

```
//去除按钮的默认边框
loginButton.setBorderPainted(false);
//去除按钮的默认背景
loginButton.setContentAreaFilled(false);
//设置背景图片
button.setIcon(new ImageIcon());
```

## 6.常用API

### 6.1 Math

帮助我们进行数学计算的类

```java
public static int abs(int a)					// 绝对值
public static double ceil(double a)				// 向上取整
public static double floor(double a)			// 向下取整
public static int round(float a)				// 按照四舍五入取整
public static int max(int a,int b)				// 最大值
public static int min(int a,int b)				// 最小值
public static double pow (double a,double b)	// 计算a的b次幂的值
public static double sqrt(double a)				// 计算平方根
public static double cbrt( a)				// 计算立方根
public static double random()					// 返回一个[0.0,1.0)的随机值
```

常量 PI：派

### 6.2 System

时间原点：1970年1月1日0点 （c语言的生日）

我们国家时间原点为 8 点

```java
public static long currentTimeMillis()			// 获取当前时间所对应的毫秒值
public static void exit(int status)				
// 终止当前正在运行的Java虚拟机，0表示正常退出，非零表示异常退出
public static native void arraycopy(Object arr1,  int  pos1, Object arr2, int pos2, int length); //将arr1从pos1开始，长度为length，拷贝到arr2的pos2上
```

**arraycopy方法底层细节：**

1.基本数据类型，那么两者的类型必须保持一致，否则会报错

2.长度不能超出范围

3.引用数据类型，那么子类类型可以赋值给父类类型

### 6.3 Runtime

Runtime 不能自己 new创建，要使用 getRuntime（），单例模式

```java
public static Runtime getRuntime()		//当前系统的运行环境对象
public void exit(int status)			//停止虚拟机，同System.exit
public int availableProcessors()		//获得CPU的线程数
public long maxMemory()				    //JVM能从系统中获取总内存大小（单位byte）
public long totalMemory()				//JVM已经从系统中获取总内存大小（单位byte）
public long freeMemory()				//JVM剩余内存大小（单位byte）
public Process exec(String command) 	//运行cmd命令
```

```java
        //7.运行cmd命令
        //shutdown :关机
        //加上参数才能执行
        //-s :默认在1分钟之后关机
        //-s -t 指定时间 : 指定关机时间
        //-a :取消关机操作
        //-r: 关机并重启
Runtime.getRuntime().exec("shutdown -s -t 3600");
```

### 6.4 Object和Objects

#### 6.4.1 Object

```java
public String toString()	//返回该对象的字符串表示形式(可以看做是对象的内存地址值)
    //java.lang.Object@776ec8df  包名加类名@地址值
    //可以对对象的类重写toSrtring方法
public boolean equals(Object obj)	
    //比较两个对象地址值是否相等
    //可以对对象的类重写equals方法比较属性值
protected Object clone()    		//对象克隆
```

```java
//直接打印对象与打印使用了toString结果一样
//System.out.println
//System：类名
//out：静态变量
//System.out：获取打印的对象
//println（）：方法
//参数：表示打印的内容
```

String 重写了equals 方法，比较字符串内容而不是地址

**浅克隆：**

- 不管对象内部的属性是基本数据类型还是引用数据类型，都完全拷贝过来 

- 基本数据类型拷贝过来的是具体的数据，引用数据类型拷贝过来的是地址值。

- Object类默认的是浅克隆，调用原clone必须要强制转换（返回Object类）

>  任何类想使用 Object 克隆必须实现 Cloneable 接口
>
>  没有抽象方法的接口是标记性接口
>
>  如果实现此接口，表示有对应功能，没有接口则不能使用对应功能
>
>  （可以自己写一个单独的clone方法，但不推荐，应该实现接口并重写方法）

**深克隆：**

​	基本数据类型拷贝过来，字符串复用，引用数据类型会重新创建新的

重写克隆为深克隆：先使用浅克隆，再创建新引用类型，拷贝数据，再将地址覆盖浅克隆里的对应地址

```
protected Object clone() throws CloneNotSupportedException {

        //先把被克隆对象中的数组获取出来
        int[] data = this.data;
        //创建新的数组，并拷贝数据
        int[] newData =new int[data.length];
        for (int i = 0; i < data.length; i++) {
            newData[i] = data[i];
        }
        //调用父类中的方法克隆对象
            User u=(User)super.clone();
        //替换克隆出来对象中的数组地址值
        u.data =newData;
        return u;
```

#### 6.4.2 Objects

```java
public static String toString(Object o) 			// 获取对象的字符串表现形式
public static boolean equals(Object a, Object b)	
    // 先进行非空判断，再比较是否相等（使用重写的Object equals方法比较）
public static boolean isNull(Object obj)			// 判断对象是否为null
public static boolean nonNull(Object obj)			// 判断对象是否不为null
```

null 空指针不能调用非静态方法

因此不能使用Object的非静态equals，而使用工具类 Objects 的equals方法

### 6.5 BigInteger

字节占用个数：byte：1 ；short：2；int：4；long：8

int类型的取值范围：-2147483648 ~ 2147483647

当int，long都存不下，使用BigInteger

<font color="red" size="3">**构造方法**</font>

```java
public BigInteger(int num, Random r) 		//获取随机大整数，范围：[0，2的num次方-1]
public BigInteger(String val) 				//获取指定的大整数（必须是整数）
public BigInteger(String val, int radix) 	
    //获取对应进制的大整数，val必须要与进制相符
    
下面这个不是构造，而是一个静态方法获取BigInteger对象
public static BigInteger valueOf(long val) 	
    //静态方法获取BigInteger的对象
    //1.不能超出long类型取值范围
    //2.在内部对常用数字 -16 ~ 16 进行了优化。
    //	提前对常用数字创建了BigInteger对象，多次调用不会创建新对象
```

对象里存储的值不能修改，只能创建新的对象存储运算后的值

<font color="red" size="3">**常见成员方法**</font>

BigDecimal类中使用最多的还是提供的进行四则运算的方法，如下：

```java
public BigInteger add(BigInteger val)					//加法
public BigInteger subtract(BigInteger val)				//减法
public BigInteger multiply(BigInteger val)				//乘法
public BigInteger divide(BigInteger val)				//除法
public BigInteger[] divideAndRemainder(BigInteger val)	 //除法，获取商和余数
public  boolean equals(Object x) 					    //比较是否相同
public  BigInteger pow(int exponent) 					//次幂、次方
public  BigInteger max/min(BigInteger val) 				//返回较大值/较小值
public  int intValue(BigInteger val) 					//转为int类型整数，超出范围数据有误
```

**存储**：创建一个 int 数组，第一位 ，正数 1，负数 -1，零 0，后面每32个字符存一个int元素

数组的长度为21亿个，范围为 pow（42亿，21亿）

### 6.6 BigDecimal

```
System.out.println(0.09 + 0.01);
//结果为 0.09999999999999999
//十进制 转 二进制计算 再转 十进制输出 会导致精度丢失
```

BigDecimal类可以进行更加精准的数据计算。

<font color="red" size="3">**构造方法**</font>

```Java
public BigDecimal(double val)	//这种方法可能会导致不精确，不建议使用
public BigDecimal(String val)

public static BigDecimal valueOf(int/double val) 	
    //静态方法获取对象，存储了 0 ~ 10 的整数对象
```

<font color="red" size="3">**常见成员方法**</font>

BigDecimal类中使用最多的还是提供的进行四则运算的方法，如下：

```java
public BigDecimal add(BigDecimal value)				// 加法运算
public BigDecimal subtract(BigDecimal value)		// 减法运算
public BigDecimal multiply(BigDecimal value)		// 乘法运算
public BigDecimal divide(BigDecimal value)			// 除法运算
public BigDecimal divide(BigDecimal value,精确位数，舍入模式)
    //舍入模式有单独一个类 RoundingMode，直接使用 类名.常量名
    //UP 		远离0舍入
    //DOWN 		靠近0舍入
    //CEILING	向正方向舍入
    //FLOOR		向负方向舍入
    //HALF_UP	四舍五入
    //HALF_DOWN	五舍六入
```

**存储**：创建一个 byte 类型数组，存各个字符的Ascii码值，长度最大为21亿

### 6.7 正则表达式

#### 6.7.1 字符类

1. \[abc\]：代表a或者b，或者c字符中的一个。
2. \[^abc\]：代表除a,b,c以外的任何字符。
3. [a-z]：代表a-z的所有小写字符中的一个。
4. [A-Z]：代表A-Z的所有大写字符中的一个。
5. [0-9]：代表0-9之间的某一个数字字符。
6. [a-zA-Z0-9]：代表a-z或者A-Z或者0-9之间的任意一个字符。
7. [a-dm-p]：a 到 d 或 m 到 p之间的任意一个字符。 
8. [a-z&&\[^bc]]：表示不包括bc的a-z 同 [ad-z]

**举例**

```java
        System.out.println("zz".matches("[^abc]")); //false
        System.out.println("zz".matches("[^abc][^abc]")); //true
        System.out.println("a".matches("[a-z&&[^m-p]]")); //true
        System.out.println("m".matches("[a-z&&[^m-p]]")); //false
```

#### 6.7.2 预定义字符

1. "." ： 匹配任何字符。
2. "\d"：任何数字[0-9]的简写；
3. "\D"：任何非数字\[^0-9\]的简写；
4. "\s"： 空白字符：[ \t\n\x0B\f\r] 的简写
5. "\S"： 非空白字符：\[^\s\] 的简写
6. "\w"：单词字符：[a-zA-Z_0-9]的简写
7. "\W"：非单词字符：\[^\w\]
8. （?i)：忽略后面字母的大小写
   - 注意在匹配时要用两个 \ ，即 \\\ 前一个 \ 表示转义字符

**举例**

```java
        //.表示任意一个字符
        System.out.println("你".matches(".")); //true
        System.out.println("你".matches("..")); //false
        System.out.println("你a".matches(".."));//true

        System.out.println("333".matches("\\d")); // false
        System.out.println("333".matches("\\d\\d\\d")); // true
 
        // 必须是数字 字母 下划线 至少 6位
        System.out.println("2442fsfsf".matches("\\w{6,}"));//true
        System.out.println("244f".matches("\\w{6,}"));//false

        // 必须是数字和字符 必须是4位
        System.out.println("23dF".matches("[a-zA-Z0-9]{4}"));//true
        System.out.println("23 F".matches("[a-zA-Z0-9]{4}"));//false
     

```

#### 6.7.3 数量词

1. X? : 0次或1次
2. X* : 任意次数
3. X+ : 至少1次
4. X{n} : 恰好n次
5. X{n,} : 至少n次
6. X{n,m}: n到m次

### 6.8 爬取

#### 6.8.1 普通爬取

```java
        //Pattern:表示正则表达式
        //Matcher: 文本匹配器，作用按照正则表达式的规则去读取字符串，从头开始读取。
        //          在大串中去找符合匹配规则的子串。
 		String str = "Java自从95年问世以来，经历了很多版本，目前企业中用的最多的是Java8和		Java11，"
        //获取正则表达式的对象
        Pattern p = Pattern.compile("Java\\d{0,2}");
        //获取文本匹配器的对象
		Matcher m = p.matcher(str);
        //m:文本匹配器的对象
        //str:要爬取的内容
        //p:规则
        //m要在str中找符合p规则的小串
        
		boolean b = m.find();
        //拿着文本匹配器从头开始读取，寻找是否有满足规则的子串
        //如果没有，方法返回false
        //如果有，返回true。在底层记录子串的起始索引,结束索引+1：0,4

        String s1 = m.group();
        System.out.println(s1);//Java
        //方法底层会根据find方法记录的索引进行字符串的截取
        // substring(起始索引，结束索引);左闭右开[0,4),读取0~3
        // 会把截取的小串进行返回。

        //第二次在调用find的时候，会继续读取后面的内容
      	b = m.find();
        //第二次调用group方法的时候，会根据find方法记录的索引再次截取子串
        String s2 = m.group();
        System.out.println(s2);//Java8
  
		//循环查找
         while (m.find()) {
            String s = m.group();
            System.out.println(s);
        }

 
```

#### 6.8.2 [非]贪婪爬取

```java
只写+和表示贪婪匹配，如果在+和后面加问号表示非贪婪爬取
+? 非贪婪匹配
*? 非贪婪匹配
贪婪爬取:在爬取数据的时候尽可能的多获取数据
非贪婪爬取:在爬取数据的时候尽可能的少获取数据

举例：
爬取abbbbbbbbbbbb
贪婪爬取获取结果:abbbbbbbbbbbb	//ab+
非贪婪爬取获取结果:ab			//ab+?
    //带+？与不带的区别
    //abbbcccabb
    //"b+?",获取两次b
    //"b"，获取所有b
    //仅对单个字符有区别
```

#### 6.8.3 字符串的正则方法

```java
public boolean matches(String regex)
//判断字符串是否满足正则表达式
public String[] split(String regex)
//按regex正则表达式作为"分隔符"来切割字符串。
public String replaceAll(String regex,String newStr)
//将匹配regex正则表达式的字符串替换为newStr。
```

#### 6.8.4 [非]捕获分组

分组：只看左括号，按照左括号的顺序，从左往右，依次为第一组，第二组，第三组等等

**捕获分组**

```java
//需求1:判断一个字符串的开始字符和结束字符是否一致?只考虑一个字符
//举例: a123a b456b 17891 &abc& a123b(false)
// \\组号:表示把第X组的内容再出来用一次
String regex1 = "(.).+\\1";

//需求2:判断一个字符串的开始部分和结束部分是否一致?可以有多个字符
//举例: abc123abc b456b 123789123 &!@abc&!@ abc123abd(false)
String regex2 = "(.+).+\\1";

//需求3:判断一个字符串的开始部分和结束部分是否一致?开始部分内部每个字符也需要一致
//举例: aaa123aaa bbb456bbb 111789111 &&abc&&
String regex3 = "((.)\\2*).+\\1";
//(.):把首字母看做一组
// \\2:把首字母拿出来再次使用，
// *:作用于\\2,表示后面重复的内容出现任意次数
//注意(.)为第二组，因为他在第二个左括号后。而((.)\\2*)为整个第一组
```

正则表达式内：\\\组号

正则外部使用：$组号

```java
String str = "我要学学编编编编程程程程程程";
//需求:把重复的内容 替换为 单个的

String result = str.replaceAll("(.)\\1+", "$1");
System.out.println(result);
```

**非捕获分组**

- ？可理解为前面内容
- (? = 正则)：获取匹配后面数据的前面内容
- (? !  正则)：不匹配后面数据的前面内容
- (? :  正则)：获取所有内容，区分是否是捕获分组，更好使用组号
  - 非捕获分组：不占用组号
- ( (?i) 正则)：忽略大小写。不是分组

```java
        String s = "Java自从95年问世以来，经历了很多版本，目前企业中用的最多的是Java8和Java11，" +
            "因为这两个是长期支持版本，下一个长期支持版本是Java17，相信在未来不久Java17也会逐渐登上历史舞台";
		//需求1:爬取版本号为8，11.17的Java文本，但是只要Java，不显示版本号。
        String regex1 = "((?i)Java)(?=8|11|17)";

        //需求2:爬取版本号为8，11，17的Java文本。结果为:Java8 Java11 Java17 Java17
        String regex2 = "((?i)Java)(8|11|17)";	//存在组号1
        String regex3 = "((?i)Java)(?:8|11|17)";//不存在组号

        //需求3:爬取除了版本号为8，11.17的Java文本，
        String regex4 = "((?i)Java)(?!8|11|17)";
```

### 6.9 包装类

包装类：基本数据类型对应的引用数据类型

| 基本类型 | 对应的包装类（位于java.lang包中） |
| -------- | --------------------------------- |
| byte     | Byte                              |
| short    | Short                             |
| **int**  | **Integer**                       |
| long     | Long                              |
| float    | Float                             |
| double   | Double                            |
| **char** | **Character**                     |
| boolean  | Boolean                           |

#### 6.9.2 装箱与拆箱

基本类型与对应的包装类对象之间，来回转换的过程称为”装箱“与”拆箱“：

- **装箱**：从基本类型转换为对应的包装类对象。
- **拆箱**：从包装类对象转换为对应的基本类型。

用Integer与 int为例：

基本数值---->包装对象

```java
Integer i = new Integer(4);//使用构造函数函数
Integer iii = Integer.valueOf(4);//使用包装类中的valueOf方法
```

包装对象---->基本数值

```java
int num = i.intValue();
```

#### 6.9.3 自动装箱与自动拆箱

由于我们经常要做基本类型与包装类之间的转换，从Java 5（JDK 1.5）开始，基本类型与包装类的装箱、拆箱动作可以自动完成。例如：

```java
Integer i = 4;//自动装箱。相当于Integer i = Integer.valueOf(4);
i = i + 5;//等号右边：将i对象转成基本数值(自动拆箱) i.intValue() + 5;
//加法运算完成后，再次装箱，把基本数值转成对象。
```

#### 6.9.4 Integer类

| 方法名                                     | 说明                                |
| ------------------------------------------ | ----------------------------------- |
| public static Integer valueOf(int i)    | 返回表示指定的 int 值的 Integer   实例 |
| public static Integer valueOf(String s) | 返回保存指定String值的 Integer 对象    |
| public static string toBinaryString(int i) | 得到二进制                          |
| public static string toOctalString(int i)  | 得到八进制                          |
| public static string toHexString(int i)    | 得到十六进制                        |
| public static int parseInt(string s)       | 将字符串类型的整数转成int类型的整数 |

- 在八种包装类中，除了Character都有对应的 parseXxx 方法

## 7 时间类

### 7.1 Date

世界标准时间（UTC）：格林尼治时间（GMT），目前已替换成原子钟

中国标准时间：UTC + 8小时

时间原点：1970年1月1日0点 （c语言的生日）

```java
public Date() {
    this(System.currentTimeMillis());
}	//将时间原点到现在的毫秒值,转换成Date对象，表示的是当前日期时间
public Date(long date) {
    fastTime = date;
}	//表示的是时间原点度过date毫秒的日期时间

public long getTime()` 			//获取日期时间对应的时间毫秒值。
public void setTime(long time)` //把毫秒值设置给日期对象
```

### 7.2 SimpleDateFormat

作用：格式化Date时间；将字符串时间转换为Date时间

**构造方法**

`public SimpleDateFormat()`：使用默认模式构造对象

`public SimpleDateFormat(String pattern)`：使用指定模式构造对象

String pattern 里面存储的是时间格式

**常用方法**

- `public String format(Date date)`：将Date对象格式化为字符串。
- `public Date parse(String source)`：将字符串解析为Date对象。

**常用的格式规则**

| 标识字母（区分大小写） | 含义 |
| ---------------------- | ---- |
| y                      | 年   |
| M                      | 月   |
| d                      | 日   |
| H                      | 时   |
| m                      | 分   |
| s                      | 秒   |

**parse**

```java
        //1.定义一个字符串表示时间
        String str = "2023-11-11 11:11:11";
        //2.利用空参构造创建simpleDateFormat对象
        //创建对象的格式要跟字符串的格式完全一致
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        Date date = sdf.parse(str);
        //3.打印结果
        System.out.println(date.getTime());//1699672271000
```

**format**

```java
        Date d1 = new Date(0L);	

		//1.利用空参构造创建simpleDateFormat对象，默认格式
        SimpleDateFormat sdf1 = new SimpleDateFormat();
        String str1 = sdf1.format(d1);
        System.out.println(str1);//	1970/1/1 上午8:00

        //2.利用带参构造创建simpleDateFormat对象，指定格式
        SimpleDateFormat sdf2 = new SimpleDateFormat("yyyy年MM月dd日HH:mm:ss");
        String str2 = sdf2.format(d1);
        System.out.println(str2);//	1970年01月01日 08:00:00
```

### 7.3 Calendar

作用：对日期进行运算

它是一个抽象类，不能创建对象，我们可以使用它的子类：java.util.GregorianCalendar类。

有两种方式可以获取GregorianCalendar对象：

- 直接创建GregorianCalendar对象；
- 通过Calendar的静态方法getInstance()方法获取GregorianCalendar对象
  - `public static Calendar getInstance()  `：根据**地区时间**获取GregorianCalendar对象。

**常用方法**

| 方法名                                   | 说明                        |
| ---------------------------------------- | --------------------------- |
| public final Date getTime()              | 获取日期对象                |
| public final setTime(Date date)          | 设置日期对象                |
| public long getTimeInMillis()            | 获取时间毫秒值              |
| public void setTimeInMillis(long millis) | 设置时间毫秒值              |
| public int get(int field)                | 获取某个字段信息            |
| public void set(int field,int value)     | 设置某个字段的值            |
| public void add(int field,int amount)    | 为某个字段增加/减少指定的值 |

field参数表示获取哪个字段的值

可以使用Calender中定义的常量来表示：

Calendar.YEAR : 年
Calendar.MONTH ：月

- Calendar的月份值是0-11，通常 get 要加1, set 要减1

Calendar.DAY_OF_MONTH：月中的日期
Calendar.HOUR：小时
Calendar.MINUTE：分钟
Calendar.SECOND：秒
Calendar.DAY_OF_WEEK：星期

- 返回值范围：1--7，分别表示："星期日","星期一",...,"星期六"

  ```java
      //查表法，查询星期几
      public static String getWeek(int w) {
          String[] weekArray = {"星期日", "星期一", "星期二", "星期三", "星期四", "星期五", "星期六"};
         return weekArray[w - 1];
      }
  ```

### JDK8以后的时间类

| JDK8时间类类名    | 作用                   |
| ----------------- | ---------------------- |
| ZoneId            | 时区                   |
| Instant           | 时间戳                 |
| ZoneDateTime      | 带时区的时间           |
| DateTimeFormatter | 用于时间的格式化和解析 |
| LocalDate         | 年、月、日             |
| LocalTime         | 时、分、秒             |
| LocalDateTime     | 年、月、日、时、分、秒 |
| Duration          | 时间间隔（秒，纳，秒） |
| Period            | 时间间隔（年，月，日） |
| ChronoUnit        | 时间间隔（所有单位）   |

### 7.4  Date类

#### 7.4.1 ZoneId 时区

| 方法名                                    | 说明                     |
| ----------------------------------------- | ------------------------ |
| static Set\<string> getAvailableZoneIds() | 获取Java中支持的所有时区 |
| static ZoneId systemDefault()             | 获取系统默认时区         |
| static Zoneld of(string zoneld)           | 获取一个指定时区         |

```jAVA
//1.获取所有的时区名称
Set<String> zoneIds = ZoneId.getAvailableZoneIds();
System.out.println(zoneIds.size());//600
System.out.println(zoneIds);// 

//2.获取当前系统的默认时区
ZoneId zoneId = ZoneId.systemDefault();
System.out.println(zoneId);//Asia/Shanghai；与电脑选择的时区为标准

//3.获取指定的时区
ZoneId zoneId1 = ZoneId.of("Asia/Pontianak");
System.out.println(zoneId1);//Asia/Pontianak
```

#### 7.4.2 Instant 时间戳

| 方法名                                      | 说明                                |
| ------------------------------------------- | ----------------------------------- |
| static  Instant  now()                      | 获取当前时间的Instant对象(标准时间) |
| static Instant  ofEpochXxx(long epochMilli) | 根据(秒/毫秒/纳秒)获取Instant对象   |
| ZonedDateTime  atZone(ZoneIdzone)           | 获取指定时区时间                    |
| boolean  isxxx(Instant otherInstant)        | 比较时间早晚的方法                  |
| Instant  minusXxx(long millisToSubtract)    | 减少时间系列的方法                  |
| Instant  plusXxx(long millisToSubtract)     | 增加时间系列的方法                  |

```java
//1.获取当前时间的Instant对象(标准时间)
Instant instant = Instant.now();
System.out.println(instant);

//2.根据(秒/毫秒/纳秒)获取Instant对象
Instant instant1 = Instant.ofEpochMilli(0L);
System.out.println(instant1);//1970-01-01T00:00:00z

Instant instant2 = Instant.ofEpochSecond(1L);
System.out.println(instant2);//1970-01-01T00:00:01Z

Instant instant3 = Instant.ofEpochSecond(1L, 1000000000L);
System.out.println(instant3);//1970-01-01T00:00:02Z；前面为秒，后面为纳秒

//3. 获取指定时区时间
ZonedDateTime time = Instant.now().atZone(ZoneId.of("Asia/Shanghai"));
System.out.println(time);


//4.isXxx 比较时间
Instant instant4=Instant.ofEpochMilli(0L);
Instant instant5 =Instant.ofEpochMilli(1000L);

//4.1 isBefore:判断调用者代表的时间是否在参数表示时间的前面
boolean result1=instant4.isBefore(instant5);
System.out.println(result1);//true

//4.2 isAfter:判断调用者代表的时间是否在参数表示时间的后面
boolean result2 = instant4.isAfter(instant5);
System.out.println(result2);//false

//5.Instant minusXxx(long millisToSubtract) 减少时间系列的方法
Instant instant6 =Instant.ofEpochMilli(3000L);
System.out.println(instant6);//1970-01-01T00:00:03Z

Instant instant7 =instant6.minusSeconds(1);
System.out.println(instant7);//1970-01-01T00:00:02Z

```

#### 7.4.3 ZoneDateTime

| 方法                                | 说明                                |
| ----------------------------------- | ----------------------------------- |
| static ZonedDateTime now()          | 获取**当前时间**的ZonedDateTime对象 |
| static ZonedDateTime ofXxxx(。。。) | 获取指定时间的ZonedDateTime对象     |
| ZonedDateTime withXxx(时间)         | 修改时间系列的方法                  |
| ZonedDateTime minusXxx(时间)        | 减少时间系列的方法                  |
| ZonedDateTime plusXxx(时间)         | 增加时间系列的方法                  |

```java
//1.获取当前时间对象(带时区)
ZonedDateTime now = ZonedDateTime.now();
System.out.println(now);//时间[时区]

//2.获取指定的时间对象(带时区)1/年月日时分秒纳秒方式指定
ZonedDateTime time1 = ZonedDateTime.of(2023, 10, 1,
                                       11, 12, 12,0（纳秒）,
                                       ZoneId.of("Asia/Shanghai"));
System.out.println(time1);

//通过Instant + 时区的方式指定获取时间对象
Instant instant = Instant.ofEpochMilli(0L);
ZoneId zoneId = ZoneId.of("Asia/Shanghai");
ZonedDateTime time2 = ZonedDateTime.ofInstant(instant, zoneId);
System.out.println(time2);


//3.withXxx 修改时间系列的方法
ZonedDateTime time3 = time2.withYear(2000);
System.out.println(time3);

//4. 减少时间
ZonedDateTime time4 = time3.minusYears(1);
System.out.println(time4);

//5.增加时间
ZonedDateTime time5 = time4.plusYears(1);
System.out.println(time5);
```

- JDK8新增的时间对象都是不可变的
- 对对象的任何操作本质是创建一个新的对象

### 7.5 SimpleDateFormat类

DateTimeFormatter

| 方法                                     | 说明               |
| ---------------------------------------- | ------------------ |
| static DateTimeFormatter ofPattern(格式) | 获取格式对象       |
| String format(时间对象)                  | 按照指定方式格式化 |



```java
ZonedDateTime time = Instant.now().atZone(ZoneId.of("Asia/Shanghai"));
// 解析/格式化器
DateTimeFormatter dtf1=DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm;ss EE a");
// 格式化
System.out.println(dtf1.format(time));
```

### 7.6 Calendar类

通用方法

| 方法名                  | 说明                     |
| ----------------------- | ------------------------ |
| static  Xxx  now()      | 获取当前时间的对象       |
| static  Xxx  of(。。。) | 获取指定时间的对象       |
| getXxx()                | 获取日历中的年月日时分秒 |
| isBefore,isAfter        | 比较两个的LocalDate      |
| withXxx  ()             | 修改时间的方法           |
| minusXxx  ()            | 减少时间的方法           |
| plusXxx  ()             | 增加时间的方法           |

LocalDateTime方法转换

| 方法名                           | 说明                             |
| -------------------------------- | -------------------------------- |
| public  LocalDate  toLocalDate() | LocalDateTime转换成LocalDate对象 |
| public  LocalTime  toLocalDate() | LocalDateTime转换成LocalTime对象 |

#### 7.6.2 LocalDate 年月日

```java
//1.获取当前时间的日历对象(包含 年月日)
LocalDate nowDate = LocalDate.now();
//System.out.println("今天的日期:" + nowDate);
//2.获取指定的时间的日历对象
LocalDate ldDate = LocalDate.of(2023, 1, 1);
System.out.println("指定日期:" + ldDate);

System.out.println("=============================");

//3.get系列方法获取日历中的每一个属性值//获取年
int year = ldDate.getYear();
System.out.println("year: " + year);
//获取月
//方式一:
Month m = ldDate.getMonth();
System.out.println(m);				//英文的月份
System.out.println(m.getValue());	//月份的数值
//方式二:直接获取月值
int month = ldDate.getMonthValue();
System.out.println("month: " + month);

//获取日
int day = ldDate.getDayOfMonth();
System.out.println("day:" + day);

//获取一年的第几天
int dayofYear = ldDate.getDayOfYear();
System.out.println("dayOfYear:" + dayofYear);

//获取星期
DayOfWeek dayOfWeek = ldDate.getDayOfWeek();
System.out.println(dayOfWeek);
System.out.println(dayOfWeek.getValue());

//is开头的方法表示判断
System.out.println(ldDate.isBefore(ldDate));
System.out.println(ldDate.isAfter(ldDate));

//with开头的方法表示修改，只能修改年月日
LocalDate withLocalDate = ldDate.withYear(2000);
System.out.println(withLocalDate);

//minus开头的方法表示减少，只能减少年月日
LocalDate minusLocalDate = ldDate.minusYears(1);
System.out.println(minusLocalDate);


//plus开头的方法表示增加，只能增加年月日
LocalDate plusLocalDate = ldDate.plusDays(1);
System.out.println(plusLocalDate);

//-------------
// 判断今天是否是你的生日
LocalDate birDate = LocalDate.of(2000, 1, 1);
LocalDate nowDate1 = LocalDate.now();

MonthDay birMd = MonthDay.of(birDate.getMonthValue(), birDate.getDayOfMonth());
MonthDay nowMd = MonthDay.from(nowDate1);

System.out.println("今天是你的生日吗? " + birMd.equals(nowMd));//今天是你的生日吗?
```

#### 7.6.3 LocalTIme 时、分、秒

```java
// 获取本地时间的日历对象。(包含 时分秒)
LocalTime nowTime = LocalTime.now();
System.out.println("今天的时间:" + nowTime);

//获取时间
int hour = nowTime.getHour();//时
System.out.println("hour: " + hour);

int minute = nowTime.getMinute();//分
System.out.println("minute: " + minute);

int second = nowTime.getSecond();//秒
System.out.println("second:" + second);

int nano = nowTime.getNano();//纳秒

//of()设置时间
System.out.println("nano:" + nano);
System.out.println("------------------------------------");
System.out.println(LocalTime.of(8, 20));//时分
System.out.println(LocalTime.of(8, 20, 30));//时分秒
System.out.println(LocalTime.of(8, 20, 30, 150));//时分秒纳秒
LocalTime mTime = LocalTime.of(8, 20, 30, 150);

//is系列的方法
System.out.println(nowTime.isBefore(mTime));
System.out.println(nowTime.isAfter(mTime));

//with系列的方法，只能修改时、分、秒
System.out.println(nowTime.withHour(10));

//plus系列的方法，只能修改时、分、秒
System.out.println(nowTime.plusHours(10));
```

#### 7.6.4 LocalDateTime

年、月、日、时、分、秒

```java
// 当前时间的的日历对象(包含年月日时分秒)
LocalDateTime nowDateTime = LocalDateTime.now();

System.out.println("今天是:" + nowDateTime);//今天是：
System.out.println(nowDateTime.getYear());//年
System.out.println(nowDateTime.getMonthValue());//月
System.out.println(nowDateTime.getDayOfMonth());//日
System.out.println(nowDateTime.getHour());//时
System.out.println(nowDateTime.getMinute());//分
System.out.println(nowDateTime.getSecond());//秒
System.out.println(nowDateTime.getNano());//纳秒
// 日:当年的第几天
System.out.println("dayofYear:" + nowDateTime.getDayOfYear());
//星期
System.out.println(nowDateTime.getDayOfWeek());
System.out.println(nowDateTime.getDayOfWeek().getValue());
//月份
System.out.println(nowDateTime.getMonth());
System.out.println(nowDateTime.getMonth().getValue());

LocalDate ld = nowDateTime.toLocalDate();
System.out.println(ld);

LocalTime lt = nowDateTime.toLocalTime();
System.out.println(lt.getHour());
System.out.println(lt.getMinute());
System.out.println(lt.getSecond());
```

### 7.7 工具类

#### 7.7.1 Duration  时间间隔

（秒，纳，秒）

```java
// 本地日期时间对象。
LocalDateTime today = LocalDateTime.now();
System.out.println(today);

// 出生的日期时间对象
LocalDateTime birthDate = LocalDateTime.of(2000, 1, 1, 0, 0, 0);
System.out.println(birthDate);

Duration duration = Duration.between(birthDate, today);//第二个参数减第一个参数
System.out.println("相差的时间间隔对象:" + duration);

System.out.println("============================================");
System.out.println(duration.toDays());//两个时间差的天数
System.out.println(duration.toHours());//两个时间差的小时数
System.out.println(duration.toMinutes());//两个时间差的分钟数
System.out.println(duration.toMillis());//两个时间差的毫秒数
System.out.println(duration.toNanos());//两个时间差的纳秒数
```



#### 7.7.2 Period  时间间隔

（年，月，日）

```java
// 当前本地 年月日
LocalDate today = LocalDate.now();
System.out.println(today);

// 生日的 年月日
LocalDate birthDate = LocalDate.of(2000, 1, 1);
System.out.println(birthDate);

Period period = Period.between(birthDate, today);//第二个参数减第一个参数

System.out.println("相差的时间间隔对象:" + period);
System.out.println(period.getYears());
System.out.println(period.getMonths());
System.out.println(period.getDays());

System.out.println(period.toTotalMonths());
```



#### 7.7.3 ChronoUnit  时间间隔

（所有单位）

```java
// 当前时间
LocalDateTime today = LocalDateTime.now();
System.out.println(today);
// 生日时间
LocalDateTime birthDate = LocalDateTime.of(2000, 1, 1,0, 0, 0);
System.out.println(birthDate);
// 得到的都是long类型的数值
System.out.println("相差的年数:" + ChronoUnit.YEARS.between(birthDate, today));
System.out.println("相差的月数:" + ChronoUnit.MONTHS.between(birthDate, today));
System.out.println("相差的周数:" + ChronoUnit.WEEKS.between(birthDate, today));
System.out.println("相差的天数:" + ChronoUnit.DAYS.between(birthDate, today));
System.out.println("相差的时数:" + ChronoUnit.HOURS.between(birthDate, today));
System.out.println("相差的分数:" + ChronoUnit.MINUTES.between(birthDate, today));
System.out.println("相差的秒数:" + ChronoUnit.SECONDS.between(birthDate, today));
System.out.println("相差的毫秒数:" + ChronoUnit.MILLIS.between(birthDate, today));
System.out.println("相差的微秒数:" + ChronoUnit.MICROS.between(birthDate, today));
System.out.println("相差的纳秒数:" + ChronoUnit.NANOS.between(birthDate, today));
System.out.println("相差的半天数:" + ChronoUnit.HALF_DAYS.between(birthDate, today));
System.out.println("相差的十年数:" + ChronoUnit.DECADES.between(birthDate, today));
System.out.println("相差的世纪(百年)数:" + ChronoUnit.CENTURIES.between(birthDate, today));
System.out.println("相差的千年数:" + ChronoUnit.MILLENNIA.between(birthDate, today));
System.out.println("相差的纪元数:" + ChronoUnit.ERAS.between(birthDate, today));
```

