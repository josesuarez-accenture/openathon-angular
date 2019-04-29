# Lab 04 - Services
## Objectives and Outcomes

In this lab we are going to introduce a important concept: Services. This will lead us to other features as:

* Dependency Injection.
* Get data from an API. 
* A introduction to Observables.
* Manage environment variables.


## Services and Dependency Injection

A service is a class that you want share across components. Usually a service is defined in a file apart and isn't associated with a specific view. As other Angular main elements, the service is annotated by a Typescript decorator named *@Injectable*. A service should has a specific job and do it well.

Examples of services would be fetch data from server, console logs, validations... these tasks can be shared  by several components and can be delegate to services to alleviate the workload to the components and don't repeat code (remember DRY - Don't Repeat Yourself ).

To use the service inside a component you have to inject it. This injection is done by the Angular Dependency Injection framework. The Dependency Injection is not an Angular Concept but a well known desing pattern to increase the efficiency and modularity.

In practice to use this mechanism in Angular you have to do it like this:

- Create the service with @Injectable annotation.
- Declare the service in a Module inside an object named *providers*. This way Angular knows how to obtain the service class itself.
- Inject the service inside the desired component by declaring it as a parameter in the *constructor* function.
- Now, you can use it as it if were part of the component (as a constant, function... it depends what the service return).

> **_Side Note:_**  A dependency doesn't have to be a service—it could be a function, for example, or a value.

It's time to create our first service. We are going to create a service to fetch our events data. This service will be used in the views that need it. In our case *event-list.component*, since this is the view that show the event list currently.

## Event service

The final propose of this service is to fetch the data from a API REST managed by our backend. The backend is the objective of another Openathon therefore we are going to get our data from a *.json* file to imitate the API request from our service.

As we've said, our service have to be imported by a module, so we will create a separate module to import the service (and all future services). This module, of course, have to be imported by de main  app module too, in order to be considered by the whole app... so:

```sh
ng g module core
ng g service core/event
```

This will create a *core* folder and the service file, *event.service.ts*. We've named the module as *core* following the <a href="https://angular.io/guide/styleguide#overall-structural-guidelines" target="_blank">Angular Style Guide</a>.

Now, paste this code to the *event.service.ts* file:

```javascript
import { Injectable } from "@angular/core";
import { HttpClient, HttpHeaders } from "@angular/common/http";
import { Observable } from "rxjs";

@Injectable({
  providedIn: "root"
})
export class EventService {
  constructor(private http: HttpClient) {}

  getEvents(): Observable<any> {
    const headers = new HttpHeaders({
      "Content-Type": "application/json"
    });

    return this.http.get("assets/events.json", { headers });
  }
}
```

There are some interesting things in this code. Let's take it slowly.

### Imports

As you know, we have to annotate the class by @Injectable typescript annotation, so we have to import it before. Now, Angular know that this class is a injectable service.

Also since we are going to get data from our backend (mocked as a .json file) we need to use the HttpClient module responsible for make the request over the HTTP protocol. We also import the HttpHeaders to typing some simple headers we will set up as an example (*"application/json"*).

The third import is a class form the RxJS library and it need a little introduction.

### Reactive Extensions Library for JavaScript (RxJS)

<br/>
<br/>
<br/>

[< Back](../lab-03) | [Next >](../lab-05)