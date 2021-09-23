---
title: 'Disruptor'
layout: post
tags:
  - Disruptor
category: Disruptor
---
Disruptor。

<!--more-->

# 一、Disruptor

```java
//单线程模式，获取额外的性能
Disruptor<MessageDto> disruptor = new Disruptor<MessageDto>(MessageDto::new, 1024 * 32, Executors.defaultThreadFactory(), ProducerType.SINGLE, new YieldingWaitStrategy());
//创建默认异常
disruptor.setDefaultExceptionHandler(new DefaultExceptionHandler());
//设置事件业务处理器---消费者
SearchReportEventHandler[] searchReportEventHandlers = new SearchReportEventHandler[POOL_SIZE];
for (int i = 0; i < POOL_SIZE; i++) {
    SearchReportEventHandler bean = SpringUtil.getBean(SearchReportEventHandler.class);
    searchReportEventHandlers[i] = bean;
}
disruptor.handleEventsWithWorkerPool(searchReportEventHandlers);
disruptor.start();
```

