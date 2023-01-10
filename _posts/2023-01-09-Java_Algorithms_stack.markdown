---
title: "{ Java Algorithms } Stack"
date: 2023-01-09 10:30:00 +07:00
tags: [Algorithm, Stack, Coding Interview, java]
---

### Stack

- 한 쪽 끝에서만 자료를 넣고 뺄 수 있는 LIFO (Last In First Out) 형식의 자료 구조
- Undo, store the previous states of your work
- A pile of books placed in a vertical order
- Stack runs perfectly for Depth First Search and Expression Evaluation Algorithm
- It used for the actions
  - To backtrack to the previous task/state, e.g., in a recursive code
  - To store a partially completed task, e.g., when you are exploring two different paths on a Graph from a point while calculating the smallest path to the target.

### Operations

- 스택(Stack)는 LIFO(Last In First Out) 를 따른다. 즉, 가장 최근에 스택에 추가한 항목이 가장 먼저 제거될 항목이다.
- pop(): 스택에서 가장 위에 있는 항목을 제거한다.
- push(item): item 하나를 스택의 가장 윗 부분에 추가한다.
- datatype top(): 스택의 가장 위에 있는 항목을 반환한다.
- isEmpty(): 스택이 비어 있을 때에 true를 반환한다.
- isFull: Returns true if the stack is full and false otherwise

### Implementaion

```java
public class Stack <V> {
    private int maxSize;
    private int top;
    private V array[];

    /*
    Java does not allow generic type arrays. So we have used an
    array of Object type and type-casted it to the generic type V.
    This type-casting is unsafe and produces a warning.
    Comment out the line below and execute again to see the warning.
    */
    @SuppressWarnings("unchecked")
    public Stack(int max_size) {
        this.maxSize = max_size;
        this.top = -1; //initially when stack is empty
        array = (V[]) new Object[max_size];//type casting Object[] to V[]
    }

    //returns the maximum size capacity
    public int getMaxSize() {
        return maxSize;
    }

    //returns true if Stack is empty
    public boolean isEmpty(){
        return top == -1;
    }

    //returns true if Stack is full
    public boolean isFull(){
        return top == maxSize -1;
    }

    //returns the value at top of Stack
    public V top(){
        if(isEmpty())
            return null;
        return array[top];
    }
}

```

### Queue

- Similar to Stack, Queue is the another linear data structure that stores elements in a sequential manner.
- Stack stores elements LIFO / Queue stores elements FIFO (First In First Out)
- Queue is quite more tricky than Stack, we have to track both on elements (front and end) in an array
- Example
  - Line of the people to get a ticket from the booth
  - Store packets on routers: packets - 데이터 전송에서 송신측과 수신측에 의하여 하나의 단위로 취급되어 전송되는 집합체
- Prioritize something over another
- 다른 기기들 사이에서 공유되는 리소스 (Web Servers and Control Units)

### Operations

### Implementation

- enqueue: inserts element to the 'end' of the queue
- dequeue: removes an element from the 'start' of the queue
- top: returns the 'first' element of the queue
- isEmpty: checks if the queue is empty
- isFull: checks if the queue is full

```java
//  5 data members
public class Queue<V> {
    private int maxSize;
    private V[] array;
    private int front;
    private int back;
    private int currentSize;

    @SuppressWarning("unchecked")
    public Queue(int maxSize) {
        this.maxSize = maxSize;
        array = (V[]) new Object[maxSize]
        front = 0;
        back = -1;
        currentSize = 0;
    }

    public int getMaxSize() {
        return maxSize;
    }

    public int getCurrentSize() {
        return currentSize;
    }

    // Helper Functions
    public boolean isEmpty() {
        return currentSize == 0;
    }

    public boolean isFull() {
        return currentSize == maxSize;
    }

    public V top() {
        return array[front];
    }

    public void enqueue(V value) {
        if(isFull()) return;
        // to keep the index in range
        back = (back + 1) % maxSize;
        array[back] = value;
        currentSize++;
    }

    public void dequeue(V value) {
        if(isEmpty()) return null;

        V temp = array[front];
        front = (front + 1) % maxSize;
        currentSize--;

        return temp;
    }
}
```

### Types of Queue

#### Linear Queue: we discussed til now

#### Circular Queue

- Both ends are connected to form a circle
- Used for..
  - Simulation of objects
  - Event handling (do something when a particular event occurs)

#### Priority Queue

- The most prioritized element is stored on the front on queue
- The least prioritized element is stored on the last on queue
- Operating System: which programs should be operated first

### Exercise 1

- Implement Two Stacks using one Array

