# 数据结构
1.哈希（Hash）
哈希是干嘛用的?看下面的伪代码，理解哈希的定义：

```
a <- {
  '0':0,
  '1':1,
  'key':'value',
  '键':'值'
}
```

一个 `key` 对应一个 `value`。所有满足一个键、一个值的这种结构。比如 http 请求的第二部分和 http 响应的第二部分都是 key：value。注意：不一定要用冒号，或逗号，只要是这种形式，都叫哈希。
了解了哈希的概念，那哈希怎么用呢？

伪代码：

```
// 声明数组 a 

a <- {
  '0':0,
  '1':2,
  '2':1,
  '3':56,
  '4':3,
  '5':67,
  '6':3,
  'length':7
}

// 声明一个 hash,记录每一个数字出现多少次

hash <- {

}

// 遍历 数组 a
// 把 a 里面的每一个数字都读到
// 根据 a[index] 取到的值，放到 hash 对应的桶里面，hash 有无数个桶

index <- 0
while( index < a['length'] )
  // 当前的数字，取值范围：0，2，1，56，3，67，3
  number = a[index]
  if hash[number] == undefined  // hash[number] 不存在
    hash[number] = 1
  else    // hash[number] 存在
    hash[number] = hash[number] + 1
  end
    index <- index + 1
end

```
遍历数组 a，每一步遍历过程，这个过程称为 `入桶`，伪代码如下：
```
// 遍历的数字，取值范围：0，2，1，56，3，67，3

hash <- {}

// 第一轮
index == 0
number = 0
hash <- { 
'0':1 
}

// 第二轮
index == 1
number = 2
hash <- { 
'0':1 ,
'2':1
}

// 第三轮
index == 2
number = 1
hash <- { 
'0':1 ,
'1':1,
'2':1
}

// 第四轮
index == 3
number = 56
hash <- { 
'0':1 ,
'1':1,
'2':1,
'56':1
}

// 第五轮
index == 4
number = 3
hash <- { 
'0':1 ,
'1':1,
'2':1,
'56':1,
'3':1
}

// 第六轮
index == 5
number = 67
hash <- { 
'0':1 ,
'1':1,
'2':1,
'56':1,
'3':1,
'67':1
}

// 第七轮
index == 6
number = 3
hash <- { 
'0':1 ,
'1':1,
'2':1,
'3':2,
'56':1,
'67':1
}

```
接下来，遍历我们的 hash，伪代码如下：
```
// hash 有多少个值？
// hash 值多少个，跟数组 a 里面最大的值有关
// 思路：找出 a 里面最大的值

index2 <- 0
max <- findMax(a)  // findMax 函数找出数组 a 里面最大的值
newArr <- {}    // 声明一个新数组
while( index2 <= max )   // index2 == max(67)是可以出现的，index2 < max + 1
  count = hash[index2]  // 取到值的个数
  if count == 1
    newArr.push(index2)
  else if count == 2
    newArr.push(index2)
    newArr.push(index2)
  else if count == 3
    newArr.push(index2)
    newArr.push(index2)
    newArr.push(index2)
  end
    index2 <- index2 + 1
  end
  print newArr
```
上面的代码优化后，如下所示：

```
index2 <- 0
max <- findMax(a)  // findMax 函数找出数组 a 里面最大的值
newArr <- {}    // 声明一个新数组
while( index2 <= max )   // index2 == max(67)是可以出现的，index2 < max + 1
  count = hash[index2]  // 取到值的个数
  if count != undefined
    countIndex = 0;
    while( countIndex < count )
      newArr.push(index2)
      countIndex <- countIndex + 1
    end
  end
    index2 <- index2 + 1
  end
  print newArr
```

分析上面伪代码，这个过程叫做 `出桶`，如下所示：

```
hash <- { 
'0':1 ,
'1':1,
'2':1,
'3':2,
'56':1,
'67':1
}

// 出桶
// 让 i 从 0 增长到 67

index2 = 0
newArr = [0]

index2 = 1
newArr = [0,1]

index2 = 2
newArr = [0,1,2]

index2 = 3
newArr = [0,1,2,3,3]

index2 = 4
什么也不做

index2 = 5
什么也不做

...
...

index2 = 56
newArr = [0,1,2,3,3,56]

...
...

index2 = 67
newArr = [0,1,2,3,3,56,67]

```
- 计数排序中的桶（复杂度 O(n + max)，比快排还快）
  比较排序的复杂度是：NLogN
  计数排序的优缺点
    优点：
     它很快
    缺点：
     1. 必须要有一个 hash 作为计数的工具
     2. 无法对小数和负数进行排序
     
