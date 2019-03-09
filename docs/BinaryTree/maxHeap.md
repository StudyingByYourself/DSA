# maxHeap

## 二叉堆(借助完全二叉树)

二叉堆的特点

1．父结点的值总是大于或等于（小于或等于）任何一个子结点的值。

2．每个结点的左子树和右子树都是一个二叉堆（都是最大堆或最小堆）。

## 最大堆实现

最大堆中父结点与左孩子结点和右孩子结点计算，可以借助数组实现最大堆
<center>
parent(i) = (i - 1) / 2

leftChild(i) = 2 * i + 1

rightChild(i) = 2 * i + 2
</center>

## shitUp与shitDown操作

shitUp(上浮):不断与父结点比较，当父结点小于该结点时，交换位置

shitDown(下沉):不断与左孩子结点与右孩子结点中的较大值比较，当该结点小于孩子结点时，交换位置

## heapify操作(时间复杂度O(n))

将任意数组整理成堆的形状

??? java
	从最后一个非叶子结点开始执行shitDown()操作，并不断向前遍历到堆顶
	```java
	public MaxHeap(E[] arr) {
		data = arr;
		size = arr.length;
		for (int i = parent(arr.length - 1); i >= 0; i--) {
			shitDown(i);
		}
	}
	```
## 整体代码实现

??? java版本
	```java
	public class MaxHeap<E extends Comparable<E>> {

		private E[] data;
		private int size;

		@SuppressWarnings("unchecked")
		public MaxHeap(int capacity) {
			data = (E[]) new Comparable[capacity];
			size = 0;
		}

		public MaxHeap() {
			this(10);
		}

		public MaxHeap(E[] arr) {
			data = arr;
			size = arr.length;
			for (int i = parent(arr.length - 1); i >= 0; i--) {
				shitDown(i);
			}
		}

		public int size() {
			return size;
		}

		public int getCapacity() {
			return data.length;
		}

		public boolean isEmpty() {
			return size == 0;
		}

		private int parent(int index) {
			if (index == 0) {
				throw new IllegalArgumentException("index-0 doesn't have parent.");
			}
			return (index - 1) / 2;
		}

		private int leftChild(int index) {
			return index * 2 + 1;
		}

		private int rightChild(int index) {
			return index * 2 + 2;
		}

		public void add(E e) {
			if (size == data.length) {
				resize(data.length * 2);
			}
			data[size] = e;
			size++;
			shitUp(size() - 1);
		}

		public E findMax() {
			if (size == 0) {
				throw new IllegalArgumentException("MaxHeap is empty");
			}
			return data[0];
		}

		public E extractMax() {
			E ret = findMax();
			swap(0, size - 1);
			data[size - 1] = null;
			size--;
			if (size == data.length / 4 && data.length / 2 != 0) {
				resize(data.length / 2);
			}
			shitDown(0);
			return ret;
		}

		private void shitUp(int index) {
			while (index > 0 && data[parent(index)].compareTo(data[index]) < 0) {
				swap(index, parent(index));
				index = parent(index);
			}
		}

		private void shitDown(int index) {
			while (leftChild(index) < size) {
				int j = leftChild(index);
				if (j + 1 < size && data[j + 1].compareTo(data[j]) > 0) {
					j = rightChild(index);
				}
				if (data[index].compareTo(data[j]) >= 0) {
					break;
				}
				swap(index, j);
				index = j;
			}
		}

		@SuppressWarnings("unchecked")
		private void resize(int newCapacity) {
			E[] newData = (E[]) new Comparable[newCapacity];
			for (int i = 0; i < size; i++) {
				newData[i] = data[i];
			}
			data = newData;
		}

		private void swap(int i, int j) {
			if (i < 0 || i >= size || j < 0 || j >= size) {
				throw new IllegalArgumentException("Index is illegal");
			}
			E temp = data[i];
			data[i] = data[j];
			data[j] = temp;
		}
	}
	```

