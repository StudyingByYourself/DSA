# QuickSort

??? 快速排序参考
	(http://developer.51cto.com/art/201403/430986.htm)

## 一般的快速排序
假设我们现在对“6  1  2 7  9  3  4  5 10  8”这个10个数进行排序。首先以第一个数6作为基准数吧。接下来，需要将这个序列中所有比基准数大的数放在6的右边，比基准数小的数放在6的左边，类似下面这种排列：
3  1  2 5  4  6  9 7  10  8
<center>
![在这里插入图片描述](/img/quickSort.gif)
</center>

???实现代码
	```java
	public class QuickSort {
		public static void sort2(Comparable[] arr, int lo, int hi) {
			if (lo > hi) {
				return;
			}
			Comparable sentinel = arr[lo];
			int i = lo;
			int j = hi;
			while (i != j) {
				while (arr[j].compareTo(sentinel) >= 0 && i < j) {
					j--;
				}
				if (i < j) {
					swap(arr, i, j);
				}
				while (arr[i].compareTo(sentinel) <= 0 && i < j) {
					i++;
				}
				if (i < j) {
					swap(arr, i, j);
				}
			}
			sort2(arr, lo, i - 1);
			sort2(arr, i + 1, hi);
		}

		public static void sort(Comparable[] arr, int lo, int hi) {
			if (lo > hi) {
				return;
			}
			Comparable sentinel = arr[lo];
			int i = lo;
			int j = hi;
			while (i != j) {
				while (arr[j].compareTo(sentinel) >= 0 && i < j) {
					j--;
				}
				while (arr[i].compareTo(sentinel) <= 0 && i < j) {
					i++;
				}
				swap(arr, i, j);
			}
			if (i > lo) {
				arr[lo] = arr[i];
				arr[i] = sentinel;
			}
			sort(arr, lo, i - 1);
			sort(arr, i + 1, hi);
		}

		private static void swap(Object[] arr, int i, int j) {
			Object t = arr[i];
			arr[i] = arr[j];
			arr[j] = t;
		}
	}
	```
## 随机化快速排序
如果需要排序的对象是近乎有序的，这时快速排序算法的时间复杂度将退化成O(n^2^)，为了维持O(nlogn)的时间复杂度，需要改变选择的基准(sentinel)，取其随机一个数为基准。

??? 代码实现
	```java
	import java.util.Arrays;
	public class QuickSort2 {
		public static void sort(Comparable[] arr, int lo, int hi) {
			if (lo > hi) {
				return;
			}
			swap(arr, lo, (int) (Math.random() * (hi - lo + 1)) + lo);
			Comparable sentinel = arr[lo];
			int i = lo;
			int j = hi;
			while (i != j) {
				while (arr[j].compareTo(sentinel) >= 0 && i < j) {
					j--;
				}
				while (arr[i].compareTo(sentinel) <= 0 && i < j) {
					i++;
				}
				swap(arr, i , j);
			}
			if (i > lo) {
				arr[lo] = arr[i];
				arr[i] = sentinel;
			}
			sort(arr, lo, i - 1);
			sort(arr, i + 1, hi);
		}
		
		private static void swap(Comparable[] arr, int i , int j) {
			Comparable t = arr[i];
	        arr[i] = arr[j];
	        arr[j] = t;
		}
	}
	```

## 三路快速排序
然而当需要排序的对象中存在大量重复元素时，则需要进一步优化。

**动画演示**
![在这里插入图片描述](/img/三路快排.gif)

??? 代码实现
	```java
	import java.util.Arrays;

	public class QuickSort3 {
		public static void sort(Comparable[] arr, int lo, int hi) {
			if (lo > hi) {
				return;
			}
			swap(arr, lo, (int) (Math.random() * (hi - lo + 1)) + lo);
			Comparable sentinel = arr[lo];
			int lt = lo;
			int gt = hi + 1;
			int i = lo + 1;
			while (i < gt) {
				if (arr[i].compareTo(sentinel) < 0) {
					swap(arr, i, lt + 1);
					i++;
					lt++;
				} else if (arr[i].compareTo(sentinel) > 0) {
					swap(arr, i, gt - 1);
					gt--;
				} else {
					i++;
				}
			}
			swap(arr, lo, lt);
			sort(arr, lo, lt - 1);
			sort(arr, gt, hi);
		}
		
		private static void swap(Comparable[] arr, int i , int j) {
			Comparable t = arr[i];
	        arr[i] = arr[j];
	        arr[j] = t;
		}

		public static void main(String[] args) {
			int n = 1000000;
			Comparable[] arr = new Comparable[n];
			for (int i = 0; i < n; i++) {
				arr[i] = i / 100000;
			}
			long startTime = System.nanoTime();
	        sort(arr, 0, arr.length - 1);
	        long endTime = System.nanoTime();
	        System.out.println((endTime - startTime) / 1000000000.0 + " s");
		}
	}
	```








<script type="text/javascript" async
  src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.5/MathJax.js?config=TeX-MML-AM_CHTML">
</script>
<script type="text/x-mathjax-config">
MathJax.Hub.Config({
  tex2jax: {inlineMath: [['$','$'], ['\\(','\\)']]}
});
</script>
