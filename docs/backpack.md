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