- [桶排序](http://bubkoo.com/2014/01/15/sort-algorithm/bucket-sort/)与计数排序的区别
  计数排序是最简单的，因为它不需要做二次排序，但是它浪费了桶
  桶排序：节约桶，浪费 CPU
  排序的一个原则：要么浪费空间，要么浪费时间
- [基数排序](http://bubkoo.com/2014/01/15/sort-algorithm/radix-sort/)与计数排序的区别
  基数排序：基数是几进制，可以适应特别大大数字，桶的个数是固定的
  
2.队列（Queue）
- 先进先出
- 可以用数组实现
- 举例：排队

代码实现如下：
```
// 声明一个队列 q
var q = [];

// 入队
q.push('张三'); 
q.push('李四');
q.push('王二');

// 出队
q.shift();  // 张三
q.shift();  // 李四
q.shift();  // 王二

```

3.栈（stack）
- 先进后出
- 可以用数组实现
- 举例：盗梦空间

代码实现如下：
```
var stack = [];

// 入栈
stack.push('第一层梦');
stack.push('第二层梦');
stack.push('第三层梦');

// 出栈
stack.pop();  // 第三层梦
stack.pop();  // 第二层梦
stack.pop();  // 第一层梦
```
注意：数组是哈希

4.链表（Linked List）
- 数组无法直接删除中间的一项，链表可以
- 用哈希（JS里面用对象表示哈希）实现链表
- head、node 概念
  head：第一个 hash 对象，链表的表头，只要知道表头就可以知道后面的所有项。
  node：节点

链表与数组的区别？
数组的优缺点：在数组里删除一项特别复杂，如果你取到数组的第 n 项很简单，array[n-1]
链表的优缺点：你可以随意的删除一项。但如果你想取到链表的第 n 项，还挺复杂的

所以，如果不涉及经常删除，或者只删头尾，就用数组。如果经常从中间删除一个数据，就用链表。

代码实现：
```
var a = {
  value:0,
  next:{
    value:2,
    next:{
      value:1,
      next:undefined
    }
  }
}

// 删除第二项
a.next = a.next.next

```
![](https://github.com/plbowter/images/blob/master/lianbiao.png)


5.树（tree）

只要你有层级结构，就要用到树。
 


- 举例：层级结构、DOM
- 概念：层数、深度、节点个数
  层数：树的层级，上面层是小的，下面层是大的
  深度：一共有多少层
  节点个数：每一个 hash 就是一个节点。没有子节点的节点称作叶子节点。
  
- [二叉树](https://zh.wikipedia.org/wiki/%E4%BA%8C%E5%8F%89%E6%A0%91)
 概念：每个节点最多只有两个分支（不存在分支度大于2的节点）的树结构。通常分支被称作“左子树”和“右子树”。二叉树的分支具有左右次序，不能颠倒。
 二叉树的第 i 层至多拥有 2^{i-1} 个节点数
 
 推导过程如下：
 
 ```
 前提：二叉树
 层数(i)  那一层最多有几个节点  所有层节点数              对应数组中的位置 
 0        1        2^0       1        (2^1-1)       0
 1        2        2^1       3        (2^2-1)       1~2
 2        4        2^2       7        (2^3-1)       3~6
 3        8        2^3       15       (2^4-1)       7~14
 4        16       2^4       31       (2^5-1)       15~31
 5        32       2^5       63       (2^6-1)       32~63
 ```
- 满二叉树
  概念：每一层上的节点数都是最大节点数
- 完全二叉树
- 完全二叉树和满二叉树可以用数组实现
- 其他树可以用哈希（对象）实现
- 操作：增删改查
- [堆排序](http://bubkoo.com/2014/01/14/sort-algorithm/heap-sort/)用到了 tree
  概念：堆（二叉堆）可以视为一棵完全二叉树，完全二叉树的一个“优秀”的性质是，除了最底层外，每一层都是满的，这使得堆可以用数组来表示（普通的一般二叉树通常用链表作为基本容器表示），每一个结点对应数组中的一个元素
  [堆排序演示](https://www.cs.usfca.edu/~galles/visualization/HeapSort.html)
- 其他：[B树](https://zh.wikipedia.org/wiki/B%E6%A0%91)、[红黑树](https://zh.wikipedia.org/wiki/%E7%BA%A2%E9%BB%91%E6%A0%91)、[AVL树](https://zh.wikipedia.org/wiki/AVL%E6%A0%91)

 