```java
// The first stack starts from the first element at index 0
// the second starts from the last element.
// Solution - Stacks on opposite ends
class TwoStacks<V> {
    private int maxSize
    private int top1, top2;
    private V[] array;

    @SuppressWarnings("unchecked")
    public TwoStacks(int max_size) {
        this.maxSize = max_size;
        this.top1 = -1;
        this.top2 = max_size;
        array = (V[]) new Object[max_size];//type casting Object[] to V[]
    }

    //insert at top of first stack
    public void push1(V value) {
        if(top1 < top2 -1) {
            array[++top1] = value;
        }
    }

    //insert at top of second stack
    public void push2(V value) {
        if(top1 < top2 -1) {
            array[--top2] = value;
        }
    }

    //remove and return value from top of first stack
    public V pop1() {
        // last out
        if(top1 > -1) {
            return array[top1--];
        }
        return null;
    }

    //remove and return value from top of second stack
    public V pop2() {
        // last out
        if(top2 < maxSize) {
            return array[top2++];
        }
        return null;
    }
}

```

### Exercise 2

- Reversing the First k Elements of a Queue

```java

void reverseK(Queue<V> queue, int k)

// input
Queue = {1,2,3,4,5,6,7,8,9,10}
k = 5

// output
result = {5,4,3,2,1,6,7,8,9,10}

    if (queue.isEmpty() || k <= 0) return;

    Stack<V> stack = new Stack<>(k)

    // Push first k elements in queue into a stack
    while(!stack.isFull())
        stack.push(queue.dequeue())

    // Pop stack elements and enqueue at the end of queue
    while(!stack.isEmpty())
        queue.enqueue(stack.pop())

    int size = queue.getCurrentSize();

    // append them at end of the queue
    for(int i = 0; i < size - k; i++)
        queue.enqueue(queue.dequeue());

```

### Exercise 3

- Implement queue using stack

```java

// Use two stack, stack1 stores the origin values
// stack2 helps stack1 to store the opposite values

Stack<V> stack1;
Stack<V> stack2;

public QueueWithStack(int maxSize) {
    stack1 = new Stack<>(maxSize);
    stack2 = new Stack<>(maxSize);
}

public boolean isEmpty(){
    return stack1.isEmpty();
}
public void enqueue(V value){
    stack1.push(value);
}

// FIFO => LIFO
public void dequeue(V value) {

    Stack<V> stack1;
    Stack<V> stack2;

    public QueueWithStack(int maxSize){
        stack1 = new Stack<>(maxSize);
        stack2 = new Stack<>(maxSize);
    }

    public boolean isEmpty(){
        return stack1.isEmpty();
    }

    public void enqueue(V value){
        stack1.push(value);
    }

    public V dequeue(){

        // pop all elements in stack1 and push them into stack2 (reversed)
        while (!stack1.isEmpty()){
            stack2.push(stack1.pop());
        }

        // pop from stack2 and return it
        V result = stack2.pop();

        // pop all elements back in stack1
        while (!stack2.isEmpty()){
            stack1.push(stack2.pop());
        }

        return result;
    }

```

### Exercise 4

- Sort the Values in a Stack

```java

void sortStack(Stack<Integer> stack)

// input
stack = {23,60,12,42,4,97,2}

// output
result = {2,4,12,23,42,60,97}

public static void sortStack(Stack<Integer> stack) {
    // 1. use a second tempStack.
    Stack<Integer> tempStack = new Stack<>(stack.getMaxSize());
    // 2. pop value from mainStack.
    while(!stack.isEmpty()) {
        int value = stack.pop();
    // 3. if the value is greater or equal to the top of tempStack, then push the value in tempStack
        if(!tempStack.isEmpty() && value >= tempStack.top()) {
            tempStack.push(value);
        }
         else {
            // else pop all values from tempStack and push them in mainStack
            while(!tempStack.isEmpty() && tempStack.top() > value)
                stack.push(tempStack.pop());
                // in the end push value in tempStack and repeat from step 2 until mainStack is not empty
                tempStack.push(value);
         }
    }

    // 4. when mainStack will be empty, tempStack will have sorted values in descending order.
    // 5. now transfer values from tempStack to mainStack to make values sorted in ascending order.
    while(!tempStack.isEmpty()) {
        stack.push(tempStack.pop());
    }
}

```

### Exercise 5

- Evaluate Postfix Expressions using Stacks

- Infix and Postfix Expressions
  - infix : operators like + and \* appear between the operands
  - postfix : operators appear after the operands / 변형방법: 괄호로 묶어서 괄호를 하나씩 지우면서 연산자를 뒤로 보냄

```java

// input
expression = "921*-8-4+" //  9 - 2 * 1 - 8 + 4

// output
result = 3
public static int evaluatePostFix(String expression) {

    Stack<Integer> stack = new Stack<>(expression.length());

    // 1.scan expression character by character,
    for(int i = 0; i < expression.length(); i++) {
        char character = expression.charAt(i);

    // 2.if character is operator then pop two elements from stack,
    // perform the operation and put the result back in stack
        if (!Character.isDigit(character)) {
            int a = stack.pop();
            int b = stack.pop();

            switch (character) {
                case '+' :
                stack.push(b + a);
                break;
                case '-' :
                stack.push(b - a);
                break;
                case '*' :
                stack.push(b * a);
                break;
                case '/' :
                stack.push(b / a);
                break;
            }
    // 3. if character is a number push it in stack
        } else stack.push(Character.getNumericValue(character));
    }
    // 4. at the end, Stack will contain result of whole expression.
    return stack.pop();
}

```
