# Angular notes
## creating and importing custom components
### component with all files

    ng g c comp1
    
### component without css file

    ng g c comp1 --inline-css
### component without html file

    ng g c comp1 --inline-template
### without html, css file

    ng g c comp1 --inline-css --inline-template

### importing in app.module.js

    ...    
    import { AppComponent } from  './app.component';
    import { Comp1Component } from  './comp1/comp1.component';
    import { Comp2Component } from  './comp2/comp2.component';
    import { Comp3Component } from  './comp3/comp3.component';
    import { Comp4Component } from  './comp4/comp4.component';  
    
    @NgModule({
    declarations: [
    AppComponent,
    Comp1Component,
    Comp2Component,
    Comp3Component,
    Comp4Component
    ],
    
    imports: [
    BrowserModule
    ],
    providers: [],
    bootstrap: [AppComponent]
    })
    export  class  AppModule { }
### importing in app.component.html

    <app-comp1></app-comp1>
    <app-comp2></app-comp2>
    <app-comp3></app-comp3>
    <app-comp4></app-comp4>

## Using inline css

    @Component({
	    selector:  'app-comp2',
	    templateUrl:  './comp2.component.html',
	    styles: [
		    `.com2class{
		    background-color: gray;
		    text-align: center;
		    }`
	    ]
    })
## Using inline html

    @Component({
    selector:  'app-comp3',
    template:  `
	    <h2 class='com3class'>
	    component 3
	    </h2>
    `,
    styleUrls: ['./comp3.component.css']
    })
## creating and importing custom module
creating

    ng generate module mod1
importing in app.module.ts

    ...
    import {Mod1Module} from  './mod1/mod1.module'  
    
    ...  
    
    @NgModule({
    declarations: [
    AppComponent,
    ...
    ],
    imports: [
    BrowserModule, Mod1Module
    ],
    providers: [],
    bootstrap: [AppComponent]
    })
    export  class  AppModule { }
## creating custom component inside custom module

    ng g c mod1/mod1comp1
to use such components in app.component.html we need to export it through its parent module. 

> mod1.module.ts

    @NgModule({
    declarations: [
    M1comp1Component
    ],
    imports: [
    CommonModule
    ], exports: [M1comp1Component]
    })

> app.component.html

    <app-comp1></app-comp1>
    <app-comp2></app-comp2>
    <app-comp3></app-comp3>
    <app-comp4></app-comp4>
    <app-m1comp1></app-m1comp1>
## Calling functions on events

> app.component.html

    <h1>calling a function on event</h1>
    <button  (click)="giveName('Vikas Anand')">click me</button>

> app.component.ts

    export  class  AppComponent {
    title = 'tut1';
    greet = 'Hello'
    giveName(name:string){
    alert(this.greet + " " + name)
    }
    }    
calling a function on multiple events
> app.component.html

    <h1>{{title}}</h1>
    <h3>Mouse events</h3>
    <div  (click)="print('button clicked')">click event</div>
    <div  (mouseover)="print('mouse over')">mouseover event</div>
    <div  (mouseout)="print('mouse out')">mouseout event</div>
    <h3>Input field events</h3>
    <input  type="text"  #box1  placeholder="keydown event"  keydown)="print(box1.value)"><br>
    <input  type="text"  #box2  placeholder="keyup event"  (keyup)="print(box2.value)"><br>
    <input  type="text"  #box3  placeholder="focus out evevt"  (blur)="print(box3.value)"><br>
    <input  type="text"  #box4  placeholder="input event"  (input)="print(box4.value)"><br>

> app.component.ts

    export  class  AppComponent {
    title = 'calling function on events';
    print(name:string | number){
    console.log(name)
    }
    }
### getting value from input field and updating in DOM
> app.component.html

    <h1>{{title}}</h1>
    <input  type="text"  #box  placeholder="type anthing"  (keyup)="getValue1(box.value)">
    <button  (click)="getValue2(box.value)">click me</button>
    <div>Value is : {{displayOnChange}}</div>
    <div>Value is : {{displayOnClick}}</div>
