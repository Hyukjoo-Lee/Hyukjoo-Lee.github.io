---
title: "{ Java Algorithms } Linked List"
date: 2023-01-03 14:00:00 +07:00
tags: [Algorithm, Linked List, Coding Interview, java]
---

### Linked List

- A linear data structure consists of nodes where each node contains data and a pointer to the next node in the list
- Singly Linked List
- Doubly Linked List

### List

- 어떤 '순서' 가 있는 데이터의 집합 (1등부터 차례차례 들어온 경마들의 기록)
- Array List
  - '연속적인' 공간에 순차적으로 데이터를 저장
  - Indexing 가능
- Linked List
  - '비연속적인' 공간에 순서대로 데이터를 저장

## Singly Linked List

- Can only be traversed in one direction, from the first node (head) towards the last node
- Last node points to NULL (end of list)

### Pros

- Insertion and deletion at head is more efficient than array
- 추가 & 삭제가 쉬움 &larr; &rarr; Array List
  - 1 &rarr; 4 &rarr; 5 &rarr; 11 &rarr; 22 &rarr; 7 인 linked list 에서 5와 11 번 사이에 9를 추가 하려면 5의 포인터가 9를 가리키게 하며, 9의 포인터가 11번을 가리키게 하면 된다.
- 반면 어레이는 하나씩 뒤로 옮겨줘야 함

### Cons

- 위치 탐색이 오래 걸린다 (from the head to the position) = Indexing 을 통해서 해당 노드에 바로 접근 불가능
- Array List 에 비하여 메모리를 더 차지함 (포인터를 저장할 공간이 따로 요구되기 때문에)
- Reverse traversing is not possible with singly linked list

## Doubly Linked List

- 각 노드에는 Next 노드를 가리키는 포인터와 Previous 노드를 가리키는 포인터가 있음
- Linked list 의 모든 장점 + 2 가지 장점

### Pros

- Head as well as tail are O(1) operations
- Can be traversed forward as well as backward

### Cons

- Greater space overhead than singly-linked list

### Singly Linked List Operations

- insertAtHead(data)
- insertAtEnd(data)
- delete(data)
- deleteAtHead()
- deleteAtEnd()
- Search(data)
- isEmpty() is a helper function which will prove useful in defining all the other functions.
- Note: Even when a linked list is empty, the head Node must always exist.

```java
    // Node object has data
    public T data;
    // And reference of nextNode
    public Node nextNode;

    public Node headNode; // head node of the linked list
    public int size;      // size of the linked list

    //Constructor - initializes headNode and size
    headNode = null;
    size = 0;

    public boolean isEmpty() {
        if (headNode == null) return true;
        return false;
    }

```

### Insertion in a singly linked list

- Insertion at Head (insertAtHead()): means that we are inserting the first element in the list
- Insertion at End (insertAtEnd()): means that we are inserting a new element as the last element of the list
- Insertion After: means that we specify the node (n), after which we are inserting the new node
  1. We traverse the linked list to find n
  2. As soon as we find it, we are assigning n's nextElement to the new node's nextElement
  3. We point n's nextElement to the new node

```java

    public T data;
    public Node nextNode;

    public Node headNode; // head node of the linked list
    public int size;      // size of the linked list

    //Constructor - initializes headNode and size
    headNode = null;
    size = 0;

    // insert at head
    public void insertAtHead(T data) {

      if(isEmpty()) {
        insertAtHead(data);
        return;
      }

      // insert data at head
      Node newNode = new Node();
      newNode.data = data;

      // 기존 head 를 newNode 의 nextNode 로 linking
      newNode.nextNode = headNode;

      // head 노드는 new node 로 replace 됨
      headNode = newNode;
      size++;
    }

    public void insertAtEnd(T data) {

    if(isEmpty()) {
        insertAtHead(data);
        return;
      }

      Node newNode = new Node();
      newNode.data = data;
      newNode.nextNode = null;

      // we can use a loop to reach the tail of the list and set our new node as the nextNode of the last node.

      // 리스트의 끝에 새로운 노드를 연결 시키기 위하여 loop 을 사용하여 last node 를 찾은 후 새로운 노드를 연결시킴

      // initialize the last node
      Node last = headNode;

      while(last.nextNode != null) {
        last = last.nextNode;
      }

      last.nextNode = newNode;
      size++;
    }

    public void insertAfter (Node prevNode, T data) {
      if(isEmpty()) {
        insertAtHead(data);
        return;
      }

      Node newNode = new Node();
      newNode.data = data;

      if(prevNode == null) {
        System.out.println("The given previous node cannot be null.");
      }

      // 기존에 위치한 노드의 다음 노드는 새로운 노드의 다음 노드가 되고
      newNode.nextNode = prevNode.nextNode;
      // 새로운 노드는 전의 노드의 다음 element 가 된다
      prevNode.nextNode = newNode;
    }


```

