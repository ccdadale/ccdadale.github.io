---
layout: post
title: "static 和 final 的作用"
date: 2016-09-12 17:47:00
categories:Java知识
tags: final static 面试
author: CaiBoWei
---



转载至:[leizhimin 的BLOG](http://lavasoft.blog.51cto.com/62575/18771/)


[TOC]
# static和final的作用 
## static
static 表示"全局"或者静态的意思，用来**修饰成员变量和成员方法**也可以形成**static代码块**，Java中并没有**全局变量**的概念。

**被静态修饰的成员变量和成员方法独立于该类的任何对象**，也就是说，它不依赖于类的特定实例，被该类的所有实例共享。只要这个类被加载，Java虚拟机就可以根据类名在运行时数据区的特定方法找到它们。**因此，static对象可以在它的任何对象创建之前访问，无需任何引用对象。**

**使用public修饰的static成员变量和成员方法本质是全局变量和全局方法，当声明它类的对象时，不生成static副本，而是所有实例共享同一个static变量**

**static变量前面可以被private修饰，表示这个变量可以在类的静态代码块中，或者其他静态成员方法中使用（也可作在非静态成员方法中使用），*但是不能再其它类中通过类名直接引用***，需要清楚的是，**private是访问权限，static表示不实例化就可以使用**，static前面加上其它的访问权限也是相同的。

static修饰的成员变量和成员方法习惯称为静态变量和静态方法，可以直接通过类名来访问，访问语法为：

 1. 类名.静态方法名(参数列表)
 2. 类名.静态变量名

用static修饰的代码块表示静态代码块，当JVM加载类时，就会执行该代码块。

**static变量**
1.  按照是否静态的对类成员变量进行分类可分两种：一种是被static修饰的变量，叫静态变量或类变量；另一种是没有被static修饰的变量，叫实例变量。两者的区别是：
        对于静态变量在内存中只有一个拷贝（节省内存），JVM只为静态分配一次内存，在加载类的过程中完成静态变量的内存分配，可用类名直接访问（方便），当然也可以通过对象来访问（但是这是不推荐的）。
        对于实例变量，每创建一个实例，就会为实例变量分配一次内存，实例变量可以在内存中有多个拷贝，互不影响（灵活）。


**静态方法**
2.静态方法可以直接通过类名调用，任何的实例也都可以调用，因此静态方法中不能用t    his和super关键字，不能直接访问所属类的实例变量和实例方法(就是不带static的    成员变量和成员成员方法)，只能访问所属类的静态成员变量和成员方法。
        因为static方法独立于任何实例，因此static方法必须被实现，而不能是抽象的abstract。


**static代码块**

3. static代码块也叫静态代码块，是在类中独立于类成员的static语句块，可以有多个，位置可以随便放，它不在任何的方法体内，JVM加载类时会执行这些静态的代码块，如果static代码块有多个，JVM将按照它们在类中出现的先后顺序依次执行它们，每个代码块只会被执行一次。例如：


    public class Test { 
        private static int a; 
        private int b; 
    
        static { 
                Test5.a = 3; 
                System.out.println(a); 
                Test5 t = new Test5(); 
                t.f(); 
                t.b = 1000; 
                System.out.println(t.b); 
        } 
    
        static { 
                Test5.a = 4; 
                System.out.println(a); 
        } 
    
        public static void main(String[] args) { 
                // TODO 自动生成方法存根 
        } 
    
        static { 
                Test5.a = 5; 
                System.out.println(a); 
        } 
    
        public void f() { 
                System.out.println("hello"); 
        } 
}

结果为：
3
hello
1000
4
5

利用静态代码块可以对一些static变量进行赋值，最后再看一眼这些例子，都一个static的main方法，这样JVM在运行main方法的时候可以直接调用而不用创建实例。

## final ##

 根据程序上下文环境，Java关键字final有“这是无法改变的”或者“终态的”含义，它可以修饰非抽象类、非抽象类成员方法和变量。你可能出于两种理解而需要阻止改变：设计或效率。
        **final类**不能被继承，没有子类，final类中的方法默认是final的。
        **final方法**不能被子类的方法覆盖，但可以被继承。
        **final成员变量**表示常量，只能被赋值一次，赋值后值不再改变。
        final不能用于修饰构造方法。
        **注意：父类的private成员方法是不能被子类方法覆盖的，因此private类型的方法默认是final类型的。**

1.  final类
     final类不能被继承，因此final类的成员方法没有机会被覆盖，默认都是final的。在设计类时候，如果这个类不需要有子类，类的实现细节不允许改变，并且确信这个类不会载被扩展，那么就设计为final类。
2.  final方法
     如果一个类不允许其子类覆盖某个方法，则可以把这个方法声明为final方法。
         使用final方法的原因有二：
         第一、把方法锁定，防止任何继承类修改它的意义和实现。
         第二、高效。编译器在遇到调用final方法时候会转入内嵌机制，大大提高执行效率。


    public class Test1 { 
        public static void main(String[] args) { 
        // TODO 自动生成方法存根 
        } 
        public void f1() { 
        System.out.println("f1"); 
        }
        
        //无法被子类覆盖的方法 
        public final void f2() { 
            System.out.println("f2"); 
        } 
        
        public void f3() { 
            System.out.println("f3"); 
        } 
        
        private void f4() { 
            System.out.println("f4"); 
        } 
    } 
    
    public class Test2 extends Test1 { 
    
        public void f1(){     
            System.out.println("Test1父类方法f1被覆盖!"); 
        } 
        public static void main(String[] args) { 
            Test2 t=new Test2(); 
            t.f1();    
            t.f2(); //调用从父类继承过来的final方法 
            t.f3(); //调用从父类继承过来的方法 
            //t.f4(); //调用失败，无法从父类继承获得 
        } 
    }
3.  final常量
    用final修饰的成员变量表示常量，值一旦给定就无法改变！
        final修饰的变量有三种：静态变量、实例变量和局部变量，分别表示三种类型的常量。
        从下面的例子中可以看出，一旦给final变量初值后，值就不能再改变了。
        另外，final变量定义的时候，可以先声明，而不给初值，这中变量也称为final空白，无论什么情况，编译器都确保空白final在使用之前必须被初始化。但是，final空白在final关键字final的使用上提供了更大的灵活性，为此，一个类中的final数据成员就可以实现依对象而有所不同，却有保持其恒定不变的特征。

    ​

    public class Test3 { 
        private final String S = "final实例变量S"; 
        private final int A = 100; 
        public final int B = 90; 
        
        public static final int C = 80; 
        private static final int D = 70; 
        
        public final int E; //final空白,必须在初始化对象的时候赋初值 
        
        public Test3(int x) { 
                E = x; 
        } 
        
        /** 
         * @param args 
         */ 
        public static void main(String[] args) { 
                Test3 t = new Test3(2); 
                //t.A=101;    //出错,final变量的值一旦给定就无法改变 
                //t.B=91; //出错,final变量的值一旦给定就无法改变 
                //t.C=81; //出错,final变量的值一旦给定就无法改变 
                //t.D=71; //出错,final变量的值一旦给定就无法改变 
        
                System.out.println(t.A); 
                System.out.println(t.B); 
                System.out.println(t.C); //不推荐用对象方式访问静态字段 
                System.out.println(t.D); //不推荐用对象方式访问静态字段 
                System.out.println(Test3.C); 
                System.out.println(Test3.D); 
                //System.out.println(Test3.E); //出错,因为E为final空白,依据不同对象值有所不同. 
                System.out.println(t.E); 
        
                Test3 t1 = new Test3(3); 
                System.out.println(t1.E); //final空白变量E依据对象的不同而不同 
        } 
        
        private void test() { 
                System.out.println(new Test3(1).A); 
                System.out.println(Test3.C); 
                System.out.println(Test3.D); 
        } 
        
        public void test2() { 
                final int a;     //final空白,在需要的时候才赋值 
                final int b = 4;    //局部常量--final用于局部变量的情形 
                final int c;    //final空白,一直没有给赋值.    
                a = 3; 
                //a=4;    出错,已经给赋过值了. 
                //b=2; 出错,已经给赋过值了. 
        } 
    }

4.  final参数
     当函数参数为final类型时，你可以读取使用该参数，但是无法改变该参数的值。


    public class Test4 { 
        public static void main(String[] args) { 
                new Test4().f1(2); 
        } 
        
        public void f1(final int i) { 
                //i++;    //i是final类型的,值不允许改变的. 
                System.out.print(i); 
        } 
    }


## static和final在一起用来表示？ ##
使用static和final来修饰的成员方法和成员变量，可以被称为"全局变量"

对于变量而言，表示不可被修改，可以通过类名直接访问
对于方法而言，表示不可被覆盖，可以通过类名直接访问



## 另，静态内部类 ##

有时使用内部类只是为了把一个类隐藏在另一个类的内部，并不需要内部类引用外围类对象。为此可以将内部类声明为static，以便取消产生的引用。


​    
​    
## 笔试题 ##

1.下列程序输出结构

    public class helloB extends helloA{
        public helloB()
        {
            System.out.println("I'am class b");
        }
        
        static {
            System.out.println("static b");
        }
        
        public static void main(String [] args)
        {
            new helloB();
        }
    }
    
    class helloA()
    {
        public helloA()
        {
            System.out.println("I'am class A");
        }
        
        static {
            System.out.println("I'am class a");
        }
        
    }

2.下列程序是否正确，若正确结果为：

    public class HasStatic{
        private static int x =100;
        
        public static void main(String[] args)
        {
            HasStatic hs1 = new HasStatic();
            hs1.x++;
            
            HasStatic hs2 = new HasStatic();
            hs2.x++;
            
            hs1 = new HasStatic();
            hs1.x++;
            
            HasStatic.x--;
            
            System.out.println("x="+x);
        }
    }