> app.component.ts

    export  class  AppComponent {
    title = 'Get input box value';
    displayOnChange:string = '';
    displayOnClick:string = '';
    getValue1(name:string){
    this.displayOnChange = name;
    }
    getValue2(name:string){
    this.displayOnClick = name;
    }
    }
## Internal, inline, component, global styles
**inline style**

> app.component.html

    <h1  style="color:red;">Hello world !</h1>
**internal style**
> app.component.html

    <style>
	    h1{
	    color: red;
    }
    </style>
    <h1>Hello world !</h1>
**component style**
> app.component.css

    h1{
	    color: red;
    }
**global style**
> style.css

    h1{
	    color: red;
    }
***Priority order***
Inline > Internal > component > global
## Property binding
Property binding is used to make the property of html element dynamic , so that it can be changed from .ts file. This feature can also be achieved with interpolation but interpolation doesn't support boolean values. 
So, properties like (disabled='true') will not work as expected.

> app.component.html

    <!-- with help of interpolation -->
    <input  type="text"  placeholder="input1"  class={{cName}}>
    <!-- with help of property binding -->
    <input  type="text"  placeholder="input2"  [class]=cName>

> app.component.ts

    export  class  AppComponent {
	    cName = 'inp';
    }
## conditional rendering in html

> app.component.ts

    export  class  AppComponent {
	    title = 'Rendering based on condition'
	    cond = true;
    }

> app.component.html

    <h1>{{title}}</h1>  
    
    <!-- this will be rendered if cond == true -->
    <div  *ngIf="cond else elseBlock">
	    cond is true
    </div>  
    
    <!-- this will be rendered if cond == false -->
    <ng-template  #elseBlock>
	    <div>
		    cond is false
	    </div>
    </ng-template>
**another method**

    <h1>{{title}}</h1>
    <div  *ngIf="cond; then ifBlock else elseBlock"></div>  
    
    <!-- this will be rendered if cond == true -->
    <ng-template  #ifBlock>
	    <div>
		    cond is true
	    </div>
    </ng-template>  
    
    <!-- this will be rendered if cond == false -->
    <ng-template  #elseBlock>
	    <div>
		    cond is false
	    </div>
    </ng-template>

**conditional rendering with random variable**

> app.component.ts

    export  class  AppComponent {
	    title = 'Rendering based on condition'
	    color = 'blue';
    }

> app.component.html

    <h1>{{title}}</h1>  
    
    <!-- this will be rendered if color == red -->
    <ng-template  [ngIf]="color==='red'">
	    <div  style="color: red;">
		    Red color
	    </div>
    </ng-template>  
    
    <!-- this will be rendered if color == blue -->
    <ng-template  [ngIf]="color==='blue'">
	    <div  style="color: blue;">
		    Blue color
	    </div>
    </ng-template>  
    
    <!-- this will be rendered if color == green -->
    <ng-template  [ngIf]="color==='green'">
	    <div  style="color: green;">
		    Green color
	    </div>
    </ng-template>
**With help of switch**

    <h1>{{title}}</h1>  
    
    <div  [ngSwitch]="color">
	    <div  *ngSwitchCase="'red'"  style="color: red;">Red color</div>
	    <div  *ngSwitchCase="'blue'"  style="color: blue;">Blue color</div>
	    <div  *ngSwitchCase="'green'"  style="color: green;">Green color</div>
	    <div  *ngSwitchDefault>Unknown color</div>
    </div>
## Using loop in html
***ngFor** : It works like for each method in JavaScript.

> app.component.ts

    export  class  AppComponent {
    title = 'Rendering based on condition'
    user = [
	    {name:'vikas', desc:'hello vikas'},
	    {name:'vishal', desc:'hello vishal'},
	    {name:'vivek', desc:'hello vivek'},
	    {name:'vipul', desc:'hello vipul'}
    ];
    }

> app.component.html 

    <h1>{{title}}</h1>
    <div  *ngFor="let item of user">
	    <h3>{{item.name}}</h3>
	    <p>{{item.desc}}</p>
    </div>

    
