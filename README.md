
# EventEmitter in JavaScript [![Build Status](https://travis-ci.org/Zlobin/EventEmitter.png?branch=master)](https://travis-ci.org/Zlobin/EventEmitter)

## Synopsis

EventEmitter is an implementation of the Event-based architecture in JavaScript.

The code is written using the ES2015 approaches, including creation of private property through WeakMap that allows you to not to check for clearing memory, and let it do to the garbage collector.

The module contains the most basic and necessary things: subscription to an event, unsubscribing from the events, running event only once, setting the maximum number of listeners.

The focus is on code readability, speed of execution, getting rid of all the excess.

You can use this library in browser either at the server as within the node.js.

## Benchmark

```javascript
const EM = new EventEmitter();

EM
  .on('foo', () => 'response')
  .emit('foo');
```

```
// Custom:
es-event-emitter 100,000: 29284943 nanoseconds
Node.js native 100,000: 41954981 nanoseconds
EventEmitter2 100,000: 91033365 nanoseconds
event-emitter 100,000: 211658201 nanoseconds

// Via benchmark library:
es-event-emitter x 21,508,327 ops/sec ±0.47% (76 runs sampled)
event-emitter x 5,444,296 ops/sec ±0.40% (89 runs sampled)
EventEmitter2 x 18,105,022 ops/sec ±2.07% (74 runs sampled)
Node.js native x 17,800,136 ops/sec ±0.87% (83 runs sampled)
Fastest is es-event-emitter
```

## Dependencies

There are no dependencies. You need only npm installed and just run `npm install` to grab the development dependencies.

## Examples
run `npm i es-event-emitter`

Run `npm i` under EventEmitter library's path and now you can include EventEmitter in html.
```html
<script src="<path to es-event-emitter library>dist/bundle.js"></script>
<script>
var EM = EventEmitter.default();
// ...
</script>
```

or just:
```javascript
import EM from 'es-event-emitter';
// ...
```

Creating an instance.
```javascript
const EM = new EventEmitter();
```

An usual example.
```javascript
EM.on('foo', () => {
  // some code...
});

EM.emit('foo');
```

It will be triggered only once and then callbacks will be removed.
```javascript
EM.once('foo', () => {
  // some code...
});

EM.emit('foo');
// Nothing happend.
EM.emit('foo');
```

Callback with parameters.
```javascript
EM.once('foo', (bar, baz) => {
  // some code...
});

EM.emit('foo', 'var 1 for bar', 'var 2 for baz');
```

Callback's call can be ordered by "weight" parameter.
```javascript
EM.on('foo', () =>
  console.log('3')
, null, 1);

EM.on('foo', () =>
  console.log('1')
, null, 3);

EM.on('foo', () =>
  console.log('2')
, null, 2);

EM.emit('foo');
// 3
// 2
// 1
```

You can use chaining.
```javascript
EM.on('foo', () => {
  // some code...
});

EM
  .emit('foo')
  .emit('foo')
  .off('foo');
```

You can set maxNumberOfListeners as a parameter when creating new instance.
```javascript
const EM = new EventEmitter(1);

EM.on('foo', () => {
  // some code...
});
// Note: it will show notification in console.
EM.on('foo', () => {
  // some other code...
});
```

## Testing

Tests are performed using mocha and expect library `npm test`.

## Building the documentation

You can use JSDoc comments found within the source code.

## Todo

1. Add event's namespace:

```javascript
EM.on('foo.*', () => {
  // some code...
});
```

2. Add events through comma:

```javascript
EM.on('foo,bar,baz', () => {
  // some code...
});
```

3. Add method "onAny" for listening each event:

```javascript
EM.onAny(() => {
  // some code...
});
```
