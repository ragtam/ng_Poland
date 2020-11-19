# Madfred Steyer | Angular future with IVY

Angular private APIs:

passing inputs through url param, without router

routes: [{ path: '', component: withRoute(ComponentName) }]

usually we would have to use Activated Route, and grab url params from there.

Barrel imports and libraries are going to be used instead of ngModules.

They are going to be optional in guture.

Right now IVY is transforming ngModules into factory functions.

---

zone.js

-   KEY TO automatic change detection
-   monkey patches all browser objects

downsides:

-   but its bundle is quite big
-   monkey patching is changing all browser object
-   but it cannot monkeypatch async/await

zone.js is going to be optional, see angular.io/guide/roadmap

ivy is introducing higher order components

from workshop:
http://workshops.angulararchitects.io/ms/f9bc9634-a993-4ab0-acd1-926fe8a4064d/module_federation.html
https://github.com/manfredsteyer/ngx-build-plus
https://www.angulararchitects.io/aktuelles/the-microfrontend-revolution-module-federation-in-webpack-5/

# Alex | @ngrx/component-store

property driling, passing inputs through several components

CartService {
readonly cartItems\$ = this.getCartProducts().pipe(shareReplay(1))
}

component subscribes to readonly prop from the service

but we need to be able to remove, add products

Service with behaviour subject.

State can be kept in the service, we have a private behaviour Subject, we provide cartItems\$ readonly getter, behaviour subject as observable.
We have add, remove product that invoke subject`s next method on bahaviour subject. Whenever someone manipulates on data, we get information about it.

But there might be a RACE CONDITION there, if two adds were clicked, but the first one took longer than the second one. Bug difficult to find.

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