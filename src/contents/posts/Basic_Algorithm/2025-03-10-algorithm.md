---
title: 算法
published: 2025-03-10
description: 在数学和计算机科学领域，算法是一系列有限的严谨指令，通常用于解决一类特定问题或执行计算
tags: [Markdown, Blogging]
category: Basic Algorithm
licenseName: "Unlicensed"
author: Ankou
sourceLink: ''
draft: false
---
# 一. 初识算法

## 1.1 什么是算法？

**定义**

在数学和计算机科学领域，算法是一系列有限的严谨指令，通常用于解决一类特定问题或执行计算

> In mathematics and computer science, an **algorithm** (/ˈælɡərɪðəm/) is a finite sequence of rigorous instructions, typically used to solve a class of specific problems or to perform a computation.[^1]



**Introduction to Algorithm[^2]**

不正式的说，算法就是任何定义优良的计算过程：接收一些值作为输入，在有限的时间内，产生一些值作为输出。

> Informally, an algorithm is any well-defined computational procedure that takes some value, or set of values, as input and produces some value, or set of values, as output in a finite amount of time.



## 1.2 什么是数据结构？

**定义**

在计算机科学领域，数据结构是一种数据组织、管理和存储格式，通常被选择用来高效访问数据

> In computer science, a **data structure** is a data organization, management, and storage format that is usually chosen for efficient access to data



**Introduction to Algorithm[^2]**

数据结构是一种存储和组织数据的方式，旨在便于访问和修改

> A data structure is a way to store and organize data in order to facilitate access and modifications



可以说，**程序 = 数据结构 + 算法**，它们是每一位程序员的基本功，下来我们通过对一个非常著名的二分查找算法的讲解来认识一下算法



## 1.3 二分查找 [^3]

二分查找算法也称折半查找，是一种非常高效的工作于有序数组的查找算法。后续的课程中还会学习更多的查找算法，但在此之前，不妨用它作为入门。

### 1) 基础版

需求：在**有序**数组 $A$ 内，查找值 $target$

* 如果找到返回索引
* 如果找不到返回 $-1$

算法描述

|      |                                                              |
| ---- | ------------------------------------------------------------ |
| 前提 | 给定一个内含 $n$ 个元素的有序数组 $A$，满足 $A_{0}\leq A_{1}\leq A_{2}\leq \cdots \leq A_{n-1}$，一个待查值 $target$ |
| 1    | 设置 $i=0$，$j=n-1$                                          |
| 2    | 如果 $i \gt j$，结束查找，没找到                             |
| 3    | 设置 $m = floor(\frac {i+j}{2})$ ，$m$ 为中间索引，$floor$ 是向下取整（$\leq \frac {i+j}{2}$ 的最小整数） |
| 4    | 如果 $target < A_{m}$ 设置 $j = m - 1$，跳到第2步            |
| 5    | 如果 $A_{m} < target$ 设置 $i = m + 1$，跳到第2步            |
| 6    | 如果 $A_{m} = target$，结束查找，找到了                      |

> ***P.S.***
>
> * 对于一个算法来讲，都有较为严谨的描述，上面是一个例子
> * 后续讲解时，以简明直白为目标，不会总以上面的方式来描述算法



java 实现


```java
public static int binarySearch(int[] a, int target) {
    int i = 0, j = a.length - 1;
    while (i <= j) {
        int m = (i + j) >>> 1;
        if (target < a[m]) {			// 在左边
            j = m - 1;
        } else if (a[m] < target) {		// 在右边
            i = m + 1;
        } else {
            return m;
        }
    }
    return -1;
}
```

* $i,j$ 对应着搜索区间 $[0,a.length-1]$（注意是闭合的区间），$i<=j$ 意味着搜索区间内还有未比较的元素，$i,j$ 指向的元素也可能是比较的目标
  * 思考：如果不加 $i==j$ 行不行？
  * 回答：不行，因为这意味着 $i,j$ 指向的元素会漏过比较
