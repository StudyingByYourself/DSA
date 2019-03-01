# 线性数据结构

##Array(动态数组)

??? java实现
	```java
	public class Array<E> {
		private E[] data;
		private int size;

		@SuppressWarnings("unchecked")
		public Array(int capacity) {
			data = (E[]) new Object[capacity];
			size = 0;
		}

		public Array() {
			this(10);
		}

		@SuppressWarnings("unchecked")
		public Array(E[] arr) {
			data = (E[]) new Object[arr.length];
			for (int i = 0; i < arr.length; i++) {
				data[i] = arr[i];
			}
			size = arr.length;
		}

		public int getSize() {
			return size;
		}

		public int getCapacity() {
			return data.length;
		}

		public boolean isEmpty() {
			return size == 0;
		}

		public void add(int index, E e) {
			if (index < 0 || index > size) {
				throw new IllegalArgumentException("Add failed.Illegal index.");
			}
			if (size == data.length) {
				resize(2 * data.length);
			}
			if (index < size) {
				for (int i = size - 1; i >= index; i--) {
					data[i + 1] = data[i];
				}
			}
			data[index] = e;
			size++;
		}

		public void addFirst(E e) {
			add(0, e);
		}

		public void addLast(E e) {
			add(size, e);
		}

		public E get(int index) {
			if (index < 0 || index >= size) {
				throw new IllegalArgumentException("Get failed.Illegal index.");
			}
			return data[index];
		}

		public E getFirst() {
			return get(0);
		}

		public E getLast() {
			return get(size - 1);
		}
		
		public void set(int index, E e) {
			if (index < 0 || index >= size) {
				throw new IllegalArgumentException("Set failed.Illegal index.");
			}
			data[index] = e;
		}
		
		public boolean contains(E e) {
			for (int i = 0; i < size; i++) {
				if (data[i].equals(e)) {
					return true;
				}
			}
			return false;
		}
		
		public int find(E e) {
			for (int i = 0; i < size; i++) {
				if (data[i].equals(e)) {
					return i;
				}
			}
			return -1;
		}
		
		public E remove(int index) {
			if (index < 0 || index >= size) {
				throw new IllegalArgumentException("Remove failed.Illegal index.");
			}
			E ret = data[index];
			for (int i = index + 1; i < size; i++) {
				data[i - 1] = data[i];
			}
			data[--size] = null;
			if (size == data.length / 4 && data.length / 2 != 0) {
				resize(data.length / 2);
			}
			return ret;
		}
		
		public E removeFirst() {
			return remove(0);
		}
		
		public E removeLast() {
			return remove(size - 1);
		}
		
		public void resize(int newCapacity) {
			@SuppressWarnings("unchecked")
			E[] newData = (E[]) new Object[newCapacity];
			for (int i = 0; i < size; i++) {
				newData[i] = data[i];
			}
			data = newData;
		}
		
		public String toString() {
			StringBuilder sb = new StringBuilder();
			sb.append(String.format("Array: size = %d, capacity = %d\n", size, data.length));
			sb.append("[");
			for (int i = 0; i < size; i++) {
				sb.append(data[i]);
				if (i != size - 1) {
					sb.append(", ");
				}
			}
			sb.append("]");
			return sb.toString();
		}
	}	
	```

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

## LinkedList

