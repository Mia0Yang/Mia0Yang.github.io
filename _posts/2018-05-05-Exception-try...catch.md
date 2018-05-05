---
layout: post
title: 'JAVA try...catch块的异常捕获'
subtitle: 'Error与Exception'
date: 2018-05-05
author: Mia0Yang
cover: '/screenshot/Exception-Theme-1.png'
tags: JAVA
---

> 关于catch块中Exception与Error类型捕获的不同

### OutOfMemoryError

这次的JAVA作业中一道题需要制造并且捕获OutOfMemory的错误，因为不太熟悉异常处理，加上这个程序本身需要运行一定时间，然后成功把自己弄糊涂了

![avatar](/screenshot/Exception-Task2.png)

### try...catch块捕获异常

使用try-catch块捕获异常，分为三种情况：
<img src="/screenshot/Exception-Task2-01.png" width="500" height="256" />
<img src="/screenshot/Exception-Task2-02.png" width="500" height="256" />
<img src="/screenshot/Exception-Task2-03.png" width="500" height="256" />

在这个程序中为了以数组溢出的方式捕获异常，我用了一个死循环来制造错误，想输出内存情况，但是最后得到的是系统的异常。

```java
public class TestArrayList {
	public static void main(String[] args) {
		Point[] point = new Point[1000000];
		MyArrayList arrayList = new MyArrayList(1000000);
		try{
			while(true){
				arrayList.add(point);
			}
		}
		catch(Exception e){
			Runtime run = Runtime.getRuntime();
			System.out.println("1.空闲的内存空间： "+run.freeMemory());
			System.out.println("2.总内存为："+run.totalMemory());
			System.out.println("3.最大可占内存：" + run.maxMemory());
			System.gc();
		}
	}
}
```
结果得到的结果为：
```
Exception in thread "main" java.lang.OutOfMemoryError: Java heap space
	at com.miao.yang.MyArrayList.add(MyArrayList.java:30)
	at com.miao.yang.TestArrayList.main(TestArrayList.java:9)

```

原本以为Exception可以捕获所有的异常，但是发现并不能。然后我又把Exception换成了OutOfMemoryError有执行了一次，发现成功了。

```java
public class TestArrayList {
	public static void main(String[] args) {
		Point[] point = new Point[100000000];
		MyArrayList arrayList = new MyArrayList(1000000);
		try{
			while(true){
				arrayList.add(point);
			}
		}
		catch(OutOfMemoryError e){
			Runtime run = Runtime.getRuntime();
			System.out.println("1.空闲的内存空间： "+run.freeMemory());
			System.out.println("2.总内存为："+run.totalMemory());
			System.out.println("3.最大可占内存：" + run.maxMemory());
			System.gc();
		}
	}
}
```

输出结果为：
```
1.空闲的内存空间： 165631952
2.总内存为：259522560
3.最大可占内存：259522560
```

这时候突然想起了try...catch块的异常并不是只有Exception一种。异常中Throwable是Exception和Error类的父类.其中Error代表仅靠程序本身无法恢复的严重错误 ，而Exception代表由Java应用程序抛出和处理的非严重错误 。本题中的OutOfMemoryError并不属于Exception，导致结果不同。


<img src="/screenshot/Exception-Task2-04.png" width="500" height="256" />


### JAVA常见异常类型

* 异常层次结构的父类: Exception 

* 算术异常类(如以零作除数)：ArithmeticExecption

* 空指针异常类：NullPointerException

* 不能加载所需的类: ClassNotFoundException

* 类型强制转换异常：ClassCastException

* 数组负下标异常：NegativeArrayException

* 数组下标越界异常：ArrayIndexOutOfBoundsException

* 违背安全原则异常：SecturityException

* 文件已结束异常：EOFException

* 文件未找到异常：FileNotFoundException

* 字符串转换为数字异常：NumberFormatException

* 操作数据库异常：SQLException

* 输入输出异常：IOException

除了系统异常，用户也可以自定义异常，在编写自己的异常类时大概要注意如下的几点：
* 所有异常都必须是 Throwable 的子类。
* 若写一个检查性异常类，需要继承 Exception 类。
* 若写一个运行时异常类，需要继承 RuntimeException 类。

<img src="/screenshot/Exception-Theme-2.png">