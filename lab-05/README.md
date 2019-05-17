# Lab 05 - Routing 2 and CRUD 

So far, we have build an application that is reading data from a database and displaying it as our event list. To do this application more useful, we are going to implement CRUD (Create, Read, Update and Delete) operations. As we've just said we are reading data already, now we are to develop further the web app adding the option to create a new event as well as update and delete an event. 

Moreover, we are going to see how to send parameters through routing and how to read them from the controllers. 

* Implement CRUD operations
* Send parameters trhough url and read them 

> **_Side Note:_** Before starting this lab, donwload this new <a href="./app/db.json">Donwnload db.json file </a>and update use it for your json-server previously installed in lab04. Remember that you can start the json server with the following command:
```sh
	json-server --watch db.json
```

## Read Event Details

Let's refactor our event list and details to a more proper way. We are going to separete them, first we will use a table for the event list using mat-table from Angular Material. When the user clicks in one event we will be using routerLink to redirect them to the a new event details view 

* Add the MatTableModule from Material in the shared.module.ts

```javascript
...
import { MatTableModule } from "@angular/material/table";
...
 imports: [
	...
	MatTableModule
  ],
```

* Edit event-list.component.html

```javascript
<div class="container">

  <div id="eventTable">
    <table mat-table [dataSource]="events" class="mat-elevation-z8">
      <ng-container matColumnDef="Date">
        <th mat-header-cell *matHeaderCellDef>Date</th>
        <td mat-cell *matCellDef="let element">{{ element.date }}</td>
      </ng-container>

      <ng-container matColumnDef="Location">
        <th mat-header-cell *matHeaderCellDef>Location</th>
        <td mat-cell *matCellDef="let element">{{ element.location }}</td>
      </ng-container>

      <ng-container matColumnDef="Title">
        <th mat-header-cell *matHeaderCellDef>Title</th>
        <td mat-cell *matCellDef="let element">{{ element.title }}</td>
      </ng-container>

      <tr mat-header-row *matHeaderRowDef="displayedColumns"></tr>
      <tr
        mat-row
        *matRowDef="let row; columns: displayedColumns"
        [routerLink]="['/eventDetails/', row.id]"
      ></tr>
    </table>
  </div>
</div>
```

* copy and paste the following in event-list.component.scss:

```javascript
.container {
  display: flex;
  justify-content: center;
  flex-flow: column;
  margin: 10%;
  margin-top: 2%;
}

#add-event-btn {
  display: flex;
  justify-content: flex-end;
}

button {
  max-width: 100px;
}

table {
  width: 100%;
  margin-top: 10px;
}
```

* Add the columns names that will be displayed in the table headers. In event-list.component change the code as follow:


```javascript
export class EventListComponent implements OnInit {
  ...
  displayedColumns: string[] = ["Date", "Location", "Title"];
```

* Add the eventDetails route in app-routing.module.ts
```javascript
...
import { EventDetailsComponent } from "./events/event-details/event-details.component";
...
const routes: Routes = [
  { path: "home", component: LandingPageComponent },
  { path: "events", component: EventListComponent },
  { path: "profile", component: ProfileComponent },
  { path: "login", component: LoginComponent },
  { path: "eventDetails/:id", component: EventDetailsComponent },
  ...

```

* Now we need to import RouterModule in our shared.moudule.ts

```javascript
import { RouterModule } from "@angular/router";
```



* As we said we are going to change the event-details.component.html to use a mat-card and include a button to edit the event. Also, we are adding the 'Edit Event' button that will redirect us to a new view.
```javascript
<div class="container">
  <div id="event-card-details">
    <div id="edit-event-btn">
      <button mat-raised-button color="primary" [routerLink]="['/addEditEvent/', event.id]">
        Edit Event
      </button>
    </div>

    <mat-card *ngIf="event">
      <mat-card-header>
        <mat-card-title>
          <h3>{{ event.title | uppercase }}</h3>
        </mat-card-title>
        <mat-card-subtitle>
          <p>{{ event.location }} - {{ event.date | date: "dd/MM/yyyy" }}</p>
        </mat-card-subtitle>
      </mat-card-header>

      <mat-card-content>
        <p>{{ event.description }}</p>
      </mat-card-content>
    </mat-card>
  </div>
</div>
```

