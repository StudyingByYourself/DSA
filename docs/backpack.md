<font size=4>
# Backpack

??? 背包问题参考
	背包问题背景：在一定容量的背包中装入含有不同容量和价值的物品
	(https://blog.csdn.net/ppp_1026hc/article/details/52138025)

## 0-1背包

### [Backpack 92](https://www.lintcode.com/problem/backpack/)

f[i][j] means whether the former i number can make up j.

<CENTER>
f[i][j] = f[i-1][j] || (f[i-1][j-A[i-1]] | j-A[i-1] >= 0)	
</CENTER>

??? 代码实现
	```java
	public class Backpack {
		public int backPack(int m, int[] A) {
			int n = A.length;
			if (n == 0) {
				return 0;
			}
			boolean[][] f = new boolean[n + 1][m + 1];
			f[0][0] = true;
			for (int j = 1; j < m + 1; j++) {
				f[0][j] = false;
			}
			for (int i = 1; i < n + 1; i++) {
				f[i][0] = true;
				for (int j = 1; j < m + 1; j++) {
					f[i][j] = f[i - 1][j];
					if (j - A[i - 1] >= 0) {
						f[i][j] = f[i][j] || f[i - 1][j - A[i - 1]];
					}
				}
			}
			for (int j = m; j >= 0; j--) {
				if (f[n][j]) {
					return j;
				}
			}
			return 0;
		}	
	}
	```
### 实际问题

淘宝上的天猫超市有满199减100的活动，如何凑单能满199，并且尽量接近199？

即求大于等于目标值的最小值，并且输出一种具体的方案。

思路(求大于等于目标值的最小值)：

f[i][j] means whether the former i number can make up j.

在new f[n + 1][target + 1] 时将target扩大两倍,f[n + 1][target * 2 + 1]

从f[n][target]开始寻找，若不为真，则将target += 1，直到f[n][target]为真。

思路(输出一种具体的方案)：

用pai[i][j] 表示拼出j的最后一个数可以是pai[i][j]

??? java实现
	```java
	public class Shopping {
		public static void shoppingSnacks(int[] prices, int target) {
			int n = prices.length;
			if (n == 0 && target != 0) {
				return;
			}
			boolean[][] f = new boolean[n + 1][target * 2 + 1];
			int[][] pai = new int[n + 1][target * 2 + 1];
			f[0][0] = true;
			for (int i = 1; i < f.length; i++) {
				for (int j = 0; j < f[0].length; j++) {
					f[i][j] = f[i - 1][j];
					if (j - prices[i - 1] >= 0) {
						f[i][j] = f[i][j] || f[i - 1][j - prices[i - 1]];
						if (f[i - 1][j - prices[i - 1]]) {
							pai[i][j] = prices[i - 1];
						}
					}
				}
			}

			// 选择不小于目标值的最小值
			while (!f[n][target] && target < f[0].length - 1) {
				target++;
			}

			// 打印购物车清单	
			if (f[n][target]) {
				int p = target;
				int q = n;
				System.out.println(p + " = ");
				while (p > 0) {
					System.out.println(pai[q][p]);
					p -= pai[q][p];
					q--;
					while (pai[q][p] == 0 && q > 0) {
						q--;
					}
				}
			} else {
				System.out.println("没有满足要求的方案");
			}
		}
	}
	```
int[] arr = {2, 3, 5},int target = 8;
![具体方案](https://s2.ax1x.com/2019/03/06/kvkhpn.png)

### [BackpackII 125](https://www.lintcode.com/problem/backpack-ii/description)
f[i][j] means the maximum value of the former i number making up j.

<CENTER>
f[i][j] = max{f[i-1][j], f[i-1][j-A[i-1]] + V[i-1] | j-A[i-1] >= 0}
</CENTER>

??? 代码实现
	```java
	public class BackpackII {
		public int backPackII(int m, int[] A, int[] V) {
	        int n = A.length;
	        if (n == 0){
	            return 0;
	        }
	        int[][] f = new int[n + 1][m + 1];
	        f[0][0] = 0;
	        for (int j = 1;j < m + 1 ;j++){
	            f[0][j] = -1;
	        }
	        for (int i = 1;i < n + 1 ;i++){
	            f[i][0] = 0;
	            for (int j = 1;j < m + 1 ;j++){
	                f[i][j] = f[i-1][j];
	                if (j - A[i-1] >= 0 && f[i-1][j - A[i-1]] != -1 ){
	                    f[i][j]=Math.max(f[i][j],f[i-1][j - A[i-1]]+V[i-1]);
	                } 
	            } 
	        } 
	        int res = 0;
	        for (int j = 0;j < m + 1 ;j++){
	            res = Math.max(res, f[n][j]);
	        }
	        return res;
	    }
	}
	```

## 完全背包

### [BackpackIII 440](https://www.lintcode.com/problem/backpack-iii/description)
f[i] means the maximum value of size i, if f[i] doen't exist,using Integer.MIN_VALUE to represent it.

<CENTER>
f[i] = max{f[i-A[j]] + V[j]}	
</CENTER>

??? 代码实现
	```java
	public class BackpackIII {		
		public int backPackIII(int[] A, int[] V, int m) {
	        int n = A.length;
	        if (n == 0){
	            return 0;
	        }
	        int[] f = new int[m + 1];
	        f[0] = 0;
	        for (int i = 1;i < m + 1 ;i++){
	            f[i] = Integer.MIN_VALUE;
	            for (int j = 0; j < n ;j++){
	                if (i - A[j] >= 0 && f[i - A[j]] + V[j] > f[i]){
	                    f[i] = f[i - A[j]] + V[j];
	                } 
	            } 
	        }
	        int res = 0;
	        for (int i = 0;i < m + 1 ;i++){
	            res = Math.max(res, f[i]);
	        } 
	        return res;
	    }	
	}
	```
</font>