### Implementation

```java
package practice_11_17;

public class SinglyLinkedList<T> {
    //Node inner class for SLL
    public class Node {
        public T data;
        public Node nextNode;

    }

    //head node of the linked list
    public Node headNode;
    public int size;

    //constructor
    public SinglyLinkedList() {
        headNode = null;
        size = 0;
    }

     public Node getHeadNode() {
        return headNode;
    }

    public void setHeadNode(Node headNode) {
        this.headNode = headNode;
    }

    public int getSize() {
        return size;
    }

    public void setSize(int size) {
        this.size = size;
    }

    public boolean isEmpty() {

        if (headNode == null) return true;
        return false;
    }

    //Inserts new data at the start of the linked list
    public void insertAtHead(T data) {
        //Creating a new node and assigning it the new data value
        Node newNode = new Node();
        newNode.data = data;
        //Linking head to the newNode's nextNode
        newNode.nextNode = headNode;
        headNode = newNode;
        size++;
    }

    //Inserts new data at the end of the linked list
    public void insertAtEnd(T data) {
        //if the list is empty then call insertATHead()
        if (isEmpty()) {
            insertAtHead(data);
            return;
        }
        //Creating a new Node with value data
        Node newNode = new Node();
        newNode.data = data;
        newNode.nextNode = null;

        Node last = headNode;
        //iterate to the last element
        while (last.nextNode != null) {
            last = last.nextNode;
        }
        //make newNode the next element of the last node
        last.nextNode = newNode;
        size++;
    }

    //inserts data after the given prev data node
    public void insertAfter(T data, T previous) {

        //Creating a new Node with value data
        Node newNode = new Node();
        newNode.data = data;
        //Start from head node
        Node currentNode = this.headNode;
        //traverse the list until node having data equal to previous is found
        while (currentNode != null && currentNode.data != previous) {
            currentNode = currentNode.nextNode;
        }
        //if such a node was found
        //then point our newNode to currentNode's nextElement
        if (currentNode != null) {
            newNode.nextNode = currentNode.nextNode;
            currentNode.nextNode = newNode;
            size++;
        }
    }

    public void printList() {
        if (isEmpty()) {
            System.out.println("List is Empty!");
            return;
        }

        Node temp = headNode;
        System.out.print("List : ");

        while (temp.nextNode != null) {
            System.out.print(temp.data.toString() + " -> ");
            temp = temp.nextNode;
        }

        System.out.println(temp.data.toString() + " -> null");
    }

    //Searches a value in the given list.
    public boolean searchNode(T data) {
        //Start from first element
        Node currentNode = this.headNode;

        //Traverse the list till you reach end
        while (currentNode != null) {
            if (currentNode.data.equals(data))
                return true; //value found

            currentNode = currentNode.nextNode;
        }
        return false; //value not found
    }

    //Deletes data from the head of list
    public void deleteAtHead() {
        //if list is empty then simply return
        if (isEmpty())
            return;
        //make the nextNode of the headNode equal to new headNode
        headNode = headNode.nextNode;
        size--;
    }

    //Deletes data given from the linked list
    public void deleteByValue(T data) {
        //if empty then simply return
        if (isEmpty())
            return;

        //Start from head node
        Node currentNode = this.headNode;
        Node prevNode = null; //previous node starts from null

        if(currentNode.data.equals(data)) {
            //data is at head so delete from head
            deleteAtHead();
            return;
        }
        //traverse the list searching for the data to delete
        while (currentNode != null) {
            //node to delete is found
            if (data.equals(currentNode.data)){
                prevNode.nextNode = currentNode.nextNode;
                size--;
                return;
            }
            prevNode = currentNode;
            currentNode = currentNode.nextNode;
        }
    }
}
```

### Exercise 1

- Insertion in a Singly Linked List (insert at End)

```java
void insertAtEnd(T data)
// input
linkedlist = 0->1->2
data = 3

// output
linkedlist = 0->1->2->3

// explaination - loop 을 사용하여 목록의 끝에 도달 후 새 노드를 마지막 노드의 다음 노드로 설정

        // Create a new node
        Node newNode = new Node();
        // Put the data
        newNode.data = data;
        // Set nextNode as null
        newNode.nextNode = null;

        // Note that we insert a data at the end of nodes
        Node last = headNode;

        // iterate to the last element
        while(last.nextNode != null) {
            last = last.nextNode;
        }

        // set newNode the next element of the last node
        last.nextNode = newNode;
        size++;
    }
}
```

### Exercise 2

- Search in Singly Linked List

