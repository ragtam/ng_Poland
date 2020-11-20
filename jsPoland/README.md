# Ruben Bridgewater | Demystifying Memory Leaks in JavaScript

memory leak, when a computer program incorrectly manages mamory, in a way then when is is no loger needed, its not released

Mamory usage should not grow, the line of memory usage should be flat, it might go up, but should go back again.

The cheapest fastest and most reliable components are not there

## Stack vs Heap

Stack - fast, automatically managed, function calls

Heap - main memory of app, our strings numbers

Heap memory v8:

-   young generation: small, only new objects, frequent scavenges. Not lasting for long time
-   intermediate generation
-   old generation: mark sweep (algorithm for freeing memory)

Garbage collection problems:

-   Typed arrays are more efficient

-   we need to close file descriptors explicitly. When reading a file for instance.

-   Reading big data in memory. Its better to fs.createReadStream, instead of reading a whole file.

-   long strings:
    let longStr = "0".repeat(1_000_000)
    const leak = longString.slice(0, 20)

    we are going to have a memory leak there, what about rest of the characters?

    Concat might not always create new string, it might reference the other.

## how to detect and fix memory leaks?

tools / flags
--inspect --trace-gc --abort-on-uncaught-exception llnode

# Josh Goldberg | Super Strength ESLint

ESLint - statically analyzes JS & TS for defects
Autofixes defects

static analysis - lint
dynamic analysys - tests
telemetry - feedback from users?

```
npm i eslint --save-dev
npx eslint --init
```

## Prettier

formatter !== linter
prettier is about code formatting, its faster at that
**dont use linter as code formatter**

eslint-plugin-prettier. Tells eslint to switch off rules that are going to overlap wit prettier

## AST

Abstract Syntax Tree
bare bones tree reporesentation of a source file. Each section of a file represented as an object

```
console.log("hi!")

- member expression ( console.log )
    - identifier ( console )
    - identifier ( log )
- literal ( "hi" )

```

lint takes our AST, applies its rules, returns new AST

# Michael Hladky | A deep dive into RxJS subjects

!!! GOTTA WATCH FULL PRESENTATION

Subject expose next complete, error, as public api. Whis is what is hidden on Observable. Subject **Its HOT all the time**.

Producer in observable is **inside**, in Subject its **outside**.

Observable one producer one subscriber
Subject one producer, several subscribers (multicasting)

@Lamis Chebbi talk on subjects

Subjects:

-   BehaviourSubject
    Forwards to multiple observers, needs to have initial value, replays latest value to new subscribers. New values are cached.
    It forwards a completion. If we subscribe to completed subject, it forwards completion.

-   ReplaySubject
    It informs us about whole history of a producer. Behaviour subject cached one value, ReplaySubject caches all values in array.
    If it completes and someone resubscribes to it? We will receive full history of a subject, and then reemits completion.
    We can set a **buffer size** on ReplaySubject. Size of cache array.
    **Window time** lets assign a lifetime to every single value. We can say that cache item lifespan is 5ms for instance.
    Good for history notifications

-   AsyncSubject
    Only if it completes, it emits last value.
    Where can we use?

What happens if we subscribe to already to completed Subject. It Completes again

# Manfred Steyer | The Microfrontend Revolution: Using Webpack 5 Module Federation

Separately compiled, built and deployed applications

```
const Comp = await import('https://other-app/xyz')
```

the above assumes its there at compile/build time. Even lazy parts must be there. This would be spitted as separate webpack bundle.

```
// shell:
import('mfe1/Cmp') // Cmp is what is exposed as property in exposes obj of a remote

remotes: {
mfe1: 'mfe1'
}

// remote:
exposes: {
'./Cmp': './my-cmp'
}
```

This tells webpack that it will have this chunk dynamically. So its no longer necessary at compile time.

We can share libraries. But HOW TO SHARE A MODULE?!?!?!

Angular CLI is delegating to builder to build application. There is a default implementation of a builder, but we need to delegate our custom buidler that will handle delegation.

```
ng add @angular-architects/module-federation
```

generates a skeleton, installs custom builder

creates webpack.config.js, webpack.prod.config.js and updates angular.json

in package json we need to add resolutions to start using webpack 5

d.ts file so that typescript did not complain about not existing module:

```
export module 'mfe1/Module' // but we could 'mfe1/*' to say that everything that starts with mfe1 is fine
```

# Christoffer Noring | Svelte, the invisible SPA framework

@chris_noring

The why?

-   less boilerplate
-   no virtual dom
-   no script as a dependency. It compiles to the state where its a vanilla JS

what it does?

-   can swap out individual components or whole app
-   we mostly need to learn HTML, CSS, JS
-   one component per file
-   styles scoped by default
-   input props, watching changes, events

