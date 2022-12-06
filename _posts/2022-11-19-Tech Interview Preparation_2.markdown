---
title: "{ Java Algorithms } Array"
date: 2022-11-19 23:00:00 +07:00
tags: [Algorithm, Array, Quick sort, Coding Interview, java]
---

### Basic Studies - 다시 기초부터

### Array

1. A collection of elements
2. Purpose: to group similar kinds of data for fast access
3. Declaration

```java
    // Declaration
    datatype arrayName[];
    datatype[] arrayName;
    int array1[];
    int[] array2;
    // Initialization
    datatype[] arrayName = new datatype [size];
    int[] array3 = new int [4];
```

4. Drawback: have to specify the size of the array (size remains fixed, cannot be extended) <--> ArrayList

#### Exercise 1

- Remove all the even elements from the array and returns back updataed array

```java
int[] removeEven(int[] arr)
// input
arr = {1,2,4,5,10,6,3};
// output
arr = {1,5,3};

// code

        int j = 0;

		for(int i = 0; i < arr.length; i++) {
			if(arr[i] % 2 != 0) {
				j++;
			}
		}

		int[] arr2 = new int[j];
		int index = 0;
		for(int i = 0; i < arr.length; i++) {
			if(arr[i] % 2 != 0) {
				arr2[index++] = arr[i];
			}
		}
 		return arr2; // change this and return the correct result array
```

#### Exercise 2

- Sort and merge the given two arrays

```java
int[] mergeArrays(int[] arr1, int[] arr2)
// input
arr1 = {1, 3, 4, 5}
arr2 = {2, 6, 7, 8}
// output
arr = {1,2,3,4,5,6,7,8}

// code
      // calculate the number of elements in two arrays
      int s1 = arr1.length;
      int s2 = arr2.length;
      int totalSize = s1 + s2;
      int[] mergedArr = new int[totalSize];
      int i1 = 0, i2=0, i3=0;

      while(i1 < s1 && i2 <s2) {
        if(arr1[i1] < arr2[i2]) {
          mergedArr[i3++] = arr1[i1++];
        } else {
          mergedArr[i3++] = arr2[i2++];
        }
      }

      while(i1 < s1) {
        mergedArr[i3++] = arr1[i1++];
      }

      while(i2 < s2) {
        mergedArr[i3++] = arr2[i2++];
      }

      return mergedArr;
    }

```

#### Exercise 3

- Find Two Numbers that Add up to "n"

1. Brute force

```java
int[] findSum(int[] arr, int n)

// input
arr = {1, 21, 3, 14, 5, 60, 7, 6}
value = 27

// output
arr = {21, 6} or {6, 21}

int[] answer = new int[2];

    for(int i = 0; i < arr.length; i++) {
      for(int j = 1; j < arr.length; j++) {
        if(arr[i] + arr[j] == n) {
          answer[0] = arr[i];
          answer[1] = arr[j];
          return answer;
        }
      }
    }

    return arr;
```

2. Sorting the array (Quick sort)

#### Quick sort

- Recursive
- Pivot: 배열의 요소 중 하나이며 정렬 후의 3가지 조건을 의미함

##### How to choose the Pivot => middle of the array

#### Pivot 의 3가지 조건

1. 최종 정렬된 배열의 correct position 에 있다.
2. 왼쪽의 모든 항목이 더 작고
3. 오른쪽의 모든 항목이 더 크다

- Array = [2, 6, 5, 3, 8, 7, 1, 0]

  1. 3 을 Pivot 으로 잡고 배열의 끝으로 이동하여 Pivot 을 제거
     [2, 6, 5, 0, 8, 7, 1, 3]
  2. (itemFromLeft) Pivot 보다 큰 왼쪽에서 시작 하는 첫 번째 요소 (6)
  3. (itemFromRight) Pivot 보다 작은 오른쪽에 위치하는 요소 (1)
  4. Swap itemFromLeft with itemFromRight
     [2, '1', 5, 0, 8, 7, '6', 3]
  5. 반복
     (itemFromLeft) = 5
     (itemFromRight) = 0
     [2, 1, '0', '5', 8, 7, 6, 3]
  6. 종료 조건
     itemFromLeft 인덱스가 종 itemFromRight 인덱스 보다 큼
     (itemFromLeft) = 5 (index is '4')
     (itemFromRight) = 0 (index is '3')
  7. Swap itemFromLeft with Pivot
     [2, 1, 0, '3', 8, 7, 6, '5']
  8. Pivot 의 3 가지 조건 확인

  - 최종 정렬된 배열의 correct position 에 있다 (v)
  - 왼쪽의 모든 항목이 더 작고 (v)
  - 오른쪽의 모든 항목이 더 크다 (v)

  9. Recursive

  - 더 큰 파티션으로 퀵 정렬 다시 수행
    => [8, 7, 6, 5]

    1. 7 을 Pivot 으로 잡고 배열의 끝으로 이동하여 Pivot 을 제거
       [8, '5', 6, '7']
    2. (itemFromLeft) Pivot 보다 큰 왼쪽에서 시작 하는 첫 번째 요소 (8)
    3. (itemFromRight) Pivot 보다 작은 오른쪽에 위치하는 요소 (6)
    4. Swap itemFromLeft with itemFromRight
       ['6', 5, '8', 7]
    5. 반복
    6. 종료 조건
       itemFromLeft 인덱스가 종 itemFromRight 인덱스 보다 큼
       (itemFromLeft) = 8 (index is '2')
       (itemFromRight) = 5 (index is '1')
    7. Swap itemFromLeft with Pivot
       [6, 5, '7', '8']
    8. Pivot 의 3 가지 조건 확인
       - 최종 정렬된 배열의 correct position 에 있다 (v)
       - 왼쪽의 모든 항목이 더 작고 (v)
       - 오른쪽의 모든 항목이 더 크다 (v)

##### Now, you can understand the concept => Do recurtion to handle the rest of it.

```java

// swap

class QuickSort {
    private static void swap(int []arr, int a, int b){
		int tmp = arr[a];
		arr[a] = arr[b];
		arr[b] = tmp;
	}

	public static void motQsort(int []arr, int front, int rear){
		int i, j, pivot, mid = front+(rear - front)/2;
		threeSort(arr, front, mid, rear);
		if(rear - front + 1 > 3){
			pivot = arr[mid];
			swap(arr, mid, rear - 1);
			i = front;
			j = rear - 1;
			while(true){
				while(arr[++i]<pivot && i<rear);
				while(arr[--j]>pivot && front<j);
				if(i >= j) break;
				swap(arr, i, j);
			}
			swap(arr, i, rear - 1);
			motQsort(arr, front, i - 1);
			motQsort(arr, i + 1, rear);
		}
	}
	private static void threeSort(int []arr, int front, int mid, int rear){
		if(arr[front]>arr[mid]) swap(arr, front, mid);
		if(arr[mid]>arr[rear]) swap(arr, mid, rear);
		if(arr[front]>arr[mid]) swap(arr, front, mid);
	}
}
```
