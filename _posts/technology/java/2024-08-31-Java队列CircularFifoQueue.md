---
title: Java 队列 CircularFifoQueue<T> 使用手册
author: since2014
date: 2024-08-31 19:15:00 +0800
categories: [Technology, Java]
tags: [Java, CircularFifoQueue]
render_with_liquid: false
---

# CircularFifoQueue<T>

## 声明

```java
CircularFifoQueue<T> queue = new CircularFifoQueue<>(n);
```

上述代码声明了一个长度为 n 的队列`queue`

# 方法

## .size()

返回队列`queue`中当前元素的个数

## .maxSize()

返回队列`queue`中能容纳元素的最大个数，即示例中初始化的值 n

## .isAtFullCapacity()

返回队列`queue`是否元素已达到最大值，即是否 `.size() = n`

## .isEmpty()

返回队列`queue`是否没有任何元素，即是否 `.size() = 0`

## .clear()

清空队列中的元素

# 代码测试

```java
public static void main(String[] args) {
    CircularFifoQueue<BigDecimal> queue = new CircularFifoQueue<>(7);
    System.out.println("begin =>: queue.size() = " + queue.size() + ", queue.maxSize() = " + queue.maxSize()
            + ", queue.isAtFullCapacity() = " + queue.isAtFullCapacity() + ", queue.isEmpty() = " + queue.isEmpty()
            + ", queue.isFull() = " + queue.isFull());
    for(int i = 0; i < 10; i ++){
        queue.offer(new BigDecimal(i + 12.5));
        System.out.println("queue = " + JSONObject.toJSONString(queue));
        System.out.println("第 " + i + "次：" + "queue.size() = " + queue.size() + ", queue.maxSize() = " + queue.maxSize()
                + ", queue.isAtFullCapacity() = " + queue.isAtFullCapacity() + ", queue.isEmpty() = " + queue.isEmpty()
                + ", queue.isFull() = " + queue.isFull());
    }
    System.out.println("end =>：" + "queue.size() = " + queue.size() + ", queue.maxSize() = " + queue.maxSize()
            + ", queue.isAtFullCapacity() = " + queue.isAtFullCapacity() + ", queue.isEmpty() = " + queue.isEmpty()
            + ", queue.isFull() = " + queue.isFull());
    queue.clear();
    System.out.println("clear => ：" + "queue.size() = " + queue.size() + ", queue.maxSize() = " + queue.maxSize()
            + ", queue.isAtFullCapacity() = " + queue.isAtFullCapacity() + ", queue.isEmpty() = " + queue.isEmpty()
            + ", queue.isFull() = " + queue.isFull());

}
```

测试结果：

```txt
begin =>: queue.size() = 0, queue.maxSize() = 7, queue.isAtFullCapacity() = false, queue.isEmpty() = true, queue.isFull() = false
queue = [12.5]
第 0次：queue.size() = 1, queue.maxSize() = 7, queue.isAtFullCapacity() = false, queue.isEmpty() = false, queue.isFull() = false
queue = [12.5,13.5]
第 1次：queue.size() = 2, queue.maxSize() = 7, queue.isAtFullCapacity() = false, queue.isEmpty() = false, queue.isFull() = false
queue = [12.5,13.5,14.5]
第 2次：queue.size() = 3, queue.maxSize() = 7, queue.isAtFullCapacity() = false, queue.isEmpty() = false, queue.isFull() = false
queue = [12.5,13.5,14.5,15.5]
第 3次：queue.size() = 4, queue.maxSize() = 7, queue.isAtFullCapacity() = false, queue.isEmpty() = false, queue.isFull() = false
queue = [12.5,13.5,14.5,15.5,16.5]
第 4次：queue.size() = 5, queue.maxSize() = 7, queue.isAtFullCapacity() = false, queue.isEmpty() = false, queue.isFull() = false
queue = [12.5,13.5,14.5,15.5,16.5,17.5]
第 5次：queue.size() = 6, queue.maxSize() = 7, queue.isAtFullCapacity() = false, queue.isEmpty() = false, queue.isFull() = false
queue = [12.5,13.5,14.5,15.5,16.5,17.5,18.5]
第 6次：queue.size() = 7, queue.maxSize() = 7, queue.isAtFullCapacity() = true, queue.isEmpty() = false, queue.isFull() = false
queue = [13.5,14.5,15.5,16.5,17.5,18.5,19.5]
第 7次：queue.size() = 7, queue.maxSize() = 7, queue.isAtFullCapacity() = true, queue.isEmpty() = false, queue.isFull() = false
queue = [14.5,15.5,16.5,17.5,18.5,19.5,20.5]
第 8次：queue.size() = 7, queue.maxSize() = 7, queue.isAtFullCapacity() = true, queue.isEmpty() = false, queue.isFull() = false
queue = [15.5,16.5,17.5,18.5,19.5,20.5,21.5]
第 9次：queue.size() = 7, queue.maxSize() = 7, queue.isAtFullCapacity() = true, queue.isEmpty() = false, queue.isFull() = false
end =>：queue.size() = 7, queue.maxSize() = 7, queue.isAtFullCapacity() = true, queue.isEmpty() = false, queue.isFull() = false
clear => ：queue.size() = 0, queue.maxSize() = 7, queue.isAtFullCapacity() = false, queue.isEmpty() = true, queue.isFull() = false
```
# *参考来源*