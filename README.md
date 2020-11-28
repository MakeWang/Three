# Three
Three 遍历

## 目录
* [二叉树遍历方式](#二叉树遍历)
  * 前序遍历----迭代、栈
  * 中序遍历----迭代、栈
  * 后序遍历----迭代、栈
 * [java代码](#java代码)
 * [反向生成二叉树](#反向生成二叉树)


-----------
![baidu](https://github.com/MakeWang/Three/blob/master/image/three.png "二叉树")


-----------
 # 二叉树遍历
 * 前序遍历


  -----------
 # java代码
 ```java
 public class ThreeTest {

    private ThreeNode mRoot;

    public ThreeTest(){
        mRoot = new ThreeNode(1,"A");
    }

    public void createThree(){
        ThreeNode b = new ThreeNode(1,"B");
        ThreeNode c = new ThreeNode(1,"C");
        ThreeNode d = new ThreeNode(1,"D");
        ThreeNode e = new ThreeNode(1,"E");
        ThreeNode f = new ThreeNode(1,"F");
        ThreeNode g = new ThreeNode(1,"G");

        mRoot.leftNode = b;
        mRoot.rightNode = c;
        b.leftNode = d;
        b.rightNode = e;
        e.rightNode = g;
        c.rightNode = f;
    }

    //查询树的深度
    public int getHeight(ThreeNode root){
        if(root == null){
            return 0;
        }
        int l = getHeight(root.leftNode);
        int r = getHeight(root.rightNode);
        return l < r ? r + 1 : l + 1;
    }

    //获取树的节点
    private int getSize(ThreeNode root) {
        if(root == null){
            return 0;
        }
        return getSize(root.leftNode) + getSize(root.rightNode) + 1;
    }

    //前序遍历-----迭代 (中-左-右)
    public void leftSelect(ThreeNode root){
        if(root == null){
            return;
        }
        System.out.println(root.data);
        leftSelect(root.leftNode);
        leftSelect(root.rightNode);
    }
    //前序遍历-----栈 (中-左-右)
    public void leftSelect2(ThreeNode root){
        if(root == null){
            return;
        }
        Stack<ThreeNode> stack = new Stack<>();
        stack.push(root);
        while (!stack.isEmpty()){
            ThreeNode pop = stack.pop();
            if(pop.rightNode != null){
                stack.push(pop.rightNode);
            }
            if(pop.leftNode != null){
                stack.push(pop.leftNode);
            }
            System.out.println(pop.data);
        }
    }

    //中序遍历-----迭代 (左-中-右)
    public void centerSelect(ThreeNode root){
        if(root == null){
            return;
        }
        centerSelect(root.leftNode);
        System.out.println(root.data);
        centerSelect(root.rightNode);
    }

    //中序遍历-----栈 (左-中-右)
    public void centerSelect2(ThreeNode root){
        Stack<ThreeNode> stack = new Stack<>();
        //指针节点
        ThreeNode p = root;
        while (p != null || !stack.isEmpty()){
            while (p != null){
                stack.push(p);
                p = p.leftNode;
            }
            p = stack.pop();
            System.out.println(p.data);
            if(p.rightNode != null){
                p = p.rightNode;
            }else{
                p = null;
            }
        }
    }

    //后序遍历-----迭代 (左-右-中)
    public void rightSelect(ThreeNode root){
        if(root == null){
            return;
        }
        rightSelect(root.leftNode);
        rightSelect(root.rightNode);
        System.out.println(root.data);
    }

    //后序遍历-----栈 (左-右-中)
    public void rightSelect2(ThreeNode root){
        Stack<ThreeNode> stack = new Stack<>();
        ThreeNode p = root;
        while (p != null || !stack.isEmpty()){
            while (p != null){
                stack.push(p);
                if(p.leftNode != null){
                    p = p.leftNode;
                }else{
                    p = p.rightNode;
                }
            }

            p = stack.pop();
            System.out.println(p.data);

            if(!stack.isEmpty() && p == stack.peek().leftNode){
                p = stack.peek().rightNode;
            }else{
                p = null;
            }
        }
    }

    private class ThreeNode{
        private int index;
        private String data;
        private ThreeNode leftNode;
        private ThreeNode rightNode;

        ThreeNode(int index, String data){
            this.index = index;
            this.data = data;
            this.leftNode = null;
            this.rightNode = null;
        }


    }

    public static void main(String []args){
        ThreeTest three = new ThreeTest();
        three.createThree();
        System.out.println("树的深度是："+three.getHeight(three.mRoot));
        System.out.println("树的节点总和是："+three.getSize(three.mRoot));
        //three.leftSelect(three.mRoot);
        //three.leftSelect2(three.mRoot);
        //three.centerSelect(three.mRoot);
        //three.centerSelect2(three.mRoot);
        //three.rightSelect(three.mRoot);
        three.rightSelect2(three.mRoot);

    }

}

 ```
 
 -----------
 # 反向生成二叉树
 ```java
 //"A","B","D","#","#","E","#","G","C","#","F","#","#"
    private ThreeNode getLeftCreateShu(ArrayList<String> arr){
        if(arr.size() == 0){
            return null;
        }
        ThreeNode threeNode;
        String item = arr.get(0);
        if("#".equals(item)){
            arr.remove(0);
            return null;
        }
        threeNode = new ThreeNode(0,item);
        if(mRoot == null){
            mRoot = threeNode;
        }
        arr.remove(0);
        threeNode.leftNode = getLeftCreateShu(arr);
        threeNode.rightNode = getLeftCreateShu(arr);
        return threeNode;
    }
  ```



# c初探：数据类型、数组、内存布局、指针

> **windows命令行 (可以不用搭理这里，在Linux玩就行)**
>
> Windows C/C++编译器： https://sourceforge.net/projects/mingw/files/
>
> ![mingw](mingw.png)
>
> 配置环境变量 PATH: ${MinGW安装目录}/MinGW/bin
>
> ![环境变量](环境变量.png)



> c与c++
>
>   C语言是一门通用计算机编程语言，广泛应用于底层开发。
>
>   c语句是面向过程的语言，c++是面向对象的语言，C++对c进行扩展。
>
>   c是c++的子集,c++是c的超集，所以大部c语言程序都可以不加修改的拿到c++下使用。

## 1、基本数据类型

 	1.**signed**----有符号，可修饰char、int。Int是默认有符号的。
	2.**unsigned**-----无符号，修饰int 、char



| 整型           | 字节 | 取值范围                        | 占位 |
| :------------- | ---- | ------------------------------- | ---- |
| int            | 4    | -2,147,483,648 到 2,147,483,647 | %d   |
| unsigned int   | 4    | 0 到 4,294,967,295              | %u   |
| short          | 2    | -32,768 到 32,767               | %hd  |
| unsigned short | 2    | 0 到 65,535                     | %hu  |
| long           | 4    | -2,147,483,648 到 2,147,483,647 | %ld  |
| unsigned long  | 4    | 0 到 4,294,967,295              | %lu  |
| char           | 1    | -128 到 127                     | %c   |
| unsigned char  | 1    | 0 到 255                        | %c   |

> 为了得到某个类型或某个变量在特定平台上的准确大小，使用 sizeof 运算符。
>
> 表达式 sizeof(type) 得到对象或类型的存储字节大小。

long int 其实就是长整型 = long 可以省去int
在标准中,规定 int至少和short一样长，long至少和int一样长。

> 为什么会存在long?
>
> long和int在早期16位电脑时候 int 2字节，long 4字节，而计算机发展到现在，一般32、64下，long和int一样。和java类比的话，java的long就是 long long 8字节

> 格式化还有： 
>
> 8进制 			%o
> 16进制			小写： %x    大写：%X
> (0x)+16进制前面 	%#x 



| 浮点型      | 字节 | 精度     | 占位 |
| ----------- | ---- | -------- | ---- |
| float       | 4    | 6位小数  | %f   |
| double      | 8    | 15位小数 | %lf  |
| long double | 8    | 19位小数 | %Lf  |



C99标准以前，C语言里面是没有bool，C++里面才有，
C99标准里面定义了bool类型，需要引入头文件stdbool.h
bool类型有只有两个值：true =1 、false=0。
因此实际上bool就是一个int
所以在c/c++中 if 遵循一个规则， 非0为true，非空为true；
NULL 其实也就是被define为了 0 



## 2、格式化

#include <stdio.h>

printf、sprintf等

sprintf:

​	将格式化的数据写入第一个参数

```c
char str[100];
sprintf(str, "img/png_%d.png", 1);
printf("%s", str);

//使用 0 补到3个字符
sprintf(str, "img/png_%03d.png", 1);
printf("%s", str);
```





## 3、数组与内存布局

> 数组  ： 连续的内存

```c
//java
int[] a

//c
//必须声明时候确定大小
int a[10]  
//或者 直接初始化 
int a[] = {1,2,3}

//大小
printf("%d",sizeof(a)/sizeof(int));
```

> 栈内存限制  linux：ulimit -a 查看
> 但是直接分配这么大不行，因为堆栈可能保存参数，返回地址等等信息



#### 动态内存申请

**malloc** 
	没有初始化内存的内容,一般调用函数memset来初始化这部分的内存空间.

**calloc**
	申请内存并将初始化内存数据为NULL.

​	 ` int *pn = (int*)calloc(10, sizeof(int));`

**realloc**

​	 对malloc申请的内存进行大小的调整.

```c
char *a = (char*)malloc(10);
realloc(a,20);
```

特别的：
**alloca**
	在栈申请内存,因此无需释放.
`int *p = (int *)alloca(sizeof(int) * 10);`



**物理内存**
	物理内存指通过物理内存条而获得的内存空间

**虚拟内存**
	一种内存管理技术
	电脑中所运行的程序均需经由内存执行，若执行的程序占用内存很大，则会导致内存消耗殆尽。
	虚拟内存技术还会匀出一部分硬盘空间来充当内存使用。



![](内存布局.png)

![内存布局解释](内存布局解释.jpg)







代码段：
存放程序执行代码（cpu要执行的指令）

> 栈是向低地址扩展数据结构
> 堆是向高地址扩展数据结构

进程分配内存主要由两个系统调用完成：**brk和mmap** 。

1. brk是将_edata(指带堆位置的指针)往高地址推；
2. mmap 找一块空闲的虚拟内存。



通过glibc (C标准库)中提供的malloc函数完成内存申请

malloc小于128k的内存，使用brk分配内存，将_edata往高地址推,大于128k则使用mmap

![brk申请内存](brk申请内存.png)