```java
// Searching for an element in a linked list

// head node 부터 시작해서 list 의 끝까지 search 를 진행

boolean searchNode(T data)

// Input
linkedlist = 5->90->10->4   and  value = 4

// Output
true

    // code
    if(isEmpty()) return false;

        Node last = headNode;

        while(last.nextNode != null) {
            if(last.data == data) {
                return true;
            } else {
                last = last.nextNode;
            }
        }
        return false; //value not found
    }
```

### Exercise 3

- Deletion in Singly Linked List(Delete by Value)

```java

// Delete by value

void deleteByValue(T data)


// Input
linkedlist = 3 -> 2 -> 1 -> 0,
data = 1

// Output
linkedlist = 3 -> 2 -> 0

public void deleteByValue(T data) {
        // if empty then simply return
        if(isEmpty()) return;

        // Start from head node
        Node currentNode = headNode;
        Node previousNode = null;

        // Data is at head so delete from head
        if(currentNode.data.equals(data)) {
            deleteAtHead();
            return;
        }

        // Traverse the list searching for the data to delete
        while(currentNode != null) {
            //node to delete is found
            if(currentNode.data == data) {
              // links the prevNode to the nextNode
              previousNode.nextNode = currentNode.nextNode;
              size--;
              return;
            }

            previousNode = currentNode;
            currentNode = currentNode.nextNode;
        }
        }
    }
}

```

### Singly Linked Lists vs Arrays

#### Memory Allocation

- 어떻게 그들이 저장이 되는지에 차이점이 있음
- 어레이는 전체 블록의 메모리를 initialize 한다 (어떠한 elements 가 없더라고 하더라도, 지정된 elements 들을 처음 부터 저장)
- 반면에 linked list 는 사용하는 메모리 portion 만 initialize
- 즉, 어레이는 초기에 크기를 define 해줘야 하고, linkedlist 는 define 를 요구하지 않음

#### Insertion and Deletion

- In a linked list, insertion / deletion at head or tail takes constant of time O(1)
- While, an array takes O(n) time; must shift the array elements left or right after the operation

#### Searching

- In a linked list, you must iterate the list from the head to tail searching the correct value O(n)
- While, an array take O(1) using the index

### Doubly Linked Lists

- Bi-directional advantages

```java
  public class Node<T> {
    public T data;
    public Node nextNode; // To store pointer to next element
    public Node prevNode; // To store pointer to prev element
  }

  // delete
```

```java
// Delete by value

        // If empty then simply return
        if (isEmpty())
            return;

        // Start from head node
        Node currentNode = this.headNode;

        if (currentNode.data.equals(data)) {
            // Data is at head so delete from head
            deleteAtHead();
            return;
        }

        // Traverse the list searching for the data to delete
        while (currentNode != null) {
            // Node to delete is found
            if (data.equals(currentNode.data)) {
                currentNode.prevNode.nextNode = currentNode.nextNode;
                if(currentNode.nextNode != null)
                    currentNode.nextNode.prevNode = currentNode.prevNode;
                    size--;
            }
            currentNode = currentNode.nextNode;
        }
    }

```

### Exercise 4

- Find the Length of a Linked List

```java

int length()

// Input
linkedlist = 0->1->2-3

// Output
length = 4

int count = 0;
Node currentNode = headNode;

  while(currentNode != null) {
    count++;
    currentNode = currentNode.nextNode;
  }

return count;
```

### Exercise 5

- Find the Middle Node of a Linked List

```java

void deleteByValue(T data)

public static <T> Object findMiddle(SinglyLinkedList<T> list)

// Input
linkedlist = 2->10->7->21

// Output
middle = 10


// Hint: 한 걸음 갈때, 두 걸음 앞서가면 항상 반밖에 도달하지 못한다는 것을 활용
current = current.nextNode.nextNode
middle = middle.nextNode

SinglyLinkedList.Node middle = list.headNode;
SinglyLinkedList.Node current = list.headNode;

  if(list.isEmpty()) {
  return null;
  }


  while(current != null && current.nextNode != null) {
    current = current.nextNode.nextNode;
    if(current != null) {
      middle = middle.nextNode;
    }
  }

return middle.data;
```

### Exercise 6

- Remove Duplicates from a Linked List

```java

	// This method will remove all elements that appears more than once in the list
    // It has two nested loop, time complexity is O(n^2)
	public static <T> void removeDuplicatedElement(SinglyLinkedList<T> list) {

		// Initialize the current node and compare node
		SinglyLinkedList<T>.Node current = list.getHeadNode();
		SinglyLinkedList<T>.Node compare = null;

		while (current != null && current.nextNode != null) {
			compare = current;
			while (compare.nextNode != null) {
				// Check if the current node.data is duplicated with the next node.data
				if (current.data.equals(compare.nextNode.data)) {
					// if it is duplicated, remove it
					compare.nextNode = compare.nextNode.nextNode;
				} else {
					// if not, compare the nextNode
					compare = compare.nextNode;
				}
			}
			// loop beginning with the next node
			current = current.nextNode;

		}
	}

```

