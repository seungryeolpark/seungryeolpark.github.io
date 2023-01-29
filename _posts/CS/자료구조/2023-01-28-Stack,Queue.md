---
title: Stack, Queue
categories: cs-ds
---

[1. Stack](#Stack)  
[2. Queue](#Queue)  
[3. Stack 으로 Queue 구현](#Stack-으로-Queue-구현)

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