---
title: "{ Java Algorithms } Binary search"
date: 2022-09-07 14:00:00 +07:00
tags: [Algorithm, binary search, java]
description: "Binary Search"
---

### What is the binary search?

- Binary search is an algorithm that searches a target within a sorted array.
- Binary search is not stating at index 0, but starts from the middle (the half of the array) .
- If the number is bigger, index moves to the lefthand side.
- If the number is smaller than the target number, we focus on the righthand side.

### Time complexity &rarr; Big O(logN)

- Searching a target index by spliting a half of an array until the end.
- Keep going until remaining data is one (worst case).
- If the number of elements is 2^n, you would be able to find the target through n times of search.
- e.g. If array has 8 elements, 8 => 4 => 2 => 1, 3 steps are required.
- When the number of inputs is N,

- Step 1. $$ \frac{1}{2} \* N $$
- Step 2. $$ \frac{1}{2} _ \frac{1}{2} _ N$$
- Step 3. $$\frac{1}{2} * \frac{1}{2} * \frac{1}{2} * N$$
- Step K. $$\frac{1}{2}^K * N $$

- Therefore, time complexity is O(log N) (Ignore constant value).

```java
package practice_09_08;

public class BinarySearch {

	public static void main(String[] args) {
		int[] nums = { -1, 0, 3, 5, 9, 12 };
		int target = 9;
		System.out.println(search(nums, target));
	}

	public static int search(int[] nums, int target) {

		int left = 0;
		int right = nums.length - 1;
		int mid = 0;

		// We can see there is no more target when left index is bigger than right index.
		while (left <= right) {

			mid = (left + right) / 2;

			/**
			 * Condition 1. if middle value is equal to target value, return the index
			 * Condition 2. if middle value is smaller than target value, focus on the left side
			 * Condition 3. if middle value is bigger than target value, focus on the right side
			 */
			if (nums[mid] == target) {
				return mid;
			} else if (nums[mid] < target) {
				left = mid + 1;
			} else {
				right = mid - 1;
			}
		}

		return -1;

	}
}

```
