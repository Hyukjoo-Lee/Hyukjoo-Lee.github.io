---
title: "{ Java Algorithms } Linked List"
date: 2022-11-20 14:00:00 +07:00
tags: [Algorithm, Linked List, Coding Interview, java]
---

### Linked List

- A linear data structure consising of nodes where each node contains data and a pointer to the next node in the list
- Singly Linked List
- Doubly Linked List

### List

- 어떤 '순서' 가 있는 데이터의 집합 (1등부터 차례차례 들어온 경마들의 기록)
- Array List
  1. '연속적인' 공간에 순차적으로 데이터를 저장
  2. Indexing 가능
- Linked List
  1. '비연속적인' 공간에 순서대로 데이터를 저장

## Singly Linked List

- Can only be traversed in one direction, from the first node (head) towards the last node
- Last node points to NULL (end of list)

### Pros

- Insertion and deletion at head is more efficient than array
- 추가, 삭제가 쉬움 <--> Array List
  e.g. 1 -> 4 -> 5 -> 11 -> 22 -> 7 인 linked list 에서 5와 11 번 사이에 9를 추가 하려면 5의 포인터가 9를 가리키게 하며, 9의 포인터가 11번을 가리키게 하면 된다.
- 반면 어레이는 하나씩 뒤로 옮겨줘야 함

### Cons

- 위치 탐색이 오래 걸린다 (from the head to the position) = Indexing 응 통해서 해당 노드에 바로 접근 불가능
- Array List 에 비하여 메모리를 더 차지함 (포인터를 저장할 공간이 따로 요구되기 때문에)
- Reverse traversing is not possible with singly linked list

## Doubly Linked List

- 각 노드에는 Next 노드를 가리키는 포인터와 Previous 노드를 가리키는 포인터가 있음
- Linked list 의 모든 장점 + 2 가지 장점

### Pros

- head as well as tail are O(1) operations
- can be traversed forward as well as backward

### Cons

- Greater space overhead than singly-linked list

### Singly Linked List Operations

- insertAtHead(data)
- insertAtEnd(data)
- delete(data)
- deleteAtHead()
- deleteAtEnd()
- Search(data)
- isEmpty() is a helper function which will prove useful in defining all the other functions

- Note: Even when a linked list is empty, the head Node must always exist.

```java

    public T data;
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

    public void insertAtHead(T data) {
        // Create a new node and assign the data value
        Node newNode = new Node();
        newNode.data = data;
        newNode.nextNode = headNode;
        headNode = newNode;
        size++;
    }

```

### Implementation

```java
    public static void main( String args[] ) {
        SinglyLinkedList<Integer> sll = SinglyLinkedList<Integer>();
        for (int i = 1; i <= 10; i++) {
			sll.insertAtHead(i);
			sll.printList();
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
        Node newNode = new Node();
        newNode.data = data;
        newNode.nextNode = null;

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
