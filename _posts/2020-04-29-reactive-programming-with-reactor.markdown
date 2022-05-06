---
layout: post
title:  "Reactive programming with Reactor"
subtitle: "A core component for non-blocking asynchronous architecture"
author: "Stéphane Bégaudeau"
date: 2020-04-29 12:21:53 +0200
image: "/img/posts/2020/04/29/reactive-programming-with-reactor/preview.png"
include_obeo_rss: false
---

In Java, we are constantly manipulating various sequences of data in our applications.
Most of the time, this is done thanks to implementations of Iterable and Iterator.
Java 8 gave us more modern APIs for sequences of data with both Optional and Stream.
Those two new concepts provide us with great APIs to manipulate sequences of respectively 0..1 elements and 0..n elements.

```
Optional.of("first").ifPresent(System.out::println);
Stream.of("first", "second", "third").forEach(System.out::println);
```

While those APIs are great, they are, in a way, simply a better version of Iterable.
Just like Iterable, you can ask the source of data for new elements and you will obtain them or not if the sequence is terminated.
Those APIs are pull-based, I can request some data and I will block the current thread until I obtain it.

The Java standard library lacks some support for asynchronous sequences of data.
CompletableFuture has been a great addition for asynchronous programming but it lacks a proper API for managing such sequences.
Doing so with CompletableFuture would be quite verbose and bug prone.
This is why members of the Java community have participated in the [Reactive Streams](https://www.reactive-streams.org) initiative in order to build a standard for asynchronous streams of data.

As a result of this work, two major frameworks implementing the Reactive Streams APIs were born, RxJava and Reactor.
RxJava is mostly used in the Android community while Reactor is favored in the Spring community.
Both projects are cooperating in a joint effort to create a set of [Reactive-Streams compliant operators](https://github.com/reactor/reactive-streams-commons).
The APIs of the Reactive Streams project have even been integrated in [Java 9](https://docs.oracle.com/javase/9/docs/api/java/util/concurrent/Flow.html#nested.class.summary) even if the Java standard library does not provide any implementation of those interfaces.
Here are the four interfaces involved:

- Publisher
- Subscriber
- Subscription
- Processor

Think of Publisher and Subscriber as a reactive version of Iterable and Iterator.
The main difference is that instead of requesting data from an Iterable thanks to its Iterator, we can subscribe to a Publisher thanks to a Subscriber to receive new data when it will be available.
The Publisher can also send an error to its subscribers to terminate the sequence because an invalid situation has occurred.
The publisher can also indicate that there won't be any more data by indicating that the sequence is now terminated.
It is thus quite easy for the subscriber to handle those situations.

The Subscription allows the Subscriber and Publisher to setup some details related to their exchange.
For example, a Subscriber can let a Publisher know that it is sending more data than what the Subscriber can currently process.
The Publisher can then adapt the volume of data sent to match the capacity of the Subscriber.

Finally, a Processor acts as both a Publisher and a Subscriber in order to create advanced sequences for example.
This new API is a push-based API, as the consumer of the data, I only have to express what I will do with the data when it will be available.
The publisher can freely determine when it will send data.

Let's have a closer look at [Reactor](https://projectreactor.io).
This projects has been created by the Spring community where it is used as a core component of [Spring Webflux](https://docs.spring.io/spring/docs/current/spring-framework-reference/web-reactive.html).
A web server has to deal with tons of I/O operations such as database or network requests.
In this situation, blocking APIs are an issue.
To fix this, servers tend to create new threads and move the blocking code to those threads which works but also create a large set of idle threads.
This is why the Spring community has introduced a new stack to build web servers based on Reactor.

Reactor is based on two major implementations of Publisher, Mono and Flux.
A Mono represents a sequence of 0..1 element while a Flux is used for sequences of 0..n elements.
Those reactive concepts are especially powerful to build asynchronous and non-blocking architecture.
Just like with Optional and Stream, if you don't subscribe to a Publisher nothing will happen.

```
Mono.just("first").subscribe(System.out::println);
Flux.just("first", "second", "third").subscribe(System.out::println);
```

They may look similar to the Optional and Stream API but they can do much more.
In order to understand the various operations that can be performed on reactive streams, the community has done an amazing job to write great documentation.
This documentation comes with marble diagrams which show a visual representation of the behavior of an operation thanks to high quality SVG.
Yet be prepared, while some are quite easy to understand others are a bit intimidating.
Even the most complex ones are still of a great help in order to understand what is going on.
I have included a couple of those marble diagrams from the official Reactor documentation below.

<img src="{{ site.url }}/img/posts/2020/04/29/reactive-programming-with-reactor/flux.svg" class="img-fluid">

There are tons of operations available on those publishers, starting with those that we are now familiar with since we use them all the time with Streams and Optionals such as `filter` and `map`.
You can have a look at the marble diagram for `map` first.

<img src="{{ site.url }}/img/posts/2020/04/29/reactive-programming-with-reactor/mapForFlux.svg" class="img-fluid">

```
Flux.just("first", "second", "third")
    .map(String::toUpperCase)
    .filter(s -> s.contains("S"))
    .subscribe(System.out::println);

// Result:
// FIRST
// SECOND
```

Just like with Streams and Optionals, playing with Flux and Mono is easy since there are lot of options to combine them.
For example, `zip` allows us to combine the result of two publishers into a new one.
With two Flux, it will wait for each Flux to emit one value and allow us to combine the results.

<img src="{{ site.url }}/img/posts/2020/04/29/reactive-programming-with-reactor/zipWithOtherUsingZipperForFlux.svg" class="img-fluid">

```
var charFlux = Flux.just("a", "b", "c", "d");
var intFlux = Flux.just(1, 2, 3, 4);

intFlux.map(Object::toString)
       .zipWith(charFlux, String::concat)
       .subscribe(System.out::println);

// Result:
// 1a
// 2b
// 3c
// 4d
```

Reactor also allows us to play with the time.
As an example, you can delay the whole sequence or each individual elements very easily.

```
Flux.just(2, 4, 6, 8)
    .delayElements(Duration.ofMillis(100))
    .subscribe(System.out::println);
```

With `concat`, Reactor gives us the ability to stick together one flux after the other.
Thanks to this operator, we can subscribe to a publisher and after that to a second one.

```
var even = Flux.just(2, 4, 6, 8)
               .delayElements(Duration.ofMillis(100));
var odd = Flux.just(1, 3, 5, 7)
              .delayElements(Duration.ofMillis(75));

even.concatWith(odd).subscribe(System.out::print);

// Result: 24681357
```

On the other hand, with Merge, we can mix two publishers together and subscribe to the end result.
Without `delayElements`, we wouldn't see the difference between `concat` and `merge` in those examples since it would go too fast.

```
var even = Flux.just(2, 4, 6, 8)
               .delayElements(Duration.ofMillis(100));
var odd = Flux.just(1, 3, 5, 7)
              .delayElements(Duration.ofMillis(75));

even.mergeWith(odd).subscribe(System.out::print);

// Result: 12345678
```

In case we need to connect to other frameworks which are expecting a CompletableFuture, we can convert a Mono or a Flux quite easily.
Of course, you will have to ensure that you Flux is not infinite, otherwise you will have to wait a long time...

```
Mono<List<Integer>> list = Flux.just(1, 2, 3)
                               .delayElements(Duration.ofMillis(100))
                               .collectList();
CompletableFuture<List<Integer>> completableFuture = list.toFuture();
```

We can also wait for a specific number of subscribers before our publisher starts emitting data.
You could thus quite easily build a multiplayer game waiting for two participants to be connected before sending them updates of the current game.
For that, you can use `publish` to create a ConnectableFlux which provides additional features to manage subscriptions such as `refCount` to wait for a specific number of subscribers.

With the following code, you could see that both subscriptions occur before the Flux send any value.
Once the Publisher start sending values, both Subscribers receive them simultaneously.

```
var stringFlux = Flux.just("first", "second", "third")
                     .publish()
                     .refCount(2);

System.out.println("First subscription");
stringFlux.subscribe(System.out::println);
System.out.println("Second subscription");
stringFlux.subscribe(System.out::println);

// Result:
// First subscription
// Second subscription
// first
// first
// second
// second
// third
// third
```

Of course, since both Mono and Flux are implementations of Publisher, you can mix them quite easily without issues.

```
Mono.just("first")
    .concatWith(Flux.just("second", "third"))
    .subscribe(System.out::println);
```

Now, let's consider that you want to build a notification system, you may not want to receive every single notification that has ever been sent just because you subscribed to the notification Flux.
In such situation, you would like to receive only the new notifications.
For that, Reactor allows you to define hot sequences of data.
Contrary to cold ones which would start sending data from the beginning to each new subscriber, hot ones will only send the new data.

Here, we will use a Processor, the DirectProcessor, to acts as a hot sequence.
Since it is Processor, it can both emit data and subscribe to another source of data.
Here we will subscribe to this Processor and manually send it some new data.
Thanks to this hot publisher, the second subscriber will only receive the values that have been emitted after its subscription.

```
DirectProcessor<String> processor = DirectProcessor.create();
processor.subscribe(value -> System.out.println("First Subscriber: " + value));
processor.onNext("first");
processor.onNext("second");
processor.subscribe(value -> System.out.println("Second Subscriber: " + value));
processor.onNext("third");

// Result:
// First Subscriber: first
// First Subscriber: second
// First Subscriber: third
// Second Subscriber: third
```

Since Reactor is dealing with asynchronous sequences of data, scheduling those quickly becomes a topic to address.
One of the strengths of Reactor is that its concepts are concurrency agnostic.
We have seen lot of options to create Flux or Mono and how subscribers can receive some data.
We can just as easily transform a Flux into a ParallelFlux with a specific scheduling policy to perform some computation in parallel.
We can even transform our ParallelFlux back into a regular Flux once the parallel work is done thanks to operators such as `ordered`.
Here is an example of the use of those APIs.

```
Flux.just("first", "second", "third")
    .parallel()
    .runOn(Schedulers.boundedElastic())
    .map(String::toUpperCase)
    .filter(s -> s.contains("S"))
    .ordered(String::compareTo)
    .subscribe(System.out::println);
```

This example is a bit stupid since the work to perform is so simple and fast that the use of a ParallelFlux is pointless, but it gives you an idea of what can be done with the Reactor API.

All those shiny new features are quite interesting but for me the best part of Reactor is its testing API.
The ability to test easily the behavior of your Publisher is amazing.
Asynchronous programming is hard and without the ability to write such powerful tests, it would be hard to be confident in the code you will write.

Let's consider an asynchronous sequence of numbers, how could we possibly test it?

```
var evenNumbers = Flux.just(2, 4)
                      .delayElements(Duration.ofMillis(100));
var oddNumbers = Flux.just(1, 3)
                     .delayElements(Duration.ofMillis(75));
Flux<Integer> numbers = evenNumbers.mergeWith(oddNumbers);
```

With the StepVerifier, Reactor gives us the ability to perform assertions on the next values of our sequence.
We can also test that the sequence will be complete or that an error will appear.

```
var evenNumbers = Flux.just(2, 4)
                      .delayElements(Duration.ofMillis(100));
var oddNumbers = Flux.just(1, 3)
                     .delayElements(Duration.ofMillis(75));
Flux<Integer> numbers = evenNumbers.mergeWith(oddNumbers);

StepVerifier.create(numbers)
            .expectNextMatches(number -> number.intValue() == 1)
            .expectNextMatches(number -> number.intValue() == 2)
            .expectNextMatches(number -> number.intValue() == 3)
            .expectNextMatches(number -> number.intValue() == 4)
            .expectComplete()
            .verify();
```

Now let's consider this Flux:

```
Flux.just(1, 2).delayElements(Duration.ofHours(1));
```

While it is not complicated, it will literally run for hours.
We could easily write a unit test testing that we will check that we will obtain our two values, but we don't want to spend hours in our continuous integration process for such a simple test.
For those situations, Reactor also give us the ability to run tests with virtual time.
This way, we can easily test this behavior in milliseconds instead.

```
StepVerifier.withVirtualTime(() -> Flux.just(1, 2).delayElements(Duration.ofHours(1)))
            .expectSubscription()
            .expectNoEvent(Duration.ofHours(1))
            .expectNextMatches(number -> number.intValue() == 1)
            .expectNoEvent(Duration.ofHours(1))
            .expectNextMatches(number -> number.intValue() == 2)
            .verifyComplete();
```

I hope that you have enjoyed that introduction to Reactive programming in Java with Reactor.
If you have any questions, don't hesitate to contact me on [Twitter](https://www.twitter.com/sbegaudeau)