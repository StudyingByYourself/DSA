# Binary Search Tree

## 5.二分搜索树(Binary Search Tree, BST)

### 5.1 二分搜索树的整体代码

??? java版
	```java
	import java.util.LinkedList;
	import java.util.Queue;

	public class BST<E extends Comparable<E>> {
		private class Node {
			public E e;
			public Node left;
			public Node right;

			public Node(E e) {
				this.e = e;
				this.left = null;
				this.right = null;
			}
		}

		private Node root;
		private int size;

		public BST() {
			root = null;
			size = 0;
		}

		public int size() {
			return size;
		}

		public boolean isEmpty() {
			return size == 0;
		}

		public void add(E e) {
			root = add(root, e);
		}

		// return the root node after add a new node
		private Node add(Node node, E e) {
			if (node == null) {
				size++;
				return new Node(e);
			}
			if (e.compareTo(node.e) < 0) {
				node.left = add(node.left, e);
			} else if (e.compareTo(node.e) > 0) {
				node.right = add(node.right, e);
			}
			return node;
		}

		public boolean contains(E e) {
			return contains(root, e);
		}

		private boolean contains(Node node, E e) {
			if (node == null) {
				return false;
			}
			if (e.compareTo(node.e) < 0) {
				return contains(node.left, e);
			} else if (e.compareTo(node.e) == 0) {
				return true;
			} else {
				return contains(node.right, e);
			}
		}

		public void preOrder(Node node) {
			if (node == null) {
				return;
			}
			System.out.println(node.e);
			preOrder(node.left);
			preOrder(node.right);
		}

		public void inOrder(Node node) {
			if (node == null) {
				return;
			}
			inOrder(node.left);
			System.out.println(node.e);
			inOrder(node.right);
		}

		public void postOrder(Node node) {
			if (node == null) {
				return;
			}
			postOrder(node.left);
			postOrder(node.right);
			System.out.println(node.e);
		}

		public void levelOrder() {
			Queue<Node> queue = new LinkedList<>();
			queue.add(root);
			while (!queue.isEmpty()) {
				Node cur = queue.remove();
				System.out.println(cur.e);
				if (cur.left != null) {
					queue.add(cur.left);
				}
				if (cur.right != null) {
					queue.add(cur.right);
				}
			}
		}

		public E minimum() {
			if (size == 0) {
				throw new IllegalArgumentException("BST is empty!");
			}
			return minimum(root).e;
		}

		private Node minimum(Node node) {
			if (node.left == null) {
				return node;
			}
			return minimum(node.left);
		}

		public E removeMin() {
			E ret = minimum();
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

		public E maximum() {
			if (size == 0) {
				throw new IllegalArgumentException("BST is empty!");
			}
			return maximum(root).e;
		}

		private Node maximum(Node node) {
			if (node.right == null) {
				return node;
			}
			return maximum(node.right);
		}

		public E removeMax() {
			E ret = maximum();
			root = removeMax(root);
			return ret;
		}

		private Node removeMax(Node node) {
			if (node.right == null) {
				Node leftNode = node.left;
				node.left = null;
				size--;
				return leftNode;
			}
			node.right = removeMax(node.right);
			return node;
		}

		public void remove(E e) {
			root = remove(root, e);
		}

		private Node remove(Node node, E e) {
			if (node == null) {
				return null;
			}
			if (e.compareTo(node.e) < 0) {
				node.left = remove(node.left, e);
				return node;
			} else if (e.compareTo(node.e) > 0) {
				node.right = remove(node.right, e);
				return node;
			} else {
				if (node.left == null) {
					Node rightNode = node.right;
					node.right = null;
					size--;
					return rightNode;
				}
				if (node.right == null) {
					Node leftNode = node.left;
					node.left = null;
					size--;
					return leftNode;
				}
				Node successor = minimum(node.right);
				successor.right = removeMin(node.right);
				successor.left = node.left;
				node.left = node.right = null;
				return successor;
			}
		}
	}
	```
### 5.2 add method 

####  5.2.1 非递归实现（not recursion）

??? java版
	```java
	public void add(E e) {
	    Node node = new Node(e);
	    if (root == null) {
	        root = node;
	        size++;
	    } else {
	        Node temp = root;
	        while (temp != null) {
	            if (e.compareTo(temp.e) < 0) {
	                if (temp.left == null) {
	                    temp.left = node;
	                    size++;
	                    return;
	                } else {
	                    temp = temp.left;
	                }
	            } else {
	                if (temp.right == null) {
	                    temp.right = node;
	                    szie++;
	                    return;
	                } else {
	                    temp = temp.right;
	                }
	            }
	        }
	    }
	}
	```

####  5.2.2 递归实现（recursion）

??? java版
	```java
	public void add(E e) {
	    if (root == null) {
	        root = new Node(e);
	        size++;
	    } else {
	        add(root, e);
	    }
	}

	private void add(Node node, E e) {
	    if (e.equals(node.e)) {
	        return;
	    } else if (e.compareTo(node.e) < 0 && node.left == null) {
	        node.left = new Node(e);
	        size++;
	        return;
	    } else if (e.compareTo(node.e) > 0 && node.right == null) {
	        node.right = new Node(e);
	        size++;
	        return;
	    }
	    if (e.compareTo(node.e) < 0) {
	        add(node.left, e);
	    } else {
	        add(node.right, e);
	    }
	}
	```


####  5.2.3 递归实现（optimize and refactor recursion method）

??? java版
	```java
	// recurse method optimization and refactor
	public void add(E e) {
	    root = add(root, e);
	}

	//return the root node after add a new node
	private Node add(Node node, E e) {
	    if (node == null) {
	        size++;
	        return new Node(e);
	    }
	    if (e.compareTo(node.e) < 0) {
	        node.left = add(node.left, e);
	    } else if (e.compareTo(node.e) > 0) {
	        node.right = add(node.right, e);
	    }
	    return node;
	}
	```

### 5.3 search method

####  5.3.1 非递归实现（not recursion）

??? java版
	```java
	public boolean contains(E e) {
	    Node temp = root;
	    while (temp != null) {
	        if (e.compareTo(temp.e) == 0) {
	            return true;
	        } else if (e.compareTo(temp.e) < 0) {
	            temp = temp.left;
	        } else {
	            temp = temp.right;
	        }
	    }
	    return false;
	}
	```

####  5.3.2 递归实现（recursion）

??? java版
	```java
	public boolean contains(E e) {
	    return contains(root, e);
	}

	private boolean contains(Node node, E e) {
	    if (node == null) {
	        return false;
	    }
	    if (e.compareTo(node.e) < 0) {
	        return contains(node.left, e);
	    } else if (e.compareTo(node.e) == 0) {
	        return true;
	    } else {
	        return contains(node.right, e);
	    }
	}
	```

### 5.4 traversal method

####  5.4.1 非递归实现（not recursion）

??? 栈Stack实现
	```java
	public void preOrder() {
	    Stack < Node > stack = new Stack < > ();
	    stack.push(root);
	    while (!stack.isEmpty()) {
	        Node cur = stack.pop();
	        System.out.println(cur.e);
	        if (cur.right != null) {
	            stack.push(cur.right);
	        }
	        if (cur.left != null) {
	            stack.push(cur.left);
	        }
	    }
	}
	```

####  5.4.2 递归实现（recursion）

??? java版
	```java
	public void preOrder(Node node) {
	    if (node == null) {
	        return;
	    }
	    System.out.println(node.e);
	    preOrder(node.left);
	    preOrder(node.right);
	}
	```