* $m$ 对应着中间位置，中间位置左边和右边的元素可能不相等（差一个），不会影响结果
* 如果某次未找到，那么缩小后的区间内不包含 $m$



### 2) 改变版

另一种写法

```java
public static int binarySearch(int[] a, int target) {
    int i = 0, j = a.length;
    while (i < j) {
        int m = (i + j) >>> 1;
        if (target < a[m]) {			// 在左边
            j = m;
        } else if (a[m] < target) {		// 在右边
            i = m + 1;
        } else {
            return m;
        }
    }
    return -1;
}
```

* $i,j$ 对应着搜索区间 $[0,a.length)$（注意是左闭右开的区间），$i<j$ 意味着搜索区间内还有未比较的元素，$j$ 指向的**一定不是**查找目标
  * 思考：为啥这次不加 $i==j$ 的条件了？
  * 回答：这回 $j$ 指向的不是查找目标，如果还加 $i==j$ 条件，就意味着 $j$ 指向的还会再次比较，找不到时，会死循环
* 如果某次要缩小右边界，那么 $j=m$，因为此时的 $m$ 已经**不是**查找目标了



## 1.4 衡量算法好坏

**时间复杂度**

下面的查找算法也能得出与之前二分查找一样的结果，那你能说出它差在哪里吗？

```java
public static int search(int[] a, int k) {
    for (
        int i = 0;
        i < a.length;
        i++
    ) {
        if (a[i] == k) {
            return i;
        }
    }
    return -1;
}
```

考虑最坏情况下（没找到）例如 `[1,2,3,4]` 查找 5

* `int i = 0` 只执行一次
* `i < a.length` 受数组元素个数 $n$ 的影响，比较 $n+1$ 次
* `i++` 受数组元素个数 $n$ 的影响，自增 $n$ 次
* `a[i] == k` 受元素个数 $n$ 的影响，比较 $n$ 次
* `return -1`，执行一次

粗略认为每行代码执行时间是 $t$，假设 $n=4$ 那么

* 总执行时间是 $(1+4+1+4+4+1)*t = 15t$
* 可以推导出更一般地公式为，$T = (3*n+3)t$ 



如果套用二分查找算法，还是 `[1,2,3,4]` 查找 5

```java
public static int binarySearch(int[] a, int target) {
    int i = 0, j = a.length - 1;
    while (i <= j) {
        int m = (i + j) >>> 1;
        if (target < a[m]) {			// 在左边
            j = m - 1;
        } else if (a[m] < target) {		// 在右边
            i = m + 1;
        } else {
            return m;
        }
    }
    return -1;
}
```

* `int i = 0, j = a.length - 1` 各执行 1 次
* `i <= j` 比较 $floor(\log_{2}(n)+1)$ 再加 1 次
* `(i + j) >>> 1` 计算 $floor(\log_{2}(n)+1)$ 次
* 接下来 `if() else if() else` 会执行 $3* floor(\log_{2}(n)+1)$ 次，分别为
  * if 比较
  * else if 比较
  * else if 比较成立后的赋值语句

* `return -1`，执行一次

结果：

* 总执行时间为 $(2 + (1+3) + 3 + 3 * 3 +1)*t = 19t$
* 更一般地公式为 $(4 + 5 * floor(\log_{2}(n)+1))*t$

> **注意：**
>
> 左侧未找到和右侧未找到结果不一样，这里不做分析



两个算法比较，可以看到 $n$ 在较小的时候，二者花费的次数差不多

<img src="/imgs/image-20221108095747933.png" alt="image-20221108095747933" style="zoom:50%;" />

但随着 $n$ 越来越大，比如说 $n=1000$ 时，用二分查找算法（红色）也就是 $54t$，而蓝色算法则需要 $3003t$

<img src="/imgs/image-20221108100014451.png" alt="image-20221108100014451" style="zoom:50%;" />



