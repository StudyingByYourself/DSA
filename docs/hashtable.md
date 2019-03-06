# 哈希表O(1)

## Seperate  Chaining(链地址法)

数组中每一个位置存储一个链表或者TreeMap

比较两个对象是否相同：hashCode()与equals()

### 哈希函数的设计--------“键”转换为“索引”

整数：小范围正整数直接使用，小范围负整数进行偏移，大整数取模时取一个素数，能使分布均匀

浮点数：用32位或者64位的二进制表示，再取模

字符串：以单词code为例

<font size=4>
hash(code) = ( c x B^3 + o x B^2 + d x B^1 + e x B^0 ) % M

= ((((c % M x B) + o) % M x B + d) % M * B + e) % M 
</font>

!!! 字符串
	```java
	int hash = 0;
	for (int i = 0;i < s.length() ;i++) {
		hash = (hash * B + s.charAt(i)) % M;
	}
	```

### 哈希冲突

不同的“键”通过哈希函数的转换得到了相同的索引

### 动态扩容(resize)

当元素个数 >= 地址数量 x upper tolerance, 则需要扩容

当元素个数 < 地址数量 x upper tolerance,则需要缩容

这里的扩容操作类比Array动态数组的扩容操作有所不同，动态数组的扩容操作可以在原有容量上x2实现，但哈希表的扩容出于对地址数量为素数的考虑，需要在哈希表中创建一张素数表，当扩容时，地址数量取素数表的下一个值。

??? java实现
	```java
	import java.util.TreeMap;
	public class HashTable<K, V> {
		private int[] capacity = { 53, 97, 193, 389, 769, 1543, 3079, 6151, 12289, 24593, 
				49157, 98317, 196613, 393241,786433, 1572869, 3145739, 6291469, 12582917, 
				25165843, 50331653, 100663319, 201326611, 402653189, 805306457,1610612741 };

		private static final int upperTol = 10;
		private static final int lowerTol = 2;

		private int capacityIndex = 0;
		private TreeMap<K, V>[] hashtable;
		private int M;
		private int size;

		@SuppressWarnings("unchecked")
		public HashTable() {
			this.M = capacity[capacityIndex];
			this.size = 0;
			hashtable = new TreeMap[M];
			for (int i = 0; i < M; i++) {
				hashtable[i] = new TreeMap<>();
			}
		}

		private int hash(K key) {
			return (key.hashCode() & 0x7fffffff) % M;
		}

		public int getSize() {
			return size;
		}

		public void set(K key, V value) {
			TreeMap<K, V> map = hashtable[hash(key)];
			if (!map.containsKey(key)) {
				throw new IllegalArgumentException(key + "doesn't exist!");
			}
			map.put(key, value);
		}

		public boolean contains(K key) {
			return hashtable[hash(key)].containsKey(key);
		}

		public V get(K key) {
			return hashtable[hash(key)].get(key);
		}

		public void add(K key, V value) {
			TreeMap<K, V> map = hashtable[hash(key)];
			if (map.containsKey(key)) {
				map.put(key, value);
			} else {
				map.put(key, value);
				size++;
				if (size >= upperTol * M && capacityIndex + 1 < capacity.length) {
					capacityIndex++;
					resize(capacity[capacityIndex]);
				}
			}
		}

		public V remove(K key) {
			TreeMap<K, V> map = hashtable[hash(key)];
			V ret = null;
			if (map.containsKey(key)) {
				ret = map.remove(key);
				size--;
				if (size < lowerTol * M && capacityIndex - 1 >= 0) {
					capacityIndex--;
					resize(capacity[capacityIndex]);
				}
			}
			return ret;
		}

		@SuppressWarnings("unchecked")
		private void resize(int newM) {
			TreeMap<K, V>[] newHashTable = new TreeMap[newM];
			for (int i = 0; i < newM; i++) {
				newHashTable[i] = new TreeMap<>();
			}
			int oldM = M;
			this.M = newM;
			for (int i = 0; i < oldM; i++) {
				TreeMap<K, V> map = hashtable[i];
				for (K key : map.keySet()) {
					newHashTable[hash(key)].put(key, map.get(key));
				}
			}
			hashtable = newHashTable;
		}
	}
	```

## 开放地址法

线性探测：当哈希冲突时，不断往后面相邻的空位置填(+1)

平方探测：当哈希冲突时，不断往后面空位置填(+1,+4,+9,+16)

二次哈希：当哈希冲突时，用另一个哈希函数开来选择位置

备注：当负载率(哈希表的元素个数占整个地址数量的百分比)到达一定程度，需要扩容操作

## java哈希表的底层实现

java8以前，哈希表底层通过以存储链表的数组实现

java8开始，当哈希冲突比较小的时候用链表，当哈希冲突比较大的时候会将链表转成红黑树

