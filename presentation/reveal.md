## Welcome
### To RxJS 101 <!-- .element: class="fragment" -->

---
<img src="bjorn.jpg" width="100" style="border-radius:100%;">
#### Bjorn Schijff
<small>Frontend Software Engineer @ Politie /</small>
<img src="codestar.svg" height="30" style="border: 0; background-color: transparent; position: relative; top: -8px;">

<img src="martin.jpg" width="100" style="border-radius:100%;">
#### Martin van Dam
<small>Frontend Software Engineer @ Port of Rotterdam /</small>
<img src="codestar.svg" height="30" style="border: 0; background-color: transparent; position: relative; top: -8px;">

---

## What is RxJS?

* A better way to manage data and events within your app. <!-- .element: class="fragment" -->

---

## Why RxJS?

* Better readable code 🤓<!-- .element: class="fragment" -->
* Data flow 🌊<!-- .element: class="fragment" -->
* Easier and safer data transformations 🤖<!-- .element: class="fragment" -->
* Functional and Reactive (!) 🙌<!-- .element: class="fragment" -->

---

## What do you use RxJS for?

* Streaming data, (i.e. WebSocket) <!-- .element: class="fragment" -->
* Games <!-- .element: class="fragment" -->
* Communicating between application components <!-- .element: class="fragment" -->

----

#### Streaming data

```ts
const locationUpdates =
    webSocket('ws://some-live-shiplocation-api')

locationUpdates
    .subscribe(newShipLocation => {
        // update UI with new location i.e.
        this.state.shiplocation = newShipLocation
    })

```

----

#### Games

```ts
const ticks = interval(this.tickMs)
    .pipe(map() => 'tick'))
const frames = interval(this.fpsMs)
    .pipe(map() => 'frame'))
const seconds = interval(1000)
    .pipe(map() => 'second'))

this.update$ = merge(ticks, frames, seconds)
    .share()
```

----

#### Communicating between application components

Service
```ts
export class EventBusService {
    private events = new Subject<Event>();

    getEvents(): Observable<Event> {
        return this.events.asObservable();
    }

    sendEvent(event: Event): void {
        this.events.next(event);
    }
}
```

----

#### Communicating between application components

Component
```ts
export class Component {
    constructor(private eventsService: EventBusService) {
        this.eventsService.getEvents()
            .filter(event => event.type === 'InterestingEvent')
            .subscribe(this.handleEvents)
    }

    doAction(): void {
        this.eventsService.sendEvent(new TestEvent());
    }

    handleEvents(event: Event) { 
        //... do things 
    }
}
```

---

### So, RxJS?

* Used heavily by `Angular`
* Lot's of adoption in libraries like 
    * `Redux-observable`
    * `VueRx`
* Java/Scala also have their implementation.

#### 🤩 So plenty of stuff to have fun with! 🤩 <!-- .element: class="fragment" -->

---

### Disclaimer

All examples are based on RxJS v6

```js
// RxJS v5
import * as Rx from 'rxjs'
const observable = Rx.Observable.from(...)

// RxJS v6 - TypeScript
import { from } from 'rxjs'
const observable = from(...)

// RxJS v6 - JavaScript
const { from } = 'rxjs';
const observable = from(...)
```

---

# The RxJS Contract

---

### Wait, what's a contract?

* A set of rules agreed upon <!-- .element: class="fragment" -->
* Using the same language throughout <!-- .element: class="fragment" -->
* Ensures we're all using the same things for the same reasons. <!-- .element: class="fragment" -->

---

### So, the RxJS Contract

* Observable
* Subscribers
* Subscription
* Operators
* Subject

----

#### Observable

- Datasource* <!-- .element: class="small" -->
- You can `subscribe` to its contents, much like a newsletter.
- Unlike a newsletter, it won't send anything until at least someone has a `subscription`.
- You can use `operators` on it, to get the parts you are interested in.

----

#### Subscribers
```js
const obs = from([1, 2, 3]);

obs.subscribe(
    next => { console.log(next); },
    error => { },
    complete => { console.log('I\'m done!'); }
)

// What does this do?
```

* A subscriber activates an Observable. <!-- .element: class="fragment" -->
* Within a subscriber you handle every value, an error, or when it's done. <!-- .element: class="fragment" -->
* Without error handling, the subscription will end immediately <!-- .element: class="fragment" -->

----

#### Subscription

```js
const obs = from([1, 2, 3]).delay(1);

const subscription = obs.subscribe(
    next => { console.log(next); },
    error => { },
    complete => { console.log('I\'m done!'); }
)

subscription.unsubscribe();

// What do you think this does?
```

```js
// and what happens when we remove the delay()?
```
<!-- .element: class="fragment" -->

* A subscription let's you unsubscribe from the ~~newsletter~~ `Observable`.

----

#### Operators

* Operators are small operations you perform on top of your Observable. <!-- .element: class="fragment" -->
* You can use multiple operators to transform the data to your liking. <!-- .element: class="fragment" -->
* This is done using the pipe() method, introduced in RxJS v6 <!-- .element: class="fragment" -->
* There's multiple categories of operators. <!-- .element: class="fragment" -->

----

```js
const obs = from([1, 2, 3])
    .pipe(
        map(x => x * 2),
        filter(x => x < 4)
    );
    
obs.subscribe(
    next => { console.log(next); }, // 2, 4, I'm done!
    error => { },
    complete => { console.log('I\'m done!'); }
)
```

----