> 画图采用的是 [Desmos | 图形计算器](https://www.desmos.com/calculator?lang=zh-CN)





计算机科学中，**时间复杂度**是用来衡量：一个算法的执行，随数据规模增大，而增长的时间成本

* 不依赖于环境因素



如何表示时间复杂度呢？

* 假设算法要处理的数据规模是 $n$，代码总的执行行数用函数 $f(n)$ 来表示，例如：
  * 线性查找算法的函数 $f(n) = 3*n + 3$
  * 二分查找算法的函数 $f(n) = (floor(log_2(n)) + 1) * 5 + 4$

* 为了对 $f(n)$ 进行化简，应当抓住主要矛盾，找到一个变化趋势与之相近的表示法



**大 $O$ 表示法[^4]**

![image-20221108103846566](/imgs/image-20221108103846566.png)

其中

* $c, c_1, c_2$ 都为一个常数
* $f(n)$ 是实际执行代码行数与 n 的函数
* $g(n)$ 是经过化简，变化趋势与 $f(n)$ 一致的 n 的函数



**渐进上界**

渐进上界（asymptotic upper bound）：从某个常数 $n_0$开始，$c*g(n)$ 总是位于 $f(n)$ 上方，那么记作 $O(g(n))$

* 代表算法执行的最差情况

例1

* $f(n) = 3*n+3$ 
* $g(n) = n$
* 取 $c=4$，在$n_0=3$ 之后，$g(n)$ 可以作为 $f(n)$ 的渐进上界，因此表示法写作 $O(n)$



例2

* $f(n) = 5*floor(log_2(n)) + 9$
* $g(n) = log_2(n)$
* $O(log_2(n))$



已知 $f(n)$ 来说，求 $g(n)$

* 表达式中相乘的常量，可以省略，如
  * $f(n) = 100*n^2$ 中的 $100$
* 多项式中数量规模更小（低次项）的表达式，如
  * $f(n)=n^2+n$ 中的 $n$
  * $f(n) = n^3 + n^2$ 中的 $n^2$
* 不同底数的对数，渐进上界可以用一个对数函数 $\log n$ 表示
  * 例如：$log_2(n)$ 可以替换为 $log_{10}(n)$，因为 $log_2(n) = \frac{log_{10}(n)}{log_{10}(2)}$，相乘的常量 $\frac{1}{log_{10}(2)}$ 可以省略
* 类似的，对数的常数次幂可省略
  * 如：$log(n^c) = c * log(n)$ 



**常见大 $O$ 表示法**

![image-20221108114915524](/imgs/image-20221108114915524.png)

 按时间复杂度从低到高

* 黑色横线 $O(1)$，常量时间，意味着算法时间并不随数据规模而变化
* 绿色 $O(log(n))$，对数时间
* 蓝色 $O(n)$，线性时间，算法时间与数据规模成正比
* 橙色 $O(n*log(n))$，拟线性时间
* 红色 $O(n^2)$ 平方时间
* 黑色朝上 $O(2^n)$ 指数时间
* 没画出来的 $O(n!)$



**渐进下界**

渐进下界（asymptotic lower bound）：从某个常数 $n_0$开始，$c*g(n)$ 总是位于 $f(n)$ 下方，那么记作 $\Omega(g(n))$



**渐进紧界**

渐进紧界（asymptotic tight bounds）：从某个常数 $n_0$开始，$f(n)$ 总是在 $c_1*g(n)$ 和 $c_2*g(n)$ 之间，那么记作 $\Theta(g(n))$



**空间复杂度**

与时间复杂度类似，一般也使用大 $O$ 表示法来衡量：一个算法执行随数据规模增大，而增长的**额外**空间成本

```java
public static int binarySearchBasic(int[] a, int target) {
    int i = 0, j = a.length - 1;    // 设置指针和初值
    while (i <= j) {                // i~j 范围内有东西
        int m = (i + j) >>> 1;
        if(target < a[m]) {         // 目标在左边
            j = m - 1;
        } else if (a[m] < target) { // 目标在右边
            i = m + 1;
        } else {                    // 找到了
            return m;
        }
    }
    return -1;
}
```



**二分查找性能**

下面分析二分查找算法的性能

时间复杂度

* 最坏情况：$O(\log n)$
* 最好情况：如果待查找元素恰好在数组中央，只需要循环一次 $O(1)$

空间复杂度

* 需要常数个指针 $i,j,m$，因此额外占用的空间是 $O(1)$ 



## 1.5 再看二分查找

### 1) 平衡版

```java
public static int binarySearchBalance(int[] a, int target) {
    int i = 0, j = a.length;
    while (1 < j - i) {
        int m = (i + j) >>> 1;
        if (target < a[m]) {
            j = m;
        } else {
            i = m;
        }
    }
    return (a[i] == target) ? i : -1;
}
```

思想：

1. 左闭右开的区间，$i$ 指向的可能是目标，而 $j$ 指向的不是目标
2. 不奢望循环内通过 $m$ 找出目标, 缩小区间直至剩 1 个, 剩下的这个可能就是要找的（通过 $i$）
   * $j - i > 1$ 的含义是，在范围内待比较的元素个数 > 1
3. 改变 $i$ 边界时，它指向的可能是目标，因此不能 $m+1$
4. 循环内的平均比较次数减少了
5. 时间复杂度 $\Theta(log(n))$



### 2) Java 版

```java
private static int binarySearch0(long[] a, int fromIndex, int toIndex,
                                     long key) {
    int low = fromIndex;
    int high = toIndex - 1;

    while (low <= high) {
        int mid = (low + high) >>> 1;
        long midVal = a[mid];

        if (midVal < key)
            low = mid + 1;
        else if (midVal > key)
            high = mid - 1;
        else
            return mid; // key found
    }
    return -(low + 1);  // key not found.
}
```

* 例如 $[1,3,5,6]$ 要插入 $2$ 那么就是找到一个位置，这个位置左侧元素都比它小
  * 等循环结束，若没找到，low 左侧元素肯定都比 target 小，因此 low 即插入点
* 插入点取负是为了与找到情况区分
* -1 是为了把索引 0 位置的插入点与找到的情况进行区分



### 3) Leftmost 与 Rightmost

有时我们希望返回的是最左侧的重复元素，如果用 Basic 二分查找

* 对于数组 $[1, 2, 3, 4, 4, 5, 6, 7]$，查找元素4，结果是索引3

* 对于数组 $[1, 2, 4, 4, 4, 5, 6, 7]$，查找元素4，结果也是索引3，并不是最左侧的元素

```java
public static int binarySearchLeftmost1(int[] a, int target) {
    int i = 0, j = a.length - 1;
    int candidate = -1;
    while (i <= j) {
        int m = (i + j) >>> 1;
        if (target < a[m]) {
            j = m - 1;
        } else if (a[m] < target) {
            i = m + 1;
        } else {
            candidate = m; // 记录候选位置
            j = m - 1;     // 继续向左
        }
    }
    return candidate;
}
```

如果希望返回的是最右侧元素

```java
public static int binarySearchRightmost1(int[] a, int target) {
    int i = 0, j = a.length - 1;
    int candidate = -1;
    while (i <= j) {
        int m = (i + j) >>> 1;
        if (target < a[m]) {
            j = m - 1;
        } else if (a[m] < target) {
            i = m + 1;
        } else {
            candidate = m; // 记录候选位置
            i = m + 1;	   // 继续向右
        }
    }
    return candidate;
}
```



**应用**

对于 Leftmost 与 Rightmost，可以返回一个比 -1 更有用的值

Leftmost 改为

```java
public static int binarySearchLeftmost(int[] a, int target) {
    int i = 0, j = a.length - 1;
    while (i <= j) {
        int m = (i + j) >>> 1;
        if (target <= a[m]) {
            j = m - 1;
        } else {
            i = m + 1;
        }
    }
    return i; 
}
```

* leftmost 返回值的另一层含义：$\lt target$ 的元素个数
* 小于等于中间值，都要向左找

Rightmost 改为

```java
public static int binarySearchRightmost(int[] a, int target) {
    int i = 0, j = a.length - 1;
    while (i <= j) {
        int m = (i + j) >>> 1;
        if (target < a[m]) {
            j = m - 1;
        } else {
            i = m + 1;
        }
    }
    return i - 1;
}
```

* 大于等于中间值，都要向右找

几个名词

![image-20221125174155058](/imgs/image-20221125174155058.png)

**范围查询**：

* 查询 $x \lt 4$，$0 .. leftmost(4) - 1$
* 查询 $x \leq 4$，$0 .. rightmost(4)$
* 查询 $4 \lt x$，$rightmost(4) + 1 .. \infty $
* 查询 $4 \leq x$， $leftmost(4) .. \infty$
* 查询 $4 \leq x \leq 7$，$leftmost(4) .. rightmost(7)$
* 查询 $4 \lt x \lt 7$，$rightmost(4)+1 .. leftmost(7)-1$

**求排名**：$leftmost(target) + 1$

* $target$ 可以不存在，如：$leftmost(5)+1 = 6$
* $target$ 也可以存在，如：$leftmost(4)+1 = 3$

**求前任（predecessor）**：$leftmost(target) - 1$

* $leftmost(3) - 1 = 1$，前任 $a_1 = 2$
* $leftmost(4) - 1 = 1$，前任 $a_1 = 2$

**求后任（successor）**：$rightmost(target)+1$

* $rightmost(5) + 1 = 5$，后任 $a_5 = 7$
* $rightmost(4) + 1 = 5$，后任 $a_5 = 7$

**求最近邻居**：

* 前任和后任距离更近者



## 习题

### 1) 时间复杂度估算

用函数 $f(n)$ 表示算法效率与数据规模的关系，假设每次解决问题需要 1 微秒（$10^{-6}$ 秒），进行估算：

1. 如果 $f(n) = n^2$ 那么 1 秒能解决多少次问题？1 天呢？
2. 如果 $f(n) = log_2(n)$  那么 1 秒能解决多少次问题？1 天呢？
3. 如果 $f(n) = n!$ 那么 1 秒能解决多少次问题？1 天呢？



**参考解答**

1. 1秒 $\sqrt{10^6} = 1000$ 次，1 天 $\sqrt{10^6 * 3600 * 24} \approx 293938$ 次
2. 1秒 $2^{1,000,000} $ 次，一天 $2^{86,400,000,000}$
3. 推算如下
   * $10! = 3,628,800$ 1秒能解决 $1,000,000$ 次，因此次数为 9 次
   * $14!=87,178,291,200$，一天能解决 $86,400,000,000$ 次，因此次数为 13 次



### 2) 耗时估算

一台机器对200个单词进行排序花了200秒(使用冒泡排序)，那么花费800秒，大概可以对多少个单词进行排序

a. 400

b. 600

c. 800

d. 1600

**答案**

* a

**解释**

* 冒泡排序时间复杂度是 $O(N^2)$
* 时间增长 4 倍，而因此能处理的数据量是原来的 $\sqrt{4} = 2$ 倍



### 3) E01. 二分查找-Leetcode 704

**要点**：减而治之，可以用递归或非递归实现

给定一个 n 个元素有序的（升序）整型数组 nums 和一个目标值 target  ，写一个函数搜索 nums 中的 target，如果目标值存在返回下标，否则返回 -1

例如

```java
输入: nums = [-1,0,3,5,9,12], target = 9
输出: 4
解释: 9 出现在 nums 中并且下标为 4
    
输入: nums = [-1,0,3,5,9,12], target = 2
输出: -1
解释: 2 不存在 nums 中因此返回 -1    
```

**参考答案**：略，可以用讲过的任意一种二分求解



### 4) E02. 搜索插入位置-Leetcode 35

**要点**：理解谁代表插入位置

给定一个排序数组和一个目标值

* 在数组中找到目标值，并返回其索引
* 如果目标值不存在于数组中，返回它将会被按顺序插入的位置

例如

```
输入: nums = [1,3,5,6], target = 5
输出: 2

输入: nums = [1,3,5,6], target = 2
输出: 1

输入: nums = [1,3,5,6], target = 7
输出: 4
```



**参考答案1**：用二分查找基础版代码改写，基础版中，找到返回 m，没找到 i 代表插入点，因此有

```java
public int searchInsert(int[] a, int target) {
    int i = 0, j = a.length - 1;
    while (i <= j) {
        int m = (i + j) >>> 1;
        if (target < a[m]) {
            j = m - 1;
        } else if (a[m] < target) {
            i = m + 1;
        } else {
            return m;
        }
    }
    return i; // 原始 return -1
}
```



**参考答案2**：用二分查找平衡版改写，平衡版中

* 如果 target == a[i] 返回 i 表示找到
* 如果 target < a[i]，例如 target = 2，a[i] = 3，这时就应该在 i 位置插入 2
* 如果 a[i] < target，例如 a[i] = 3，target = 4，这时就应该在 i+1 位置插入 4

```java
public static int searchInsert(int[] a, int target) {
    int i = 0, j = a.length;
    while (1 < j - i) {
        int m = (i + j) >>> 1;
        if (target < a[m]) {
            j = m;
        } else {
            i = m;
        }
    }
    return (target <= a[i]) ? i : i + 1;
    // 原始 (target == a[i]) ? i : -1;
}
```



**参考答案3**：用 leftmost 版本解，返回值即为插入位置（并能处理元素重复的情况）

```java
public int searchInsert(int[] a, int target) {
    int i = 0, j = a.length - 1;
    while(i <= j) {
        int m = (i + j) >>> 1;
        if(target <= a[m]) {
            j = m - 1;
        } else {
            i = m + 1;
        } 
    }
    return i;
}
```



### 5) E03. 搜索开始结束位置-Leetcode 34

给你一个按照非递减顺序排列的整数数组 nums，和一个目标值 target。请你找出给定目标值在数组中的开始位置和结束位置。

如果数组中不存在目标值 target，返回 [-1, -1]。

你必须设计并实现时间复杂度为 O(log n) 的算法解决此问题



例如

```
输入：nums = [5,7,7,8,8,10], target = 8
输出：[3,4]

输入：nums = [5,7,7,8,8,10], target = 6
输出：[-1,-1]

输入：nums = [], target = 0
输出：[-1,-1]
```



**参考答案**

```java
public static int left(int[] a, int target) {
    int i = 0, j = a.length - 1;
    int candidate = -1;
    while (i <= j) {
        int m = (i + j) >>> 1;
        if (target < a[m]) {
            j = m - 1;
        } else if (a[m] < target) {
            i = m + 1;
        } else {
            candidate = m;
            j = m - 1;
        }
    }
    return candidate;
}

public static int right(int[] a, int target) {
    int i = 0, j = a.length - 1;
    int candidate = -1;
    while (i <= j) {
        int m = (i + j) >>> 1;
        if (target < a[m]) {
            j = m - 1;
        } else if (a[m] < target) {
            i = m + 1;
        } else {
            candidate = m;
            i = m + 1;
        }
    }
    return candidate;
}

public static int[] searchRange(int[] nums, int target) {
    int x = left(nums, target);
    if(x == -1) {
        return new int[] {-1, -1};
    } else {
        return new int[] {x, right(nums, target)};
    }
}
```
# 参考文章

[^1]: ["Definition of ALGORITHM"](https://www.merriam-webster.com/dictionary/algorithm). *Merriam-Webster Online Dictionary*. [Archived](https://web.archive.org/web/20200214074446/https://www.merriam-webster.com/dictionary/algorithm) from the original on February 14, 2020. Retrieved November 14, 2019.
[^2]: Introduction to Algorithm 中文译作《算法导论》
[^3]: 主要参考文档 https://en.wikipedia.org/wiki/Binary_search_algorithm
[^4]: 图片及概念均摘自 Introduction to Algorithm 4th，3.1节，3.2 节