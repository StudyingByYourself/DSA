# AVL树

## 1 AVL树的左旋与右旋

### 1.1 AVL树的左旋与右旋的规则

![在这里插入图片描述](https://s2.ax1x.com/2019/01/21/kiHpY4.jpg)

### 1.2 AVL树的左旋与右旋的实例

备注：在左旋和右旋时需要重新计算height值

![在这里插入图片描述](https://s2.ax1x.com/2019/01/21/kibdUO.md.png)

??? 左旋与右旋
	```java
	// 右旋
	//        y                            x
	//       / \                        /     \
	//      x   T4     向右旋转(y)     z       y
	//     / \       - - - - - - ->   / \     / \
	//    z   T3                    T1   T2 T3   T4
	//   / \
	// T1 T2
	private Node rightRotate(Node y) {
		Node x = y.left;
		Node T3 = x.right;
		x.right = y;
		y.left = T3;
		y.height = 1 + Math.max(getHeight(y.left), getHeight(y.right));
		x.height = 1 + Math.max(getHeight(x.left), getHeight(x.right));
		return x;
	}

	// 左旋
	//    y                                   x
	//   / \                               /     \
	// T1   x           向左旋转(y)       y       z
	//     / \          - - - - - - ->   / \     / \
	//   T2   z                        T1   T2 T3   T4
	//       / \
	//     T3   T4	
	private Node leftRotate(Node y) {
		Node x = y.right;
		Node T2 = x.left;
		x.left = y;
		y.right = T2;
		y.height = 1 + Math.max(getHeight(y.left), getHeight(y.right));
		x.height = 1 + Math.max(getHeight(x.left), getHeight(x.right));
		return x;
	}
	```

## 2 AVL树的整体代码

??? java版
	```java
	import java.util.Queue;
	import java.util.List;
	import java.util.ArrayList;
	import java.util.LinkedList;

	public class AVLTree<K extends Comparable<K>, V> {

		private class Node {
			public K key;
			public V value;
			public Node left;
			public Node right;
			public int height;

			public Node(K key, V value) {
				this.key = key;
				this.value = value;
				this.left = null;
				this.right = null;
				this.height = 1;
			}
		}

		private Node root;
		private int size;

		public AVLTree() {
			root = null;
			size = 0;
		}

		public int getSize() {
			return size;
		}

		public boolean isEmpty() {
			return size == 0;
		}

		// 计算高度
		public int getHeight(Node node) {
			if (node == null) {
				return 0;
			}
			return node.height;
		}

		// 计算平衡因子
		private int getBalanceFactor(Node node) {
			if (node == null) {
				return 0;
			}
			return getHeight(node.left) - getHeight(node.right);
		}

		// 判断是否为二分搜索树
		public boolean isBST() {
			List<K> keys = new ArrayList<>();
			inOrder(root, keys);
			for (int i = 1; i < keys.size(); i++) {
				if (keys.get(i).compareTo(keys.get(i - 1)) < 0) {
					return false;
				}
			}
			return true;
		}

		private void inOrder(Node node, List<K> keys) {
			if (node == null) {
				return;
			}
			inOrder(node.left, keys);
			keys.add(node.key);
			inOrder(node.right, keys);
		}

		// 判断是否为平衡二叉树
		public boolean isBalanced() {
			return isBalanced(root);
		}

		private boolean isBalanced(Node node) {
			if (node == null) {
				return true;
			}
			int balanceFactor = getBalanceFactor(node);
			if (Math.abs(balanceFactor) > 1) {
				return false;
			}
			return isBalanced(node.left) && isBalanced(node.right);
		}

		private Node getNode(Node node, K key) {
			if (node == null) {
				return null;
			}
			if (key.compareTo(node.key) < 0) {
				return getNode(node.left, key);
			} else if (key.compareTo(node.key) > 0) {
				return getNode(node.right, key);
			} else {
				return node;
			}
		}

		public boolean contains(K key) {
			return getNode(root, key) != null;
		}

		public void preOrder() {
			preOrder(root);
		}

		private void preOrder(Node node) {
			if (node == null) {
				return;
			}
			System.out.println(node.key);
			preOrder(node.left);
			preOrder(node.right);
		}

		public void inOrder() {
			inOrder(root);
		}

		private void inOrder(Node node) {
			if (node == null) {
				return;
			}
			inOrder(node.left);
			System.out.println(node.key);
			inOrder(node.right);
		}

		public void postOrder() {
			postOrder(root);
		}

		private void postOrder(Node node) {
			if (node == null) {
				return;
			}
			postOrder(node.left);
			postOrder(node.right);
			System.out.println(node.key);
		}

		public void levelOrder() {
			Queue<Node> q = new LinkedList<>();
			q.add(root);
			while (!q.isEmpty()) {
				Node cur = q.remove();
				System.out.println(cur.key);
				if (cur.left != null) {
					q.add(cur.left);
				}
				if (cur.right != null) {
					q.add(cur.right);
				}
			}
		}

		public V get(K key) {
			Node node = getNode(root, key);
			return node == null ? null : node.value;
		}

		public void set(K key, V newValue) {
			Node node = getNode(root, key);
			if (node == null) {
				throw new IllegalArgumentException(key + "doesn't exist!");
			}
			node.value = newValue;
		}

		// 右旋
		//        y                            x
		//       / \                        /     \
		//      x   T4     向右旋转(y)     z       y
		//     / \       - - - - - - ->   / \     / \
		//    z   T3                    T1   T2 T3   T4
		//   / \
		// T1 T2
		private Node rightRotate(Node y) {
			Node x = y.left;
			Node T3 = x.right;
			x.right = y;
			y.left = T3;
			y.height = 1 + Math.max(getHeight(y.left), getHeight(y.right));
			x.height = 1 + Math.max(getHeight(x.left), getHeight(x.right));
			return x;
		}

		// 左旋
		//    y                                   x
		//   / \                               /     \
		// T1   x           向左旋转(y)       y       z
		//     / \          - - - - - - ->   / \     / \
		//   T2   z                        T1   T2 T3   T4
		//       / \
		//     T3   T4	
		private Node leftRotate(Node y) {
			Node x = y.right;
			Node T2 = x.left;
			x.left = y;
			y.right = T2;
			y.height = 1 + Math.max(getHeight(y.left), getHeight(y.right));
			x.height = 1 + Math.max(getHeight(x.left), getHeight(x.right));
			return x;
		}

		public void add(K key, V value) {
			root = add(root, key, value);
		}

		private Node add(Node node, K key, V value) {
			if (node == null) {
				size++;
				return new Node(key, value);
			}
			if (key.compareTo(node.key) < 0) {
				node.left = add(node.left, key, value);
			}
			if (key.compareTo(node.key) > 0) {
				node.right = add(node.right, key, value);
			} else {
				node.value = value;
			}
			// 计算高度
			node.height = 1 + Math.max(getHeight(node.left), getHeight(node.right));
			// 计算平衡因子
			int balanceFactor = getBalanceFactor(node);
			// LL
			if (balanceFactor > 1 && getBalanceFactor(node.left) >= 0) {
				return rightRotate(node);
			}
			// RR
			if (balanceFactor < -1 && getBalanceFactor(node.right) <= 0) {
				return leftRotate(node);
			}
			// LR
			if (balanceFactor > 1 && getBalanceFactor(node.left) < 0) {
				node.left = leftRotate(node.left);
				return rightRotate(node);
			}
			// RL
			if (balanceFactor < -1 && getBalanceFactor(node.right) > 0) {
				node.right = rightRotate(node.right);
				return leftRotate(node);
			}
			return node;
		}

		public K findMin() {
			if (size == 0) {
				throw new IllegalArgumentException("Can not find something from a blank tree.");
			}
			return findMin(root).key;
		}

		private Node findMin(Node node) {
			if (node.left == null) {
				return node;
			}
			return findMin(node.left);
		}

		public K removeMin() {
			K ret = findMin();
			root = removeMin(root);
			return ret;
		}

		private Node removeMin(Node node) {
			if (node.left == null) {
				Node rightNode = node.right;
				node.right = null;
				size--;
				return rightNode;
			}
			node.left = removeMin(node.left);
			return node;
		}

		public void remove(K key) {
			root = remove(root, key);
		}

		private Node remove(Node node, K key) {
			if (node == null) {
				return null;
			}
			Node retNode;
			if (key.compareTo(node.key) < 0) {
				node.left = remove(node.left, key);
				retNode = node;
			} else if (key.compareTo(node.key) > 0) {
				node.right = remove(node.right, key);
				retNode = node;
			} else {
				if (node.left == null) {
					Node rightNode = node.right;
					node.right = null;
					size--;
					retNode = rightNode;
				} else if (node.right == null) {
					Node leftNode = node.left;
					node.left = null;
					size--;
					retNode = leftNode;
				} else {
					Node successor = findMin(node.right);
					// successor.right = removeMin(node.right);
					successor.right = remove(node.right, successor.key);
					successor.left = node.left;
					node.left = node.right = null;
					retNode = successor;
				}
			}
			if (retNode == null) {
				return null;
			}
			retNode.height = 1 + Math.max(getHeight(retNode.left), getHeight(retNode.right));
			int balanceFactor = getBalanceFactor(retNode);
			// LL
			if (balanceFactor > 1 && getBalanceFactor(retNode.left) >= 0) {
				return rightRotate(retNode);
			}
			// RR
			if (balanceFactor < -1 && getBalanceFactor(retNode.right) <= 0) {
				return leftRotate(retNode);
			}
			// LR
			if (balanceFactor > 1 && getBalanceFactor(retNode.left) < 0) {
				retNode.left = leftRotate(retNode.left);
				return rightRotate(retNode);
			}
			// RL
			if (balanceFactor < -1 && getBalanceFactor(retNode.right) > 0) {
				retNode.right = rightRotate(retNode.right);
				return leftRotate(retNode);
			}
			return retNode;
		}
	}
	```