??? note "java实现(含有虚拟头结点)"
	```java
	public class LinkedList<E> {
		private class Node {
			private E e;
			private Node next;

			public Node(E e, Node next) {
				this.e = e;
				this.next = next;
			}

			public String toString() {
				return e.toString();
			}
		}

		private Node dummyHead;
		private int size;

		public LinkedList() {
			dummyHead = new Node(null, null);
			size = 0;
		}

		public int getSize() {
			return size;
		}

		public boolean isEmpty() {
			return size == 0;
		}

		public void add(int index, E e) {
			if (index < 0 || index > size) {
				throw new IllegalArgumentException("index is illegal");
			}
			Node prev = dummyHead;
			for (int i = 0; i < index; i++) {
				prev = prev.next;
			}
			prev.next = new Node(e, prev.next);
			size++;
		}

		public void addFirst(E e) {
			add(0, e);
		}

		public void addLast(E e) {
			add(size, e);
		}

		public E get(int index) {
			if (index < 0 || index >= size) {
				throw new IllegalArgumentException("index is illegal");
			}
			Node cur = dummyHead.next;
			for (int i = 0; i < index; i++) {
				cur = cur.next;
			}
			return cur.e;
		}

		public E getFirst() {
			return get(0);
		}

		public E getLast() {
			return get(size - 1);
		}

		public void set(int index, E e) {
			if (index < 0 || index >= size) {
				throw new IllegalArgumentException("index is illegal");
			}
			Node cur = dummyHead.next;
			for (int i = 0; i < index; i++) {
				cur = cur.next;
			}
			cur.e = e;
		}

		public boolean contains(E e) {
			Node cur = dummyHead.next;
			while (cur != null) {
				if (cur.e.equals(e)) {
					return true;
				}
				cur = cur.next;
			}
			return false;
		}

		public E remove(int index) {
			if (index < 0 || index >= size) {
				throw new IllegalArgumentException("index is illegal");
			}
			Node prev = dummyHead;
			for (int i = 0; i < index; i++) {
				prev = prev.next;
			}
			Node delNode = prev.next;
			prev.next = delNode.next;
			delNode.next = null;
			size--;
			return delNode.e;
		}

		public E removeFirst() {
			return remove(0);
		}

		public E removeLast() {
			return remove(size - 1);
		}
		
		public String toString() {
			StringBuilder ret = new StringBuilder();
			Node cur = dummyHead.next;
			while(cur != null) {
				ret.append(cur.e);
				ret.append("-->");
				cur = cur.next;
			}
			ret.append("NULL");
			return ret.toString();
		}
	}
	```


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

??? Queue接口
	```java
	public interface Queue<E> {
		int getSize();

		boolean isEmpty();

		void enqueue(E e);

		E dequeue();

		E getFront();
	}
	```

对于Queue而言，动态数组实现的队列在入队上的时间复杂度为O(1),但在出队的时间复杂度为O(n),这时可以考虑用基于动态数组的循环队列LoopQueue+双指针(front,tail，其中tail指针停留在下一个需要入队的位置)实现入队和出队的时间复杂度都为O(1),当front = tail时表示队列为空，当tail + 1 = front时表示队列满了，为此需要额外留空一个空间；

??? note "LoopQueue"
	```java
	public class LoopQueue<E> implements Queue<E> {
		private E[] data;
		private int front;
		private int tail;
		private int size;

		@SuppressWarnings("unchecked")
		public LoopQueue(int capacity) {
			data = (E[]) new Object[capacity + 1];
			front = 0;
			tail = 0;
			size = 0;
		}

		public LoopQueue() {
			this(10);
		}

		public int getCapacity() {
			return data.length - 1;
		}

		public int getSize() {
			return size;
		}

		public boolean isEmpty() {
			return front == tail;
		}

		public void enqueue(E e) {
			if ((tail + 1) % data.length == front) {
				resize(getCapacity() * 2);
			}
			data[tail] = e;
			tail = (tail + 1) % data.length;
			size++;
		}

		@SuppressWarnings("unchecked")
		private void resize(int newCapacity) {
			E[] newData = (E[]) new Object[newCapacity + 1];
			for (int i = 0; i < size; i++) {
				newData[i] = data[(i + front) % data.length];
			}
			data = newData;
			front = 0;
			tail = size;
		}

		public E dequeue() {
			if (isEmpty()) {
				throw new IllegalArgumentException("Can not pop from an empty Queue");
			}
			E ret = data[front];
			data[front] = null;
			front = (front + 1) % data.length;
			size--;
			if (size == getCapacity() / 4 && getCapacity() / 2 != 0) {
				resize(getCapacity() / 2);
			}
			return ret;
		}

		public E getFront() {
			if (isEmpty()) {
				throw new IllegalArgumentException("Can not peek from an empty Queue");
			}
			return data[front];
		}
		
		public String toString() {
			StringBuilder ret = new StringBuilder();
			ret.append("front [");
			for (int i = front; i != tail; i = (i + 1) % data.length) {
				ret.append(data[i]);
				if ((i + 1) % data.length != tail) {
					ret.append(", ");
				}
			}
			ret.append("] tail");
			return ret.toString();
		}

	}
	```

链表实现的队列在入队上的时间复杂度为O(n),在出队上的时间复杂度为O(1),这时考虑修改链表，加入双指针(head,tail),可以实现入队和出队的时间复杂度都达到O(1)

