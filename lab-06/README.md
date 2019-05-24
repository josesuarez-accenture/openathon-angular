# Lab 06 - Central State management

This lab is about the app state. It's a more advanced concept that we've already seen so far, but it's important to know it since nowadays it's the way that the enterprise-ready applications are made (either using NgRx, Redux, Flux or others). 

The challenge is that you have to change the way to think about the events and facts in your app since the app architecture are going to change now: it's an architecture of central state.

Let's go step by step.

## The state of an app

The first thing you have to know is what the state of an app is. Consider the main app view.  The *Home* link is active and the view show you a presentation text, no data yet from API. This is the current state of the app, we could name it "main state" but the name isn't important really, the most importante think is the snapshot the user is watching in the app, the state of the app.

If now we¡are going to *Events" the app make a request to the API and this data is part of the new app state now (along with the new view and other things which make possible how everything looks like). Each time something happen a new state is set.

When we have a big enterprise-size app (even a medium/small app) we nee to know the things that happens in one part of the app from the others parts of the app: we need communicate different parts of the app. 

For example, thinking in our app if we would like to create a breadcrumbs (see side note below) and place it bellow the toolbar (where normally it is placed). In this situation the routing system need to communicate where the user is to the breadcrumb component (the most normal way is that the breadcrumb life happily in his own component).


> **_Side Note:_** Don't you know what the breadcrumbs is? The next image illustrate what this is. It's a simple way to see where are you in a web app. They locate the page where you are (Home / Tutorials / Drawing in the next picture).

<p align="center">
    <img src="./resources/breadcrumbs.jpg" width="400">
</p>

To do this Angular has several options. We can pass data from parents to children, we can make a Service to inject data where we want... All of this solutions are good but they solve particular situations or perhaps a broader situation with more work. At the end it is about manage the state and when a particular part of the app change... other parts have to know these changes.

I'm sure that you've realized how many situations as this can happen in an app: to do click in one button and react to this in other part, move the cursor and something happen in a side bar, to do click in a link and change de style of the view, todo click and show data in the right side, go back and show the same data form the API that before... we present you... our friend the *central state* of the app, known in some context as Redux an in the Angular world as NgRx/Store.

Our very friend *central state* will know and manage all state we want to manage and are important for us.

> **_Side Note:_** There's a lot to talk about state management. You can start for the <a target="_blank" href="https://facebook.github.io/flux/">Flux</a> and the init of the solution to this problem created by Facebook. After that, you can lear something about <a target="_blank" href="https://redux.js.org/introduction/getting-started">Redux</a> a popular library. We will only focus on the mechanism used by Angular, the NgRx/Store library.

## The Central State and NgRx/Store

We could think of the Central State as a place where:

* The state is organized in a unique place.
* You can ask it what is the current state.
* You can describe changes in the state of this place.
* You can subscribe to the state to know changes.
* The state will notify changes to subscribers.

NgRx is the library created inside the Angular community to manage a central state which is commonly named the *single source of truth*. 

The library use intensively the reactive library RxJS which you already know a little. This means that yo have to have a good understanding of the reactive programming using observables. That don't means that you have to study deeply the RxJS in this moment, we are going to go slow and if you need some more grasp out of the scope of this lab we give you external resources.

Our application will change very little his functionality but, we said, we are going to change the architecture... it will be a deep change inside the app.

## Architecture

There are several pieces of the Central State Architecture and others contextual libraries to apply difference functionalities to it, but we are going to focus in the minimum needed but completely functional for our app.

The basic Architecture consist in two main actors. *Actions* and *Reducers*.

Actions are unique events that happen throughout your app (mouse click, requests, external interactions...). When an action happens we will send it to the *Reducer* (it can go to other place named "*Effects* but we talk about this later) and the *Reducer* will change the state as necessary depending of the type of the action. So, the *Reducer* is the only place where the state of the app will change.

The *Reducers* are pure functions so they aren't really changing the state but making a copy of the existing state and changing one or more properties on the new state (if you know the mechanism of git control version, this will already be known to you). 

The NgRx library give us some help to do this. We have to create the actions by implementing the NgRx Action interface with a *type* property (mandatory) and another *payload* property (optional). The first is to say the type which you will define and the second is to store the eventual data that entails the action. We will always create our actions so this way. Isn't that great?

*Reducers* always wait until an action is dispatched so they can do their job. How are the *Actions* dispatched? The NgRx library give us the *dispatch* method (through the *store* class) to dispatch Actions by creating a new copy of our Action class (remember that a Action is a class which implements the Action interface). Look how it's:

```javascript
...
this.store.dispatch(new event.getEvents());
...
```

In somewhere we have to have created the *getEvents* action as a class, the store takes care of the dirty job, that is: dispatch the action, make the *Reducer* listen to it and make the *Reducer* change and produce a new state that all observers which are subscripted to it can be act accordingly. Of course, some of this things will depends to us and they are our job in the following sections.

Take a look to this simple Architecture:

<p align="center">
    <img src="./resources/CentralState.png" width="900">
</p>

## Changing the architecture of our application

Well, let's get our hands a little dirty. The first thing we have to thing is about the new folder structure. Though there will only have a store (central, remember) we will split the store in logical parts to manage it easier (think about a big app with a big state to manage). Each part will be a feature or main aspect of our app so we are going to create a folder named *store* to host this parts. Each part will have their own *Actions* and *Reducers* and all of this parts we are going to gather in a file named *app.store.ts* which will pass all to our existing *app.module.ts" to inform to the app about our new store. The *app.store.ts*  will live in the root since is a file to gather all.

<p align="center">
    <img src="./resources/new_structure_store.png" width="250">
</p>

Now is moment to install the NgRx library.

```bash
npm install @ngrx/store --save
```

## Central store to login

That is to say, everything that we want to put in the store as an action that provokes changes in the state. In our case, we will add the action "logged" to change the toolbar link (login/logout) and to inform to the other parts of the app (if they want) that the user is logged in. We won't see any difference in the view but the way to manage this information will change to a more logic manner.

Create a *login.actions.ts* file inside *store/login* folder with this content.

```javascript
import { Action } from '@ngrx/store'

export const LOGGED = 'login/logged'

export class Logged implements Action {
  readonly type = LOGGED;

  constructor(public payload: boolean) {}
}


export type Actions = Logged;
```

As you see there are an import a three exports. You already know the purpose of the import: implements our action class with the Action interface.

As we already advanced you, this interface forces us to create a property named *type* (read only) and an optional property named *payload*. In our case *type* will be *LOGGED* which we define it as a constant wit *login/logged* value and our *payload* will be a boolean because if the user is logged in it will be *true* and if the user is logged out the value will be *false*.

> **_Side Note:_** It's a good practice to namespace the name of the actions (*login/logged*) in order to avoid collisions.

We export the constant and the class since we will need in the *Reducer* as you will see later.

The third export it's a type alias from typescript (basically it's used to give another name to a type) and we will need in the *Reducer* too to can discriminate the action type (in a *switch* statement). 

> **_Side Note:_** If you want to understand the Typescript's discriminate unions you can read <a target="_blank" href="https://basarat.gitbooks.io/typescript/docs/types/discriminated-unions.html">this</a>.




<br/>
<br/>
<br/>

[< Lab 05 - Routing 2 and CRUD](../lab-05)

<p align="center">
    <img src="../boring-theory-1/resources/header.png">
</p>