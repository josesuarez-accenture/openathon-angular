# Lab 07 - Style and Deploy (extra bonus optional for code ninjas)

Something about Frontend can't avoid to talk about style, desing, UX... This is an important and extensive aspect and we aren't going to make a lab with all details about it. We only show you the final result of our app and will explain some main features.

The same about deploy. Another extensive topic that we hope to face in a future openathon (maybe about DevOps). But at this moment we will show you how deploy de app in the Cloud using Amazon Web Services.

Both aspects configure the final of our openathon but there are a lot of aspects that are left out or perhaps we could have done it differently. Considere this lab as a possibility to learn some about the big picture of the Frontend world.

Since we are going to jump some steps which are out of our scope, you always can see the final app in the *app* folder and in our site http://open-events.site where we've deployed the final app.

## Style

Web design is a thrilling topic. We do something in order to others can be take the "feel and look" for our app we wish. In our app we are going to design a style close to the Accenture brand and we will get some elements form the <a target="_blank" href="https://ts.accenture.com/sites/BrandSpace/">Accenture Brand Webpage</a>. We will show you how to implant some of these elements and other important aspects about the style in Angular.

All design journey start by imagining and drawing how we want to lay out our final product. We will jump directly to our final desired designs. Take a look to to our mockups:

<p align="center">
    <img src="./resources/01_Landing_page.jpg" width="800">
</p>
<p align="center">
    <img src="./resources/02_Events.jpg" width="800">
</p>
<p align="center">
    <img src="./resources/03_Event_details.jpg" width="800">
</p>
<p align="center">
    <img src="./resources/04_Event_add.jpg" width="800">
</p>

These are only indications or ways to go to our ending design. We can modify some details on the way. But this will be our "look and feel".

To reach this style, we've modified all html and scss files and we've create some other code in several places (you can study the code in the *app* folder). We'll just go over some of the highlights.

### Manage the style in Angular

It's not very complicated to manage the style in Angular but you have to take into account some important aspects.

* Each component has their own scss to styling. This scss code is closed to outside the component. For example, your classes naming can't collide with other names of other components.
* The *src/style.scss* file is where we can put the global styles which will affect to the whole app.
* We could have other general style files including them in the *angular.json* file (*"styles"* property).

We will split the styling in files and we will use the *import* command from css to join and compose our final style.

### Some generic styling files

It's good idea to have some files to store style variables and other details as mixins (thanks to Sass).

> **_Side Note:_**  Sass is a preprocessing engine for css that add some helpful features. The files with *.scss* suffix follow the a Sass syntax. You can see something more about Sass <a target="_blank" href="https://sass-lang.com/guide">here</a>.