??? note "LinkedList(双指针)"
	```java
	public class LinkedList<E> {
		private class Node {
			private E e;
			private Node next;

			public Node(E e, Node next) {
				this.e = e;
				this.next = next;
			}

			public String toString() {
				return e.toString();
			}
		}

		private Node head;
		private Node tail;
		private int size;

		public LinkedList() {
			head = null;
			tail = null;
			size = 0;
		}

		public int getSize() {
			return size;
		}

		public boolean isEmpty() {
			return size == 0;
		}

		public void add(int index, E e) {
			if (index < 0 || index > size) {
				throw new IllegalArgumentException("index is illegal");
			}
			if (size == 0) {
				head = tail = new Node(e, null);
				size++;
				return;
			}
			if (index == 0) {
				Node cur = head;
				head = new Node(e, cur);
			} else if (index == size) {
				tail.next = new Node(e, null);
				tail = tail.next;
			} else {
				Node cur = head;
				for (int i = 0; i < index - 1; i++) {
					cur = cur.next;
				}
				cur.next = new Node(e, cur.next);
			}
			size++;
		}

		public void addFirst(E e) {
			add(0, e);
		}

		public void addLast(E e) {
			add(size, e);
		}

		public E get(int index) {
			if (index < 0 || index >= size) {
				throw new IllegalArgumentException("index is illegal");
			}
			Node cur = head;
			for (int i = 0; i < index; i++) {
				cur = cur.next;
			}
			return cur.e;
		}

		public E getFirst() {
			return get(0);
		}

		public E getLast() {
			return get(size - 1);
		}

		public void set(int index, E e) {
			if (index < 0 || index >= size) {
				throw new IllegalArgumentException("index is illegal");
			}
			Node cur = head;
			for (int i = 0; i < index; i++) {
				cur = cur.next;
			}
			cur.e = e;
		}

		public boolean contains(E e) {
			Node cur = head;
			while (cur != null) {
				if (cur.e.equals(e)) {
					return true;
				}
				cur = cur.next;
			}
			return false;
		}

		public E remove(int index) {
			if (index < 0 || index >= size) {
				throw new IllegalArgumentException("index is illegal");
			}
			E ret = get(index);
			if (size == 1) {
				head = tail = null;
				size--;
				return ret;
			}
			if (index == 0) {
				head = head.next;
			} else if (index == size - 1) {
				Node cur = head;
				for (int i = 0; i < index - 1; i++) {
					cur = cur.next;
				}
				tail = cur;
				tail.next = null;
			} else {
				Node cur = head;
				for (int i = 0; i < index - 1; i++) {
					cur = cur.next;
				}
				Node delNode = cur.next;
				cur.next = delNode.next;
				delNode.next = null;
			}
			size--;
			return ret;
		}

		public E removeFirst() {
			return remove(0);
		}

		public E removeLast() {
			return remove(size - 1);
		}
		
		public String toString() {
			StringBuilder ret = new StringBuilder();
			Node cur = head;
			while(cur != null) {
				ret.append(cur.e);
				ret.append("-->");
				cur = cur.next;
			}
			ret.append("NULL");
			return ret.toString();
		}
	}
	```

??? note "LinkedListQueue"
	```java
	public class LinkedListQueue<E> implements Queue<E> {
		
		private LinkedList<E> list;
		
		public LinkedListQueue() {
			list = new LinkedList<>();
		}
		
		public int getSize() {
			return list.getSize();
		}

		public boolean isEmpty() {
			return list.isEmpty();
		}

		public void enqueue(E e) {
			list.addLast(e);
		}

		public E dequeue() {
			return list.removeFirst();
		}

		public E getFront() {
			return list.getFirst();
		}
		
		public String toString() {
			StringBuilder ret = new StringBuilder();
			ret.append("top front [");
			ret.append(list);
			ret.append("] tail");
			return ret.toString();
		}
	}
	```

看Java的源代码发现，Java中的Queue是基于双指针的LinkedList实现的。

## PriorityQueue

??? java 
	```java
	public class PriorityQueue<E extends Comparable<E>> implements Queue<E> {
		private MaxHeap<E> maxHeap;
		
		public PriorityQueue() {
			maxHeap = new MaxHeap<>();
		}
		
		public int getSize() {
			return maxHeap.size();
		}

		public boolean isEmpty() {
			return maxHeap.isEmpty();
		}

		public void enqueue(E e) {
			maxHeap.add(e);
		}

		public E dequeue() {
			return maxHeap.extractMax();
		}

		public E getFront() {
			return maxHeap.findMax();
		}
	}
	```

看Java的源代码发现，Java中的PriorityQueue是基于最小堆实现的。