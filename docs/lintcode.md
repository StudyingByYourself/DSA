# 数组(双指针和滑动窗口)

## other
539\. Move Zeroes

172\. Remove Element

100\. Remove Duplicates from Sorted Array

101\. Remove Duplicates from Sorted Array II

## 归并
64\. Merge Sorted Array 

## 快速排序
5\. Kth Largest Element

148\. Sort Colors ----三路快排

## 双指针
608\. Two Sum II - Input array is sorted

1282\. Reverse Vowels of a String

1283\. Reverse String

383\. Container With Most Water

415\. Valid Palindrome

## 滑动窗口(Sliding Window)(https://blog.csdn.net/yy254117440/article/details/53025142)
406\. Minimum Size Subarray Sum

384\. Longest Substring Without Repeating Characters

647\. Find All Anagrams in a String

首先统计字符串p的字符个数，然后用两个变量l和r表示滑动窗口的左右边界，用变量count表示字符串p中需要匹配的字符个数，然后开始循环，如果右边界的字符已经在哈希表中了，说明该字符在p中有出现，则count自减1，然后哈希表中该字符个数自减1，右边界自加1，如果此时count减为0了，说明p中的字符都匹配上了，那么将此时左边界加入结果res中。

如果此时r和l的差为p的长度，说明此时应该去掉最左边的一个字符，我们看如果该字符在哈希表中的个数大于等于0，说明该字符是p中的字符，为啥呢，因为上面我们有让每个字符自减1，如果不是p中的字符，那么在哈希表中个数应该为0，自减1后就为-1，所以这样就知道该字符是否属于p，如果我们去掉了属于p的一个字符，count自增1，参见代码如下

备注：r和l的差为p的长度时有两种情况，一种是刚好匹配完全了需要的所有字符，另一种是遍历了p的长度，发现完全不符合

??? java实现
	```java
	public List<Integer> findAnagrams(String s, String p) {
			List<Integer> list = new ArrayList<>();
			char[] ss = s.toCharArray();
			char[] pp = p.toCharArray();
			int[] map = new int[256];
			for (int i = 0; i < pp.length; i++) {
				map[pp[i]]++;
			}
			int l = 0, r = -1, count = pp.length;
			while (r + 1 < ss.length) {
				if (map[ss[++r]]-- >= 1) {
					count--;
				}
				if (count == 0) {
					list.add(l);
				}
				if (r - l + 1 == pp.length && map[ss[l++]]++ >= 0) {
					count++;
				}
			}
			return list;
		}
	```

32\. Minimum Window Substring


Sort Characters By Frequency(leetcode 451)