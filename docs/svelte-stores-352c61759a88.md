# 苗条商店的辉煌

> 原文：<https://medium.com/geekculture/svelte-stores-352c61759a88?source=collection_archive---------18----------------------->

![](img/9e50a82d38ac006aee252dbc96af4dc6.png)

在我看来,[苗条](https://svelte.dev/)图书馆最有用的组成部分之一是*“商店”*。你可以在这里阅读官方文档[，但简而言之，Store 是一个反应性对象，在其中你*【subscribe】*从你的应用程序内部的任何组件状态变化。再者，店铺可以*【衍生】*。例如，商店值可以基于一个或多个其他商店实例的值。](https://svelte.dev/tutorial/writable-stores)

然而，对我来说，随着*“定制”*商店的创建，事情变得非常有趣。显然，Svelte 的创建者非常清楚这些商店的力量和灵活性，并提供了一个简单的界面来遵循。基本上，任何实现了*“subscribe”*方法的对象都是存储！

[Feed Army](https://feed.army/) 大量使用反应式存储，因为新的 Feed 数据通过 WebSockets 注入应用程序。stores 的反应性意味着当提要被更新时，显示逻辑可以在数据被使用时轻松地呈现新的提要内容。

在撰写本文时， [Feed Army](https://feed.army/) 在内部使用 x4 定制商店

1.  本地存储库
2.  地图商店
3.  对象存储
4.  队列存储

第一批 x3 存储(尤其是 Map & Object 存储)都扩展了原生 JS 类型。这很重要，因为你可以利用 JS 提供的内置和优化的方法。例如，一个新的 MapStore 实例有一个访问底层[映射的访问器。条目](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map/entries)方法。这使得像使用任何其他原生 JS 对象一样使用存储变得很容易。当然还有被动的额外好处。

然而，我想在这里与你分享的是一个简单的队列存储的内容。创建这样一个商店的原因是由于我对“当”“苗条”*“滴答”*以及在 DOM/UI 中迭代反应对象所做的一个假设。基本上，我的显示组件会*“跳过”新的 feed 条目，这并不理想，因此创建了一个队列。*

****这里* *可以参考苗条微任务 gotcha* [*。*](/geekculture/svelte-gotcha-the-reactive-microtasks-b27f00d53fb6)*

*首先，下面是一个相当简单/直接的队列类的示例内容(在 Typescript 中):*

```
*export interface QueueInterface {
  count(): number;
  dequeue?(): any;
  enqueue(...args: any): void;
  flush(): any[];
  reset(): void;
  setFifo(fifo: boolean): void;
  setLifo(lifo: boolean): void;
  truncate(length: number): void;
}export class queue {
  protected elements: any[];
  protected fifo = true;
  constructor(...args: any) {
    this.elements = [...args];
  }
  count() {
    return this.elements.length;
  }
  dequeue?(): any {
    if (this.fifo) {
      return this.elements.shift();
    }
    return this.elements.pop();
  }
  enqueue(...args: any) {
    return this.elements.push(...args);
  }
  // Like dequeue but will flush all queued elements
  flush(): any[] {
    let elms = [];
    while (this.count()) {
      elms.push(this.dequeue());
    }
    return elms;
  }
  setFifo(fifo = true) {
    this.fifo = fifo;
  }
  setLifo(lifo = true) {
    this.fifo = !lifo;
  }
  reset(): void {
    this.truncate(0);
  }
  truncate(length: number) {
    if (Number.isInteger(length) && length > -1) {
      this.elements.length = length;
    }
  }
}export default queue;*
```

*这并不奇怪，所以让我们看看这个类是如何通过我的 QueueStore 使用的:*

```
*import { writable, get } from "svelte/store";
import { queue } from "queue";export *interface* QueueStore {
  count(): *number*;
  dequeue?(): *any*;
  enqueue(...*args*: *any*): *void*;
  flush(): *any*[];
  reset(): *void*;
  self(): *object*;
  setFifo(*fifo*: *boolean*): *void*;
  setLifo(*lifo*: *boolean*): *void*;
  subscribe(*v*: *any*): Unsubscriber;
  truncate(*length*: *number*): *void*;
}export const newQueueStore = function () {
  let store = writable(new queue());
  return {
    count: () => get(store).count(),
    dequeue: () => {
      let q = get(store);
      const item = q.dequeue();
      store.set(q);
      return item;
    },
    enqueue: (...args: any) => {
      let q = get(store);
      q.enqueue(...args);
      store.set(q);
    },
    flush: () => {
      let q = get(store);
      const e = q.flush();
      store.set(q);
      return e;
    },
    reset: () => {
      let q = get(store);
      q.reset();
      store.set(q);
    },
    self: () => get(store),
    setFifo(fifo: boolean): void {
      let q = get(store);
      q.setFifo(fifo);
    },
    setLifo(lifo: boolean): void {
      let q = get(store);
      q.setLifo(lifo);
    },
    subscribe: store.subscribe,
    truncate: (length: number) => {
      let q = get(store);
      q.truncate(length);
      store.set(q);
    },
  } as QueueStore;
};*
```

*请注意，为了保持一致性，我总是尝试为底层对象创建访问器。因此，现在有了反应式存储，任何订阅者都会被通知任何队列更新，并可以相应地*“反应”*。如果需要，订户甚至可以获得底层队列类实例。例如:*

```
*const qs = newQueueStore();
qs.subscribe((q: QueueStore) => {
  while (q.count()) {
    const item = q.dequeue();
    console.log(item); // hello world
  }
  console.log(q === qs.self()); // true
});
qs.enqueue("hello world");*
```

*一个明确的 ***【喊出】*** 给这里苗条的创作者。最灵活和强大的！*

**原载于*[*https://www.kylehq.com*](https://www.kylehq.com/2021/06/the-brilliance-of-sveltejs-stores/)*。**