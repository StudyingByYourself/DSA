# 线性数据结构

##Array(动态数组)

类比Java中的ArrayList,但ArrayList的扩容是乘以1.5倍

| method | time complexity 			| 
| :-- 	 | :--: 		   			| 
| add			|		O(n)   		|
| addFirst		|		O(n)   		|
| addLast		|		O(1)        |
| remove		|		O(n)        |
| removeFirst	|		O(n)        |
| removeLast	|		O(1)        |
| set			|		O(1)      	|
| get			|		O(1)    	|
| getFirst		|		O(1)   		|
| getLast		|		O(1)   		|
| resize  		|     	O(1)均摊    |

## LinkedList(含有虚拟头结点)

类比Java中的LinkedList, 但Java中的LinkedList采用了双指针

| method | time complexity 		| 
| :-- 	 | :--: 		   		| 
| add:			|	O(n)        |
| addFirst:		|	O(1)        |
| addLast:		|	O(n)        |
| remove:		|	O(n)        |
| removeFirst:	|	O(1)        |
| removeLast:	|	O(n)        |
| set:			|	O(n)        |
| get:			|	O(n)        |
| getFirst:		|	O(1)        |
| getLast:		|	O(n)        |
| resize:       |  	O(1)均摊    |

## Stack

对于Stack而言，无论是用动态数组实现的栈还是用链表实现的栈，都可以做到入栈和出栈的时间复杂度为O(1)

看Java的源代码发现，Java中的Stack是基于Vector动态数组实现的，Vector和ArrayList的区别在于Vector对元素的访问都用synchronized(同步)修饰，是线程安全的，Vector,ArrayList和LinkedLis共同实现了List集合接口。

## Queue

对于Queue而言，动态数组实现的队列在入队上的时间复杂度为O(1),但在出队的时间复杂度为O(n),这时可以考虑用基于动态数组的循环队列LoopQueue+双指针(front,tail，其中tail指针停留在下一个需要入队的位置)实现入队和出队的时间复杂度都为O(1),当front = tail时表示队列为空，当tail + 1 = front时表示队列满了，为此需要额外留空一个空间；

链表实现的队列在入队上的时间复杂度为O(n),在出队上的时间复杂度为O(1),这时考虑修改链表，加入双指针(head,tail),可以实现入队和出队的时间复杂度都达到O(1)

看Java的源代码发现，Java中的Queue是基于双指针的LinkedList实现的。
