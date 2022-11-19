---
title: "{ Java Algorithms } Tech Interview Preparation_1"
date: 2022-11-18 23:00:00 +07:00
tags: [Algorithm, Binary Search, Coding Interview, java]
---

### How to answer the interview questions

- 어떠한 제약이 없는지 확인한다. (e.g. any limitation of time complexity, length of a string)
- 문제를 이해 했는지 보여주기 위하여 define 을 해야한다. (this problem is...)
- 예시 선언 (e.g. 나는 이렇게 해서 테스트 할거다. I will try to solve the problem using '~~' approach first. Or using a stack, hashmap..)
- 쉬운방법으로 먼저 접근 가능. (brute force approach will be okay but improve your answer if you can)
- 코드 작성 시작
- 코드 작성하면서 말로 못하면 코멘트를 달면서 진행.
- 답변 중 stuck 이 될 시 example 에 대입을 하면서 테스트 해야함.
- 마무리 with time complexity and space complexity.

### Time complexity & Space complexity

- 시간 복잡도는 연산의 개수를 세어 얼마만큼의 연산(대입, 덧셈, 곱셈, 나눗셈, 비교)이 수행되는가
- 공간 복잡도는 순환 호출을 할 경우 요구되는 추가 공간, 그러니까 동적으로 필요한 공간 (e.g 로컬 변수만 사용되며 상수 값만 요구되는 경우 (어레이를 선언하지 않는 경우 = O(1))

### Patterns

- Pattern 1. 만약 주어진 인풋이 sorted 되 있으면, '1. Binary Search' 나 '2. Two Pointers' 전략을 사용할 것.
- Pattern 2. top/maximum/minimum/closest k elements among n elements 를 다룰때, '3. Heap' 을 사용할 것.
- Pattern 3. 만약 인풋의 조합 또는 순열을 시도해야 하는 경우에는 '4. recursive backtracking' 또는 '5. iterative Breadth-First Search' 를 시도 해 볼 수 있다.

- Sample Problem 1. Binary Search: Bitonic array maximum
- Monotonically increasing or decreasing means that for any index i in the array, arr[i] != arr[i+1]
- A bitonic array is a sorted array; the only difference is that its first part is sorted in ascending order, and the second part is sorted in descending order.

### What is the Binary search?

- 바이너리 서치란 정렬된 배열에서 어떠한 타겟이 되는 값의 위치를 찾는 알고리즘
- 배열의 중간 인덱스에 있는 값 부터 시작해서 탐색 시작
- Binary search is not stating at index 0, but starts from the middle (the half of the array)
- 그 값 보다 크면 왼쪽에 있는 values 을 discard 하고, 남은 어레이 안의 또 중간 인덱스에 있는 값과 탐색시작
- 반대로 작으면 오른쪽에 있는 values 을 discard 하고 탐색 진행.

### Binary search Time complexity &rarr; Big O(logN)

- 어레이 값들의 절반을 계속 나누어 쪼개면서 탐색.
- worst case 를 생각해보면 남은 데이터가 하나가 될 때 까지 탐색 해야함.
- 만약 탐색을 해야하는 값의 수가 2^n 이라면 n 번의 탐색을 통해서 원하는 값을 찾을 수 있음.
- e.g. 8 개의 값들을 가지는 어레이 라면, 8 => 4 => 2 => 1, 3번의 steps 이 required.
- 자료의 갯수 N에 따른 시행 횟수는 따라서 시간 복잡도는 Big O 표기법으로 O(logN)으로 나타낼 수 있음. (상수 부분은 무시하기 때문에)

### 풀이

- Brute Force approach
  Time complexity is O(N)
  Space complexity O(N) where N is the size of the search input.

```java
for(int i = 0; i < nums.length; i++) {
	if(nums[i] == target) {
  	return i;
  }
  return -1;
}

```

- Binary Search approach
  Time complexity is O(logN)
  Space complexity is O(1) - 로컬 변수만 사용, 루프 안에 따로 array 등이 선언되지 않았다.

```java
class Solution {
    public int search(int[] nums, int target) {
        int left = 0;
        int right = nums.length - 1;
        int mid = 0;

        while(left <= right) {
            mid = (left + right) / 2;
            if(nums[mid] == target) {
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

### Two Pointer approach

- 한 포인터는 배열의 시작 값을 가리키고 다른 포인터는 끝 값을 가리키면서 시작
- 모든 단계에서 두 포인터의 합이 일치하는지 확인
- 맞다면 해당 인덱스를 리턴함
- 만약 아니라면, 두 가지 중 하나를 수행

1. 만약 두 포인터가 가리키는 두 숫자의 합이 타켓 값보다 크면, 더 작은 합을 가진 쌍이 필요하기 때문에, 끝 값을 가리키는 포인터를 줄인다.
2. 만약 두 포인터가 가리키는 두 숫자의 합이 타멧 값보다 작으면, 더 큰 합을 가진 쌍이 필요하기 때문에, 시작 값을 가리키는 포인터를 늘린다.

```java
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

### Same Strategy but would be a better way

```java
class Solution {
    public int search(int[] nums, int target) {
    int left = 0, right = arr.length - 1;
    while (left < right) {

      // 정수 값이 너무 커지면 오퍼플로우 발생 가능성이 있기 때문에
      // '타켓에서 시작값을 뺀 차이' 를 '끝 값' 과 비교하는 방법을 선호한다.
      int targetDiff = target - arr[left];
      if (targetDiff == arr[right])
        return new int[] { left, right }; // found the pair

      if (targetDiff > arr[right])
        left++; // we need a pair with a bigger sum
      else
        right--; // we need a pair with a smaller sum
    }
    return new int[] { -1, -1 };
}
```

### K closest points to the origin

- Pattern 2. top/maximum/minimum/closest k elements among n elements 를 다룰때, 'Heap' 을 사용할 것.

```java
class Solution {
   	public int[][] kClosest(int[][] points, int k) {

		// Declare PriorityQueue - max heap 사용하여 탑 item 을 제거하기 위한 전략
		// We can use a Max Heap to find K points closest to the origin.

		// x,y 좌표 두개의 제곱을 가진 point 를 PriorityQueue 를 사용하여
		PriorityQueue<int[]> maxheap = new PriorityQueue<int[]>(
	    (p1, p2) -> p2[0] * p2[0] + p2[1] * p2[1] - p1[0] * p1[0] + p1[1] * p1[1]);
        // start by pushing K points in the heap.
		for (int[] p : points) {
            // While iterating through the remaining points,
            // add P to always keep the closest points in the heap.
			maxheap.add(p);
        // if a point (say P) is closer to the origin than the top point of the max-heap
			if (maxheap.size() > k) {
                // remove that top point from the heap
				maxheap.poll();
			}
		}
		int[][] result = new int[k][2];
		while (k > 0) {
			result[--k] = maxheap.poll();
		}
		return result;

	}
}
```
