---
description: Concurrency Model and Event loop in Javascript
---

# Event Loop

Below video is has the best explanation of overview of event loop. This video doesn't focus on multiple queue which are available instead only focuses on `macro-queue` and ignores micro-queue.

{% embed url="https://www.youtube.com/watch?v=8aGhZQkoFbQ" %}

Below video explains optimizations that ecmascript engine performs \(in this case v8\).

{% embed url="https://www.youtube.com/watch?v=p-iiEDtpy6I&t=795s" %}



If you consider the full browser environment, you should also consider queues for rendering, dom parsing, etc.

Ref: [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/EventLoop)

JavaScript has a concurrency model based on an event loop, which is responsible for executing the code, collecting and processing events, and executing queued sub-tasks.