* And event-details.component.scss

```javascript
.container {
  display: flex;
  justify-content: center;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

#event-card-details {
  display: flex;
  flex: 1;
  flex-flow: column;
  margin: 2%;
  max-width: 800px;
}

#edit-event-btn {
  display: flex;
  justify-content: flex-end;
  margin-bottom: 15px;
}

```

* We are going to get the parameter id from the URL and using our event service to get the especific event from the db. 

event-details.component.ts
```javascript
import { Component, OnInit, Input } from "@angular/core";
import { Event } from "../../models/event";
import { ActivatedRoute } from "@angular/router";
import { EventService } from "../../core/event.service";

@Component({
  selector: "oevents-event-details",
  templateUrl: "./event-details.component.html",
  styleUrls: ["./event-details.component.scss"]
})
export class EventDetailsComponent implements OnInit {
  event: Event;

  constructor(
    private route: ActivatedRoute,
    private eventService: EventService
  ) {}

  ngOnInit() {
    const id = this.route.snapshot.params["id"];
    this.eventService.getEvent(id).subscribe((event: Event) => {
      console.log(event);
      this.event = event;
    });
  }
}
```

* Add a new method in event-service to get the event using the GET method.

```javascript
getEvent(id: number): Observable<any> {
    const headers = new HttpHeaders({
      "Content-Type": "application/json"
    });

    return this.http.get(environment.apiURL + "events/" + id, { headers }).pipe(
      retry(3),
      catchError(this.handleError)
    );
  }
```

* Save all the changes that we have made. You should be able to get the event list and click in one of them and see its details in a new view.


## Create And Update An Event

** Now we are going to create a new component that will allow us to add and edit events. In order to achieve both operation reusing the same tamplate, we will be passing the event id parameter through the URL when editing an event and fill the form with the event details. Contrarily when we will be adding a new event we wil pass an empty/null event id. In that case the from's fields will be empty and the user will need to complete it with te new event to add. 


* Edit the event.service.ts and add a new method 'addEvent' that will save our event in the db using the POST method.

```javascript
 addEvent(event: Event): Observable<any> {
    const headers = new HttpHeaders({
      "Content-Type": "application/json"
    });

    return this.http
      .post(environment.apiURL + "events/", event, { headers })
      .pipe(
        retry(3),
        catchError(this.handleError)
      );
  }
```

* Create a new component named add-edit-event

```javascript
 ng g component add-edit-event
```


* Copy and paste the following code in add-edit-event.component.html. As you can see we are using a form an the <a href="https://material.angular.io/components/form-field/overview">mat-form-field</a> form Ancular Material. The form is linked to 'addEditForm', a variable of type FormGroup declared in the controller, that will share its properties with the form fields throught the formControlName. Furthermore we have the 'save' button that will submit the form.

```javascript
<div class="container">
  <h2>Add Event</h2>
  <form
    novalidate
    [formGroup]="addEditForm"
    #fform="ngForm"
    (ngSubmit)="onSubmit()"
    *ngIf="addEditForm"
  >
    <mat-form-field>
      <input matInput placeholder="Title" type="text" formControlName="title" />
    </mat-form-field>
    <mat-form-field>
      <input matInput placeholder="Date" type="date" formControlName="date" />
    </mat-form-field>
    <mat-form-field>
      <input
        matInput
        placeholder="Location"
        type="text"
        formControlName="location"
      />
    </mat-form-field>

    <mat-form-field>
      <textarea
        matInput
        placeholder="Description"
        rows="4"
        type="text"
        formControlName="description"
      ></textarea>
    </mat-form-field>

    <div id="save-btn">
      <button mat-raised-button color="primary" type="submit">
        Save
      </button>
    </div>
  </form>
</div>

```

* Change the add-edit-event.component.ts as following. We are reading the parameter id that we are getting through the URL and decide then if we are adding or editing a new event. Then we are creating the form 'addEditForm' (previously we need to import 'FormBuilder' and 'FormGroup' in order to declare it)  and then declare and set their properties and values when required (in case that we are editing an event). Finally we have the 'submit()' method that will add or update the event through the 'EventService'.