**Its invisible**

```
<script>
    let name = 'world'
</script>

<h1>{name}</h1>
```

on the basis of that it generates vanilla js

svelte.dev => interactive tutorial

```
npx degit sveltejs/template hello-app
npm install
npm run dev
```

PRETTY FAST, 2 sec to scaffold, 2 sec to start the hello world app

```
<button on:click={ourFun}>
```

but, no string sanitization

## Components

one component does one thing

```
<style>

</style>

<script>
let src = "somesrc.jpg"
</script>

<h1></h1>
<img src = {scr}>

// in another component

import Nested from './nested.svelte'

```

deta down from parent to child

events from child to parent

```
//component input

export let answer;

<Nested answer="42" />

```

## Component output

createEventDispatcher, dispatch event by name and with a payload, handle event

```
let dispatcher = createEventDispatcher()

dispatcher.dispatch('message', { payloadProp: 'hello' })

<Inner  on:message={handleMessageFromInner} />

// if we need to just forward a message from inner

<Inner on:message />

// and on the very outer we just grab the ordinary way
```

## Directives

```
// if
{#if user.loggedIn}
<p>text</p>


// for loops
{#each todos as todo}
    <div>some div</div>
{/each}

// await
{#await promise}
    <p>its loading</p>
{:then data}
    <p>data loaded {data}</p>

```

## Testing

testing happens on the surface, JEST, different paradigm

# Minko Gechev | Scaled Web Development

source maps explorer to understand the bundles

configure size bugdets

lighthouse ci, add that as a part of ci

Ng Module are for h

Incremental builds: Bazel style

# Valentin Kononov | Runtime Type Safety in Typescript

to be checked in the console:

```
> '5' - 3
> '5' + 3
> '5' + - '2'
> 5 + { v: 2 }
> 5 - { v: 2 }
```

npm i ts-stronger-types

# Sam Bellen | Knock knock, who’s there

jwt.io
jwt.sambego.tech

# Henri Helvetica | Moving Pictures

bit.ly/movingpictures_jspoland

## into the future of web media

image formats in order:

gif =>
png (possibility for transparency) =>
joint photographic experts group (alpha channel possible) =>
scalable vector graphic (scallable)

each of them is almost 20 year old, together 104 years old :D

## foundation of the net is compression

without a compression 12 megapixel image would take 36 megs of storage

heif - format by apple, was not issued because of patents

jpg is most popular format.

so we need high performant image formats:

-   jpeg xl: pik and fuif formats. alpha, loosless, animation, modern compression, responsive, royalty free, **close to final stage**
-   avif: comes from a video format. Alliance for Open Media. Chrome and opera only. Firefox needs to be configured
-   webp: 10yrs old, made for web, transparency, animations, going to replace png. Supported across the browsers

## tooling for webp

squoosh.app
gimp
photoshop plugin

-   webp2 is under development. 30% better compression

# Nir Kaufman | There are no bad practices! How to code for humans - Not computers

url: nir.life

# Vitalii Bobrov | Job Behind the Scene: Web Worker

js is single threaded, but we can paralellize it using web workers, shared workers

## Workers limitations

-   makes copy of data
-   have no direct access to dom and global functions
-   communicates via messages

How to use web worker

```
const worker = new Worker('path/to/worker.js')
```

its even supported by IE10

main thread:

```
worker.postMessage('data')
worker.addEventListener(
    'message',
    (event: MessageEvent) => {
        console.log(event.data)
    }
)
```

worker thread:

```
worker.addEventListener(
    'message',
    (event: MessageEvent) => {
        console.log(event.data)
    }
)

postMessage(data)
```

We can`t pass a reference. We should use **buffers** for posting data.

```
const buffer = new Uint8Array(data);
worker.postMessage(buffer, [buffer])

addEventListener( 'message', {data} )
```

This way we are saving a lot of memory. It will save copying of elements.

## Shared array buffers

Can be shared across Workers. DOPE. It`s not green in all the browsers.

Pool of workers:

```
let workersPool = []

for( let i = 0; i < navigator.hardwareConcurrency; i++ ) {
    let new Worker = {
        worker: new Worker('./some/path/to/worker.js'),
        inUse: false
    };
    worlersPool.push(newWorker);
}
```

We should not exceed this number, hardwareConcurrency, if we do, worker will just have to wait for a thread to be free.

**Thread !== Code**

## Shared Worker

can be used by a few tabs of a browser of the same origin. Data persists between tabs. We need to set port.

```
const sharedWorker = new SharedWorker('')

sharedWorker.port.postMessage(['data'])
```

## helper libraries

-   comlink: simplifies worker communication. Proxy object for worker.

## Time complexity | Sorting

merge sorting, external sort
