# Kamil Gałek - Small fixes for huge performance problems

go to his github and go through pull requests in galczo5/perf2020

[his repo from conference](https://github.com/galczo5/perf2020)

**A MUST !!!**

# Madfred Steyer | Angular future with IVY

Angular private APIs:

passing inputs through url param, without router

```
routes: [{ path: '', component: withRoute(ComponentName) }]
```

usually we would have to use Activated Route, and grab url params from there.

Barrel imports and libraries are going to be used instead of ngModules.

They are going to be optional in guture.

Right now **IVY** is transforming ngModules into factory functions.

---

zone.js

-   kay to automatic change detection
-   monkey patches all browser objects

downsides:

-   but its bundle is quite big
-   monkey patching is changing all browser object
-   but it cannot monkeypatch async/await

zone.js is going to be optional, see angular.io/guide/roadmap

ivy is introducing higher order components

# Alex | @ngrx/component-store

property driling, passing inputs through several components

```
CartService {
readonly cartItems\$ = this.getCartProducts().pipe(shareReplay(1))
}
```

component subscribes to readonly prop from the service

but we need to be able to remove, add products

Service with behaviour subject.

State can be kept in the service, we have a private behaviour Subject, we provide cartItems\$ readonly getter, behaviour subject as observable.
We have add, remove product that invoke subject`s next method on bahaviour subject. Whenever someone manipulates on data, we get information about it.

But there might be a **RACE CONDITION** there, if two adds were clicked, but the first one took longer than the second one. Bug difficult to find.

**@ngrx/component-store** is a standalone lib for state management. it can replace a service like the one above.

-   **exhaustMap**: hey I am already working on sth, get lost
-   **switchMap**: hay, I am working on sth, I am discrading that and start doing what you want now
-   **concatMap**: hey, i am working on sth, ill put you on the queue

tapResponse, for catching errors in streams

## where to user componentStore?

-   provided in root? works, but this is not the first choice
-   complex independent component state ( provided in some component, providers metadata). Ties store to the lifecycle of this component.
-   reactive local ui state,
-   shared/common component state, each instance of a component, might have its own component store

## STORE:

-   logic is pushed outside of a component, we just say, hey store, dispatch some event, and effects and reducers take care of that

-   selectors, reducers, effects, usually in separate files

-   in the end store is a ONE object

-   usually lifecycle of whole application

## COMPONENT STORE:

-   this logic is inside a component that has component store injected

-   should be small, selectors, reducers, effects are all in one file

-   several objects keeping state

-   lifecycle of providing component

## useful links:

discord.gg/angular (official one)
discord.gg/ngrx

# Michael Hladky | Fully Zone-Less - High-Performance Angular applications in post IVY

```
npm i rx-angular/state
```

rx-angular/template

async & onPush everywhere?

push pipe, let pipe, rxLet

from application rendering to component rendering

# Minko Gechev | Angular performance profiling patterns

youtu.be/ybNj-id0kjY | Minko`s talk on performance

## Profiling an application:

-   we need to be in production build
-   no mangle, disable mangling: NG_BUILD_MANGLE = false ng build --prod
-   no browser extension enabled, they might some noise to the profiler, open in incognito mode

performance tab, record. Flame Graph vs Flame charts

rendering on demand: intersectionObserver api

move expensive calls to web workers

# Dhananjay Kumar | Simplifying Dependency Injection in Angular

tt: @debug_mode

static property is shared across objects. 5 times constructor calls on service of the same type. One property that can sum up the number of calls

**scenario 1:**: we have a service, but its not injected anywhere:
Service provided in providers in module, is included although its not used, not injected anywhere. As opposed if we used provided in root, its tree shakeable, so it won`t be included in the bundle.

providers array, **useExisting** instead of **useClass**

**scenario 2:** Lazy loaded module service provided in app module
There will be one instance of a service

# Bonnie Brennan Sander Elias - You Don't Know Stack

queue and stack.

task que needs to finish once we can start doing anything else. Once its done, its gone, and new one gets created. Once task queue is started, we can`t add anything to that

**if you set 0 on set timeout, it sets to 4ms**

# Santosh Yadav | Performance Optimization for Angular Apps

use track by, apart from performance improvements, it`ll prevent rerender, if list is unchanged

guess parser - load modules based on user interaction

avoid a shared module

**source-map-explorer** to analyze bundles

```
ng add @ngx-bundlders/analyze
```

---

use budgets

use strict mode in angular

-- tree shakable libs
instead of a single entry point, add a secondary one, by ng-packagr or bazel

commonjs are not tree shakable. angular 10 will throw a warning that commonjs is used
s

use cdn, decrease server load, low latency, high availability.

use prod and use gzip minification ( have to be done on server side)

# Nir Kaufman | Angular as a meta-framework: Extend and abstract it for your needs.

3rd party libraries that manipulate the dom, like adding tooltips for instance, might trigger change detection. This might be a good idea to wrap 3rd party around a directive, that invokes a function from

# Wassim Chegham - From design to code

xLayers: SketchApp design into any framework

# Robert Willemelis | ~920 Bytes: Creating the smallest Angular App in the world

emojis for css classes?!?!?!

-   minifiers do better job

-   unused bytes

willi84 github, ng bundle optimizer

# Gerard Sans | Offline-first made easy with GraphQL and Amplify DataStore

offline first, offline service worker ( sits between a browser and a network)

# Maciej Czerwiakowski | Performance behaviors
