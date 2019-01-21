<font size=3>

# 买股票问题

## Best Time to Buy and Sell Stock 149

### 方法一：时间复杂度O(n^2^)

- 两次for循环，第一层循环寻找买入价，第二层循环寻找卖出价

??? java版
	```java
	public class Solution {
	    /**
	     * @param prices: Given an integer array
	     * @return: Maximum profit
	     */
	    public int maxProfit(int[] prices) {
	        int n = prices.length;
	        if (0 == n) {
	        	return 0;
	        }
	        int profit = 0;
	        for (int i = 0;i < n - 1;i++){
	            for (int j = i + 1;j < n;j++) {
	                if (prices[j] - prices[i] > profit ){
	                    profit = prices[j] - prices[i];
	                } 
	            }
	        } 
	        return profit;
	    }
	}
	```

### 方法二：时间复杂度O(n)

- 一次for循环，min记录当前位置的最小值，profit记录到当前位置的最大获益。

??? java版
	```java
	public class Solution {
	    /**
	     * @param prices: Given an integer array
	     * @return: Maximum profit
	     */
	    public int maxProfit(int[] prices) {
	        int n = prices.length;
	        if (0 == n) {
	        	return 0;
	        }
	        int profit = 0;
	        int min = prices[0];
	        for (int i = 1;i < n;i++){
	            min = Math.min(min, prices[i]);
	            profit = Math.max(profit, prices[i] - min);
	        } 
	        return profit;
	    }
	}
	```

## Best Time to Buy and Sell Stock II 150

方法：时间复杂度O(n) 

- 一次for循环，profit累加上相邻两次股票差为正数的收益

??? java版
	```java
	public class Solution {
	    /**
	     * @param prices: Given an integer array
	     * @return: Maximum profit
	     */
	    public int maxProfit(int[] prices) {
	        int n = prices.length;
	        if (0 == n){
	            return 0;
	        } 
	        int profit = 0;
	        for (int i = 0;i < n - 1 ;i++){
	            if (prices[i+1] - prices[i] > 0){
	                profit += prices[i+1] - prices[i];
	            } 
	        } 
	        return profit;
	    }
	}
	```

## Best Time to Buy and Sell Stock III 151

方法：时间复杂度O(n)

![股票](/img/股票.png)

f[i][j]表示前i天在j状态的最大收益(eg:前1天的下标为0)

当处于非持股状态时，考虑前一天处于非持股或者持股状态

<center>
f[i][j] = max{f[i-1][j], f[i-1][j-1] + A[i-1] – A[i-2]}	
</center>

当处于持股状态时，考虑前一天处于非持股或者持股状态

<center>
f[i][j]=max{f[i-1][j-1], f[i-1][j]+A[i-1]–A[i-2], f[i-1][j-2]+A[i-1]–A[i-2]}
</center>

??? java版
	```java
	public class Solution {
		/**
		 * @param prices: Given an integer array
		 * @return: Maximum profit
		 */
		public int maxProfit(int[] A) {
			int n = A.length;
			if (0 == n) {
				return 0;
			}
			int[][] f = new int[n + 1][5 + 1];
			for (int j = 0; j < 6; j++) {
				f[0][j] = 0;
			}
			for (int i = 1; i < n + 1; i++) {
				for (int j = 1; j < 6; j += 2) {
					f[i][j] = f[i - 1][j];
					if (i > 1 && j > 1) {
						f[i][j] = Math.max(f[i][j], f[i-1][j-1] + A[i-1] - A[i-2]);
					}
				}
				for (int j = 2; j < 6; j += 2) {
					f[i][j] = f[i - 1][j - 1];
					if (i > 1) {
						f[i][j] = Math.max(f[i][j], f[i-1][j] + A[i-1] - A[i-2]);
					}
					if (i > 1 && j > 2) {
						f[i][j] = Math.max(f[i][j], f[i-1][j-2] + A[i-1] - A[i-2]);
					}
				}
			}
			int profit = 0;
			for (int j = 1; j < 6; j += 2) {
				if (f[n][j] > profit) {
					profit = f[n][j];
				}
			}
			return profit;
		}
	}
	```

对空间进行优化:空间复杂度O(1)

??? java版
	```java
	public class Solution {
		/**
		 * @param prices: Given an integer array
		 * @return: Maximum profit
		 */
		public int maxProfit(int[] A) {
			int n = A.length;
			if (0 == n) {
				return 0;
			}
			int[][] f = new int[2][5 + 1];
			for (int j = 0; j < 6; j++) {
				f[0][j] = 0;
			}
			int old = 0;
			int now = 1;
			for (int i = 1; i < n + 1; i++) {
				old = now;
				now = 1 - now;
				for (int j = 1; j < 6; j += 2) {
					f[now][j] = f[old][j];
					if (i > 1 && j > 1) {
						f[now][j] = Math.max(f[now][j], f[old][j-1]+A[i-1]-A[i-2]);
					}
				}
				for (int j = 2; j < 6; j += 2) {
					f[now][j] = f[old][j - 1];
					if (i > 1) {
						f[now][j] = Math.max(f[now][j], f[old][j]+A[i-1]-A[i-2]);
					}
					if (i > 1 && j > 2) {
						f[now][j] = Math.max(f[now][j], f[old][j-2]+A[i-1]-A[i-2]);
					}
				}
			}
			int profit = 0;
			for (int j = 1; j < 6; j += 2) {
				if (f[now][j] > profit) {
					profit = f[now][j];
				}
			}
			return profit;
		}
	}
	```
## Best Time to Buy and Sell Stock IV 393

在至多2次买卖的基础上，将2次买卖的5个阶段扩充到k次买卖的2k+1个阶段
??? java版
	```java
	public class Solution {
		/**
		 * @param K: An integer
		 * @param prices: An integer array
		 * @return: Maximum profit
		 */
		public int maxProfit(int K, int[] A) {
			int n = A.length;
			if (0 == n) {
				return 0;
			}
			if (K > n / 2) {
				int res = 0;
				for (int i = 0; i < n - 1; i++) {
					if (A[i + 1] > A[i]) {
						res += A[i + 1] - A[i];
					}
				}
				return res;
			}
			int[][] f = new int[n + 1][2 * K + 1 + 1];
			for (int j = 0; j < 2 * K + 2; j++) {
				f[0][j] = 0;
			}
			for (int i = 1; i < n + 1; i++) {
				for (int j = 1; j < 2 * K + 2; j += 2) {
					f[i][j] = f[i - 1][j];
					if (i > 1 && j > 1) {
						f[i][j] = Math.max(f[i][j], f[i-1][j-1] + A[i-1] - A[i-2]);
					}
				}
				for (int j = 2; j < 2 * K + 2; j += 2) {
					f[i][j] = f[i - 1][j - 1];
					if (i > 1) {
						f[i][j] = Math.max(f[i][j], f[i-1][j] + A[i-1] - A[i-2]);
					}
					if (i > 1 && j > 2) {
						f[i][j] = Math.max(f[i][j], f[i-1][j-2] + A[i-1] - A[i-2]);
					}
				}
			}
			int profit = 0;
			for (int j = 1; j < 2 * K + 2; j += 2) {
				if (f[n][j] > profit) {
					profit = f[n][j];
				}
			}
			return profit;
		}
	}
	```
	
</font>