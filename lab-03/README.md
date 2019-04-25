# Lab 03 - Routing Basics
## Objectives and Outcomes
In this lab we will transform our aplication with a component-based navigation to a routing-based navigation. This feature will make that the app more web-like app and other goodness:

* We will have a url for each view. 
* It will allow us to navigate back and for (navigation history).
* It will allow manage advanced features like url parameters, child views, route guards...

Actually, the app is a SPA (Single Page Application) therefore there isn't real change of url. Like a desktop app, the whole life of a SPA happens in one view-page. As we have said, what we will do is to bring the SPA closer to the web world by providing it with different routes per page.


## A new folder structre

In order to do this, we are going to change the folder structure following the <a href="https://angular.io/guide/styleguide#style-04-07">best practices</a> and we will split the folders by major features. 

Por otro lado, vamos a tener en cuenta la escalabilidad de la aplicación y prepararemos la aplicación para alojar un gran número de componentes y funcionalidades diferentes. Es una buena práctica para hacer que su aplicación sea escalable sin caer en la excesiva complejidad.

We are going to do this adding a new module each major feature in order to atomize the app and to make more operative the possible addition of componets.

We are going to consider three major features: events, profile and login, therefore we'll create three folders with three different modules. These modules are named "Feature Modules" because they manage features. After, these modules will have to be imported by the main app module.

Add the new folders/modules like this:

```sh
ng g module events
ng g module profile
ng g module login
```

> **_Side Note:_**  How you decide what feature is a major feature candidate to host a Feature Module? It depends. Sometime it's clear since the feature is a main feature (as "events") and sometimes you think that a feature will grow a lot in the future... In this case, we can argue the need of Feature Modules in "login" and "profile" features, but we will include them for sake of example.