### Exercise 7

- Union & Intersection of Lists (Union = 합집합, Intersection = 교집합)

```java

// input

list1 = 11->3->8->null
list2 = 7->14->3->null

// output

union = 11->3->8->7->14->null
intersection = 3->null


    // Traverse the first list to reach the last node(O(n)) and link the list2 to the last node of list1
	// And then call removeDuplicatesWithHashing() to remove duplicated elements in the merged list (a linear function - O(n+m))
	public static <T> SinglyLinkedList<T> union(SinglyLinkedList<T> list1, SinglyLinkedList<T> list2) {

		// get the head of the list1
		SinglyLinkedList<T>.Node last = list1.getHeadNode();

		// loop it to reach the last node
		while (last.nextNode != null) {
			last = last.nextNode;
		}

		// link the list2 to the last element of list1
		last.nextNode = list2.getHeadNode();
		// remove duplicated elements in the merged list
		list1.removeDuplicatesWithHashing();

		return list1;
	}


    // Traverse list1 in a loop which is O(n)
    // In each iteration, we call contains() which runs in O(m)
	public static <T> SinglyLinkedList<T> intersection(SinglyLinkedList<T> list1, SinglyLinkedList<T> list2) {
		// Initialize the result linked list
		SinglyLinkedList<T> result = new SinglyLinkedList<T>();
		// Initialize the current node
		SinglyLinkedList<T>.Node current = list1.getHeadNode();

		while(current != null) {
			// if the list2 contains the value in current node
			if(contains(list2, current.data)) {
				// insert it to the head of result list
				result.insertAtHead(current.data);
			}
			// next ++
			current = current.nextNode;
		}

		return result;
	}

	// This method will remove all elements that appears more than once in the list
	public static <T> void removeDuplicatedElement(SinglyLinkedList<T> list) {

		// Initialize the current node and compare node
		SinglyLinkedList<T>.Node current = list.getHeadNode();
		SinglyLinkedList<T>.Node compare = null;

		while (current != null && current.nextNode != null) {
			compare = current;
			while (compare.nextNode != null) {
				// Check if the current node.data is duplicated with the next node.data
				if (current.data.equals(compare.nextNode.data)) {
					// if it is duplicated, remove it
					compare.nextNode = compare.nextNode.nextNode;
				} else {
					// if not, compare the nextNode
					compare = compare.nextNode;
				}
			}
			// loop beginning with the next node
			current = current.nextNode;

		}
	}

	// This method will check if the list contains the value
    public static <T> boolean contains(SinglyLinkedList<T> list, T data) {
        SinglyLinkedList<T>.Node current = list.getHeadNode();
        while (current != null) {
            if (current.data.equals(data)) return true;
            current = current.nextNode;
        }
        return false;
    }

```

### Exercise 8

- Return the Nth node from End

```java
    // This method is to find the nth element from the end
    // first, get the size of list to access the length of list
    // find the node which is at x position from the start (n = size - n + 1)
    public static <T> Object nthElementFromEnd(SinglyLinkedList<T> list, int n) {
    	// get the size of list
    	int size = list.getSize();
    	// calculate the distance from start
    	n = size - n + 1;

    	SinglyLinkedList.Node current = list.getHeadNode();
    	int count = 1;

    	// traverse
    	while(current != null) {
    		if(count == n) return current.data;
    		count++;
    		current = current.nextNode;
    	}

    	return null;
    }


```

### Exercise 9

- Find if a Doubly Linked-list is a Palindrome

```java
    // Find if a Doubly Linked-list is a Palindrome
    public static <T> boolean isPalindrome(DoublyLinkedList<T> list) {
    	DoublyLinkedList<T>.Node start = list.getHeadNode();
    	DoublyLinkedList<T>.Node end = list.getTailNode();

    	while(start != null) {
    		if(start.data != end.data) return false;
    		start = start.nextNode;
    		end = end.prevNode;
    	}
    	return false;
    }

```

### Interview questions

- Before the creation of linkedlist, you need to know the size of the list (false)
- Basic information stored in class Node of a Singly linked list

```java
// The Node class contains data and pointer of Node type that points to next Node
class Node<T> {
    T data;
    Node nextNode
}
```

- An array has a fixed size, otherwise a linked list has dynamic size (you need to specify the fixed size during initialization)

- Traverse the list

```java
Node currentNode = list.headNode;
while(currentNode != null) {
    currentNode = currentNode.nextNode;
}

```

- Circular linked list = The last node in the linked list points to the first element
- Application of linked-list
  - Used to implement file systems
  - Used to implement hash tables
  - Used to implement adjacency lists
- The nodes of a linked list are not stored at consecutive locations in memory like the elements of an array. Instead, the nodes are linked together by the next pointers, with the last node having a next pointer that points to null
