# Web Fundamentals

## Concurrentcy Model and the Event Loop [[MDN doc]](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/EventLoop)
Javascript uses a single threaded runtime, but it has a concurrency model based on event loop.

### Glossary
- Stack: Function calls form a stack of frames. (Call stack)
- Heap: Objects are allocated in a heap which is just a name to denote a large (mostly unstructured) region of memory.
- Queue: A message(event) queue. The processing of functions continues until the stack is once again empty. Then, event loop will process the messages in queue one by one.

### Event Loop
It's called this name because how it was implemented.

```js
while (queue.waitForMessage()) {
    queue.processNextMessage()
}
```
- Each message is processed completely before any other message is processed in FIFO order.
- Messages are added anytime an event occurs and there's an event listener attached to it, otherwise the event is lost.

For example, calling `setTimeout(listener, minimalTime)` will produce a message with `listener` which will be executed after `minimalTime`, not a guaranteed time. It's because `setTimeout` message has to wait for other messages to be processed.