#### But wait, there's more! 🙀

* Subject <!-- .element: class="fragment" -->
    * BehaviorSubject
    * ReplaySubject
* Observers <!-- .element: class="fragment" -->

We may go into these later. <!-- .element: class="fragment" -->

⛔ This is advanced, you can forget about these now. <!-- .element: class="fragment" -->

---

## Let's tango! 💃

In your terminal: 

```sh
git clone https://github.com/code-star/rxjs-101.git

cd rxjs-101/exercises
```

* Open the folder in your favorite IDE/text editor.

```sh
code . 
```
For Visual Studio Code.

---

# Exercises
Part one

----

### Exercise #1
Use `range()` to write your first Observable, and log its contents to your console.

Remember that an Observable does nothing on its own!

```js
const { range } = rxjs;
const rangeObservable = range(1, 3);
// ...
```

---

# Operators

----

- We've talked about operators for a little bit.
- But there's more! 

----

* Transformation like `map()`
* Filtering like `filter()`
* Utility like `tap()`
* Error handling like `catchError()`
* Combination like `merge()`
* Flattening like `switch()`
* Multicasting like `share()`
* And plenty of combined operators, like `switchMap()` or `mergeMap()`

----

### `map()`
Transform the value to another value
```ts
const obs = from([1, 2, 3])
    .pipe(
        map(x => x * 2) // multiplies each item by 2
    );

// outputs: 2, 4, 6
```

----

### `filter()`
Filter out values you're not interested in

```ts
const obs = from([1, 2, 3])
    .pipe(
        filter(x => x > 2) // only use values that are higher than 2
    );

// outputs: 3
```

----

### `tap()`
Utility for doing side effects but it will not effect the actual data

```ts
const obs = from([1, 2, 3])
    .pipe(
        tap(x => console.log(x)) // logs the value to the console
    );

// outputs: 1, 2, 3
```

----

### `catchError()`
Handle errors and eventually return a fallback value

```ts
const obs = from([1, new Error('fail here!'), 3])
    .pipe(
        catchError((err, caught) => {
            return null
        })
    );

// outputs: 1, null, 3
```

----

### `merge()`
Merge multiple Observables together

```ts
const obs1 = from([1, 2, 3])
const obs2 = from(['a', 'b', 'c'])

merge(obs1, obs2)

// outputs: 1, 2, 3, 'a', 'b', 'c'
```

----

* There's about a 120 operators. 
* They're all on https://github.com/ReactiveX/rxjs/tree/master/src
* It's a bit scary at first, but don't be afraid to just go there and lookup what an operator does.

---

# Exercises
Part two

----

### Exercise #2
Use `filter()` to let the observable only return the numbers that are lower than 10

```js
const { from, pipe } = rxjs;
const { filter, map } = rxjs.operators;

const observable = from([1, 2, 4, 8, 16, 32]).pipe(
    // apply filtering here... //
)

observable.subscribe(value => {
    console.log(value) // should log the values 1, 2, 4, 8
})
```

----

### Exercise #3
Use `map()` to multiply the values by 2

```js
const { from, pipe } = rxjs;
const { filter, map } = rxjs.operators;

const observable = from([1, 2, 4, 8, 16, 32]).pipe(
    // apply transformations here... //
)

observable.subscribe(value => {
    console.log(value) // should log the values 2, 3, 8, 16, 32, 64
})
```

----

### Exercise #4
Combine `filter()`with `map()`.

Hint: use `typeof x === 'string'` and `toUpperCase()`

---

# Common mistakes
- By André Staltz

----

* Using Subjects too much. Use Observable.create!
* Using Observable.create too much. Use Creation operators
* Subscribing too much and unsubscribing too much
* Subject.next inside subscribe
* Subscribe inside subscribe

---

# Error handling
* By default, the Observable will finish on errors
* `catchError` operator will help handle errors

```js
const faultyArray = [0, new Error('Index 1 not found!'), 2]
const faultyObservable = from(faultyArray).pipe(
    catchError((err, caught) => {
        return someValueOnError
    })
)
```

---

# Exercises
Part three

----

### Exercise #5
A: There are two things that are going wrong in this code. Can you see what they are? Adjust the code to fix those problems.

Hint: Run the example first and check the console log.

B: Rewrite this code so you don't need a Subject anymore.

Hint: the `from` creator and the `map` operator could help here.

----

### Exercise #6
A part of this exercise is given business logic, but we need the Observable to complete.

Add operators to make sure we handle any errors given by the Observable so it completes again.

---

# Cold versus Hot
Cold Observable
```js
new Observable((observer) => {
    // 'private' producer
    const producer = timer(5000)
    producer.subscribe(...)
})
```

Hot Observable
```js
// 'public' producer for reusability
const producer = timer(5000)
new Observable((observer) => {
  producer.subscribe(...)
})
```

⛔️ Advanced stuff! Interested? Look up `share()` and its friends.<!-- .element: class="fragment" -->



---

# Exercises
Part four

----

### Exercise #7
Fix the bug! 🐛

----

### Exercise #8
Link the two observables to merge a Pokémon Observable with a Pokémon Moves Observable.

Hint; not all types have moves, and you only need (should have) 1 subscribe.

---

# Final review; 
- Reflect on how easy it can be to read RxJS code
- Functional programming !!!
- RxJS and Reactive programming is a mindset. Get in the habit of doing it and it'll get easier.

---

Questions? ⁉️

--- 

🙌 Thanks for your attention and attendance! 🙌