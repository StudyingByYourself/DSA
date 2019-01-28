# 链表

35\. Reverse Linked List

36\. Reverse Linked List II

??? 解析
	为了实现在某一段[m, n]上翻转顺序，首先设置虚拟头结点，找到m的前一个结点preNode，

	curNode = preNode.next  获取当前结点

	postNode = curNode.next 负责移动

	例如  3->1->4->2->6->7->9->3 在[3,5]上翻转，

	![](https://github.com/StudyingByYourself/DSA/blob/master/docs/img/LinkedListChangePosition.png)

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

