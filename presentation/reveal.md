## Welcome
### To RxJS 101 <!-- .element: class="fragment" -->
Introduction <!-- .element: class="fragment" -->

Note: Introduce ourselves. What's our experience with RxJS?

---

## What is RxJS?

* A better way to manage data and events within your app. <!-- .element: class="fragment" -->

---

## Why RxJS?

* Better readable code 🤓<!-- .element: class="fragment" -->
* Data flow 🌊<!-- .element: class="fragment" -->
* Easier and safer data transformations 🤖<!-- .element: class="fragment" -->
* Functional (!) 🙌<!-- .element: class="fragment" -->

---

## What do you use RxJS for?

* Streaming data, (i.e. WebSocket) <!-- .element: class="fragment" -->
* Games <!-- .element: class="fragment" -->
* Communicating between application components <!-- .element: class="fragment" -->

----

#### Streaming data

```ts
Pronto example
```

----

#### Games

```ts
const ticks = Observable.interval(this.tickMs)
    .map(() => 'tick')
const frames = Observable.interval(this.fpsMs)
    .map(() => 'frame')
const seconds = Observable.interval(1000)
    .map(() => 'second')

this.update$ = Observable.merge(ticks, frames, seconds)
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
    * `NgRx`
    * `VueRx`
    * ...
* Java/Scala also have their implementation.

#### 🤩 So plenty of stuff to have fun with! 🤩 <!-- .element: class="fragment" -->

---

# The RxJS Contract

---

### Wait, what's a contract?

* A set of rules agreed upon <!-- .element: class="fragment" -->
* Using the same language throughout <!-- .element: class="fragment" -->
* Ensures we're all using the same things for the same reasons. <!-- .element: class="fragment" -->

Note: // Todo

---

### So, the RxJS Contract

* Observable
* Operators
* Subscribers
* Subscription
* Subject

----

#### Observable

----

#### Operators

----

#### Subscribers

----

#### Subscription

----

#### Subject

---

## Let's tango! 💃

In your terminal: 

```sh
git clone https://github.com/code-star/rxjs-101.git

cd rxjs-101
```

* Open the folder in your favorite IDE/text editor.

----

* Open index.html in your favorite browser (Chrome/Safari/Firefox recommended)

```sh
open index.html
```

* Check your console!

Note: // Todo, add tip for opening console fast. We're going to do it alot.

---

# Exercises

----

### Exercises #1

- Use `Observable.range()` to write your first Observable, and log its contents to your console.
- Use `Observable.of()` to return an `Array` of numbers and log its contents.
- Use `Observable.create()`

----

```js
var range = Observable.range(3);
// Range generates an Observable that counts from 1 to X.

range.subscribe(
    x => console.log(x);
);
```