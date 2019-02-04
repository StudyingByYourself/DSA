# 链表

35\. Reverse Linked List

36\. Reverse Linked List II

??? 解析
	为了实现在某一段[m, n]上翻转顺序，首先设置虚拟头结点，找到m的前一个结点preNode，

	curNode = preNode.next  获取当前结点

	postNode = curNode.next 负责移动

	例如  3->1->4->2->6->7->9->3 在[3,5]上翻转，

	![](https://s2.ax1x.com/2019/01/29/kM5yz6.png)

	```java
	ListNode dummy = new ListNode(0);
	dummy.next = head;
	head = dummy;   //在head前面接了一个虚拟头结点
	for (int i = 1; i < m; i++) {
		head = head.next;
	}
	ListNode preNode = head;
	ListNode curNode = preNode.next;
	ListNode postNode = curNode.next;
	for (int i = 0; i < n - m; i++) {
		curNode.next = postNode.next;
		postNode.next = preNode.next;
		preNode.next = postNode;
		postNode = curNode.next;
	}
	```


112\. Remove Duplicates from Sorted List

96\. Partition List

1292\. Odd Even Linked List

方法一： 借鉴221题的思路，利用Queue的FIFO原理将每个数字分别存储到队列中，再将队列中的数据取出转化成链表

方法二：开始时若 head,head.next,head.next.next 都为空时则直接返回head，再后续判断中cur,cur.next,cur.next.next都为空时则不执行下去

167\. Add Two Numbers

先分别计算出两个数，再计算其和，然后再拆分成链表的方法不可行，因为其和会超出int类型的范围

221\. Add Two Numbers II

利用Stack的FILO原理将每个数字存储到一个栈中，再将栈中的数据取出转化成链表

452\. Remove Linked List Elements

113\. Remove Duplicates from Sorted List II 

165\. Merge Two Sorted Lists 

451\. Swap Nodes in Pairs

450\. Reverse Nodes in k-Group    借鉴36题的思想

Insertion Sort List
Sort List

372\. Delete Node in a Linked List

174\. Remove Nth Node From End of List

方法一：两次遍历，第一次遍历计算链表的长度，第二次删除指定的位置

方法二：利用双指针，p和q两个的位置始终确定在n + 1的长度

170\. Rotate List

99\. Reorder List

223\. Palindrome Linked List