```javascript
import { Component, OnInit } from "@angular/core";
import { FormBuilder, FormGroup } from "@angular/forms";
import { Event } from "../../models/event";
import { EventService } from "../../core/event.service";
import { Router } from "@angular/router";
import { ActivatedRoute } from "@angular/router";

@Component({
  selector: "oevents-add-edit-event",
  templateUrl: "./add-edit-event.component.html",
  styleUrls: ["./add-edit-event.component.scss"]
})
export class AddEditEventComponent implements OnInit {
  addEditForm: FormGroup;
  event: Event;

  constructor(
    private fb: FormBuilder,
    private eventService: EventService,
    private router: Router,
    private route: ActivatedRoute
  ) {}

  ngOnInit() {
    const id = this.route.snapshot.params["id"];

    if (id) {
      this.eventService.getEvent(id).subscribe((event: Event) => {
        console.log(event);
        this.event = event;
        this.createForm();
      });
    } else {
      this.createForm();
    }
  }

  createForm() {
    if (this.event) {
      this.addEditForm = this.fb.group({
        title: this.event.title,
        location: this.event.location,
        date: this.event.date,
        description: this.event.description,
        addedBy: this.event.addedBy
      });
    } else {
      this.addEditForm = this.fb.group({
        title: "",
        location: "",
        date: "",
        description: "",
        addedBy: ""
      });
    }
  }

  onSubmit() {
    this.event = this.addEditForm.value;
    if (this.event.id) {
      this.eventService.updateEvent(this.event).subscribe((event: Event) => {
        console.log(event);
        this.addEditForm.reset();
        this.router.navigate(["/events"]);
      });
    } else {
      this.eventService.addEvent(this.event).subscribe((event: Event) => {
        console.log(event);
        this.addEditForm.reset();
        this.router.navigate(["/events"]);
      });
    }
  }
}

```

* Add the following code in add-edit-event.component.scss

```javascript
.container {
  display: flex;
  justify-content: center;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
form {
  display: flex;
  flex-flow: column;
  width: 40%;
}

#save-btn {
  display: flex;
  justify-content: flex-end;
}

```



* In event-list.component.html add a button that will redirect to add event view. As we said before, we'll pass and empty string as the paramater because in this case we are creating a new event.

```javascript
<div class="container">
  <div id="add-event-btn">
    <button mat-raised-button color="primary" [routerLink]="['/addEditEvent/', '']">
      Add Event
    </button>
  </div>
  ...
```


* Save all the changes and try it out. You should be able to create and edit events. 


## Delete an event

* The last thing that we need to do in order to have a complete CRUD operational web app is to be able to delete an event. We will include the delete button in event-details.component.html.

```javascript
<div class="container">
  <div id="event-card-details" *ngIf="event">
    <div id="edit-event-btn">
      <button id="delete-btn" mat-raised-button color="warn" (click)="deleteEvent(event)" >
        Delete Event
      </button>

      <button mat-raised-button color="primary" [routerLink]="['/addEditEvent/', event.id]" >
        Edit Event
      </button>
      ...
```

* Then in the event.service.ts include the 'deleteEvent' method that will use the DELETE method to remove the event from the db. 

```javascript
  deleteEvent(id: string): Observable<any> {
    const headers = new HttpHeaders({
      "Content-Type": "application/json"
    });
    return this.http
      .delete(environment.apiURL + "events/" + id, { headers })
      .pipe(
        retry(3),
        catchError(this.handleError)
      );
  }
```

* Finally, in the event-details.component.ts add the delete event method.

```javascript
...
 ngOnInit() {
    const id = this.route.snapshot.params["id"];
    this.eventService.getEvent(id).subscribe((event: Event) => {
      console.log(event);
      this.event = event;
    });
  }

  deleteEvent(event: Event) {
    console.log(event);
    this.eventService.deleteEvent(event.id).subscribe(() => {
      console.log("Event Removed");
    });
    this.router.navigate(["/events"]);
  }
  ...


Save all the changes. Te application should allow you to delete events now.
