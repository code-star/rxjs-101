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
const locationUpdates =
    Observable.webSocket('ws://some-live-shiplocation-api')

locationUpdates
    .subscribe(newShipLocation => {
        // update UI with new location i.e.
        this.state.shiplocation = newShipLocation
    })

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
// Misschien geen Subjects laten zien in onze voorbeelden? Gaan we vragen over krijgen.
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
    * `NgRx` // Dit is Redux voor Angular. Niet echt relevant voorbeeld.
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

---

### So, the RxJS Contract

* Observable
* Operators
* Subscribers
* Subscription
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
var obs = Observable.from([1, 2, 3]);

obs.subscribe(
    next => { console.log(next); },
    error => { },
    complete => { console.log('I\'m done!'); }
)

// What does this do?
```

* A subscriber activates an Observable. <!-- .element: class="fragment" -->
* Within a subscriber you handle every value, an error or when it's done. <!-- .element: class="fragment" -->

----

#### Subscription

```js
var obs = Observable.from([1, 2, 3])
                    .delay(1);

var subscription = obs.subscribe(
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
* This is done using the `pipe()` method, introduced in RxJS 6.0.0.
// Vraagje: Ik weet eigenlijk niet hoe de `pipe` method werkt als je geen Typescript gebruikt? Moeten we dit noemen?

```js
var obs = Observable.from([1, 2, 3])
		.pipe(map(x => x * 2));

obs.subscribe(
    next => { console.log(next); }, // 2, 4, 6
    error => { },
    complete => { console.log('I\'m done!'); }
)
```
<!-- .element: class="fragment" -->

* There's multiple categories of operators. <!-- .element: class="fragment" -->

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

cd rxjs-101
```

* Open the folder in your favorite IDE/text editor.

----

// Hebben we dit al voorbereid?
* Open index.html in your favorite browser (Chrome/Safari/Firefox recommended)

```sh
open index.html
```

* Check your console!

Note: // Todo, add tip for opening console fast. We're going to do it alot.

---

# Exercises

----

### Exercise #1

- Use `Observable.range()` to write your first Observable, and log its contents to your console.

```js
var range = Observable.range(1, 3); // Counts to `x`
```

- Use `Observable.of()` to return an `Array` of numbers and log its contents.

```js
var obs = Observable.of('single string');
```


----

```js
var range = Observable.range(1, 3);
// Range generates an Observable that counts from 1 to X.

range.subscribe(
    x => console.log(x);
);
```

*  Observable does nothing until it has a <br /> 📥 `subscriber`.

----

```js
var obs = Observable.of([1, 2, 3]);

obs.subscribe(
    x => console.log(x); // [1, 2, 3]
);
```

*  Observable does nothing until it has a <br /> 📥 `subscriber`.
* `of()` returns the `array` as a whole, not per value.
