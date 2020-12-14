---
description: >-
  Implement a library similar to that of RxJS, a library for composing
  asynchronous and event-based programs by using observable sequences.
---

# Event Management API \(RxJS like\)

Note: Complete implementation is not necessary.

The essential concepts in RxJS which solve async event management are:

* **Observable:** represents the idea of an invokable collection of future values or events.
* **Observer:** is a collection of callbacks that knows how to listen to values delivered by the Observable.
* **Subscription:** represents the execution of an Observable, is primarily useful for cancelling the execution.
* **Operators:** are pure functions that enable a functional programming style of dealing with collections with operations like `map`, `filter`, `concat`, `reduce`, etc.
* **Subject:** is the equivalent to an EventEmitter, and the only way of multicasting a value or event to multiple Observers.
* **Schedulers:** are centralized dispatchers to control concurrency, allowing us to coordinate when computation happens on e.g. `setTimeout` or `requestAnimationFrame` or others.

### Goal:

Try to create a setup and primitives which can accommodate all the above requirements.

Implementing all the operators are not required, but the basic flow, should be such that it allows to extend the library with as many operators as required in the future, without making any changes to the base implementation.



#### Step 1: To get the candidate started:

* Implement an `Observable` and `Subscription` 
* Things to note: `Observable` should be lazy initialized

```javascript
const observable = new Observable(subscriber => {
  subscriber.next(1);
  subscriber.next(2);
  subscriber.next(3);
  setTimeout(() => {
    subscriber.next(4);
    subscriber.complete();
  }, 1000);
});
 
console.log('just before subscribe');
observable.subscribe({
  next(x) { console.log('got value ' + x); },
  error(err) { console.error('something wrong occurred: ' + err); },
  complete() { console.log('done'); }
});
console.log('just after subscribe');

/* Logs:
just before subscribe
got value 1
got value 2
got value 3
just after subscribe
got value 4
done
*/
```

#### Step 2: Add streaming functionality to it

```javascript
const observable2 = observable.pipe(map(x => 2*x))

console.log('just before subscribe');
observable2.subscribe({
  next(x) { console.log('got value ' + x); },
  error(err) { console.error('something wrong occurred: ' + err); },
  complete() { console.log('done'); }
});
console.log('just after subscribe');

/* Logs
just before subscribe
got value 2
got value 4
got value 6
just after subscribe
got value 8
done
*/
```

#### Step 3: Add a helper function to auto-create observables \(e.g. `fromEvent`\)

```javascript
const elem = document.getElementbyId('btn')

fromEvent(elem, 'click').subscribe(() => {
   console.log('btn is clicked') // logs when the button is clicked
})
```

The candidate can choose to create any methods like `fromPromise` , `fromTimeout`, etc.

#### Step 4: Create a subject

```javascript
const subject = new Subject<number>();
 
subject.next(1);

subject.subscribe({
  next: (v) => console.log(`observerA: ${v}`)
});

subject.next(2)

subject.subscribe({
  next: (v) => console.log(`observerB: ${v}`)
});
 
subject.next(3);
subject.next(4);

// Logs:
// observerA: 2
// observerA: 3
// observerB: 3
// observerA: 4
// observerB: 4
```

#### Step 5: Extend the above functionality and implement a `BehaviorSubject`

```javascript
const subject = new BehaviourSubject()

subject.next(1);

subject.subscribe({
  next: (v) => console.log(`observerA: ${v}`)
});

subject.next(2)

subject.subscribe({
  next: (v) => console.log(`observerB: ${v}`)
});
 
subject.next(3);
subject.next(4);

// Logs:
// observerA: 1
// observerA: 2
// observerB: 2
// observerA: 3
// observerB: 3
// observerA: 4
// observerB: 4

```

