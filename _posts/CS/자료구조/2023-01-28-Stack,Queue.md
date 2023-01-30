---
title: Stack, Queue
categories: cs-ds
---

[1. Stack](#Stack)  
[2. Queue](#Queue)  
[3. Stack 으로 Queue 구현](#Stack-으로-Queue-구현)  
[4. Deque](#Deque)

## Stack
+ 선형 자료구조이다.
+ Last In First Out(LIFO) 나중에 들어간 원소가 먼저 나온다.
+ 차곡차곡 쌓이는 구조로 먼저 들어간 원소는 맨바닥에 깔리며 호출 시 가장 위에 있는 녀석이 호출되는 구조이다.

## Queue
+ 선형 자료구조이다.
+ First In First Out(FIFO) 먼저 들어간 원소가 먼저 나온다.
+ 먼저 들어간 놈이 맨앞에서 대기하고 있다가 먼저 나오게 되는 구조이다.

## Stack 으로 Queue 구현
+ 2개의 스택을 이용하여 구현
+ Enqueue(삽입)
    + 첫 번째 스택에만 추가한다.
+ Dequeue(제거)
    + 두 번째 스택이 비어있지 않다면 b에 있는 값을 제거(반환)하면 된다.
    + 두 번째 스택이 비었다면 첫 번째 스택이 빌 때까지 제거하면서 두 번째 스택에 추가한다.
  
```java
class StackQueue {
    Stack<Integer> a;
    Stack<Integer> b;
    
    public StackQueue() {
        this.a = new Stack<>();
        this.b = new Stack<>();
    }
    
    public void enqueue(int data) {
        a.push(data);
    }
    
    public int dequeue() {
        if (b.isEmpty()) {
            while (!a.isEmpty()) {
                b.push(a.pop());
            }
        }
        int data = b.pop();
        return data;
    }
}
```

## Deque
+ 양쪽 끝에서 삽입과 삭제가 모두 가능한 자료구조
+ 선형 자료 구조
+ Stack(LIFO), Queue(FIFO)처럼 활용이 가능하다.
+ Deque 의 시간 복잡도
  + 삽입, 삭제, 조회(TOP) : O(1)

### Deque 주요 명령어
+ addFirst : head 에 자료 삽입
+ addLast : tail 에 자료 삽입
+ removeFirst : head 의 자료 제거
+ removeLast : tail 의 자료 제거
+ peekHead : head 의 자료 반환 (제거 x)
+ peekTail : tail 의 자료 반환 (제거 x)

### Deque 구현
+ 배열과 리스트로 구현하는 두 가지 방법이 존재한다.
+ 일반적으로 배열로 구현하는 것이 속도가 빠르다.