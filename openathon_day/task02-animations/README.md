<p align="center">
    <img src="../../boring-theory-1/resources/header.png">
</p>

# Task 02 - Animations

Animations are a main topic of the UX . Nowadays they aren't a superfluous feature and they are used to guide the user through our app, enhance the user experience, call attention to focus in some feature...

We will introduce the basic management o the animations in Angular because this is a feature that Angular manage internally. To animate efficiently in Angular you have to learn the "Angular way to animate".

Our task will consist of animating the title that appears in some views at the top left when the user enter to these views. Something like <a target="_blank" href="https://www.dropbox.com/s/vnq7qk0plhvn6kp/task02_mini.mp4?dl=0">this</a>.

We'll follow proximately the official documentation so that you can take a look on it to learn more details without lost the task. Te difference is that in this document we have left only the information relevant to the task.

## Getting started

Angular use @angular/animations module to create animations so the first task to do is import this module to our *app.module.ts* file:

```javascript
...
import { BrowserAnimationsModule } from '@angular/platform-browser/animations';
...

@NgModule({
  imports: [
    ...
    BrowserAnimationsModule
  ]
...
```

Also, we need to import the specific functions that you will use in your components, something like this:

```javascript
import { trigger, style, animate, transition, state } from '@angular/animations';
```

You'll only import those you're going to use.

Also in the component you have to add te *animations* metadata property.

```javascript
...
@Component({
  selector: 'app-root',
  templateUrl: 'app.component.html',
  styleUrls: ['app.component.css'],
  animations: [
    // animation go here
  ]
})
...
```

The animations are triggered from this property as components of the array.

## Basic animation

Inside our array we'll use one or more *trigger()* functions to trigger the animation, and inside these functions we can use other functions as *state()* or *transition()* as we'll see.

To create a simple animation changing from a state to other we'll use the *state()* and *transition()* functions.

```javascript
...
animations: [
    trigger('openClose', [
      ...
      state('open', style({
        height: '200px',
        opacity: 1,
        backgroundColor: 'yellow'
      })),
      state('closed', style({
        height: '100px',
        opacity: 0.5,
        backgroundColor: 'green'
      })),
      transition('open => closed', [
        animate('1s')
      ]),
      transition('closed => open', [
        animate('0.5s')
      ]),
    ])
  ]
...
```

In this code snippet you can see how we can write an animation changing between two states, *open* and *close*. We define the both states with their css properties and after that we define the transitions from one state to other.

The *state()* function has two arguments: the state name and a *style()* function to set the CSS style attributes. Note that the css syntax is a little different from the usual css. For example we need to enclose de non-numeric values with quotes.

The *transition()* function is used to set up the changes between states over a period of time. It takes two arguments: a expression to define the direction between two states, and the second one accepts an *animate()* function to define the length, delay, and easing of a transition.

```javascript
animate ('duration delay easing')
```

Inside the *transition()* function you can also define a *style()* function for defining styles while transitions are taking place.

What is the difference between using the *style()* function inside the *state()* function or inside the *transition()* function? You'll use the first one to define styles that are applied at the end of each transition, they persist after the animation has completed and you'll use the second one to define intermediate styles, which create the illusion of motion during the animation.

Whit the animation defined you have to attach it to the html element you want to animate. This is done like this:

```html
<div [@openClose]="isOpen ? 'open' : 'closed'"></div>
```

Now the element is managed by the animation triggered in the component. Note as the *isOpen* expression can be one of our defined states in the *animations* property.


## Predefined states

What happens if you want to animate an element when is showed? That is, when *ngIf* expression is *true* for example. How do we indicate the state before is showed? or perhaps we want to animate the element when change from a specific state to whatever state. The responses are the * (wildcard) and *void* states.

```javascript
...
transition('* => open', [
    animate('0.5s')
]),
...
```

```javascript
...
transition('void => open', [
    animate('0.5s')
]),
...
```

Note that you can use whatever combination *closed => void*, *open <=> close*, void => *... Also note that when you use:

```javascript
...
transition('void => *', [
      style({ transform: 'translateX(-100%)' }),
      animate(100)
]),
...
```

You're saying that the animation start when the element is showed in the view. To do this more concise we can use aliases: *:enter* and *:leave* like this:

```javascript
transition ( ':enter', [ ... ] );  // alias for void => *
transition ( ':leave', [ ... ] );  // alias for * => void
```

These aliases works very well with **ngIf* and **ngFor* directives to show/hide elements and also when a view is created. 

In these situations we can write the trigger in the html like:

```html
<div @myTrigger *ngIf="isShown"></div>
```

and in the code we don't need to use *state()*:

```javascript
...
trigger('myTrigger', [
  transition(':enter', [
    style({ opacity: 0 }),
    animate('5s', style({ opacity: 1 })),
  ]),
  transition(':leave', [
    animate('5s', style({ opacity: 0 }))
  ])
]),
...
```

When *isShown* is *true* (showed) the *:enter* transition take effect and when is *false* (hidden) the *:leave* transition starts.

## The task

With all of this information you can do the intended task. We need to animate the *h1* tags (the titles) in *add-edit-event.components.html*, *event-details.component.html*, *event-list.component.html*, *profile.component.html*, *login.component.html* and *signup.component.html*.

The animation will start on the view creation from a hidden state and the movement will be from the top to the bottom like <a target="_blank" href="https://www.dropbox.com/s/vnq7qk0plhvn6kp/task02_mini.mp4?dl=0">this</a> (the rest of the details are up to you).

In the *app* folder of the *lab-07* laboratory you have the start point of the task.

As we have to animate several components using similar code (the same animation), a extra point is to follow the DRY Principle (do not repeat yourself)  an to reduce the repetition of code somehow.

<p align="center">
    <img src="../../boring-theory-1/resources/header.png">
</p>
