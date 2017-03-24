# Howto use Angular4 and PrimeNG in a Spring Boot app.

This repo doesn't contain any code, just a description on how to get stuff up and running, pulled 
togehter from other resources on the web.

## Install Angular if needed

Install Angular (currently version 4). You may have to run this as root or
homebrew or some id that has rights to do stuff in the brew/npm universe. -g
means install globally. You can skip it and install locally in your user as well.

```
npm install -g @angular/cli
```

##Create a Spring Boot project:

```
spring init --dependencies=web [name of the app]
```
You can also include jax-rs, jpa, postgresql etc.

## First initialize Angular with Angular-cli

https://angular.io/docs/ts/latest/cli-quickstart.html

Initialize Angular with angular-cli. Run from same folder as spring init.

```
ng new [name of the app]
```
Angular-cli will create stuff within the existing src folder and so on.
You will be asked if you want to overwrite ```.gitignore``` which is probably
a good idea, if you are accustomed to java .gitignores.

## Add PrimeNG

http://blogs.bytecode.com.au/glen/2016/10/27/primeng-with-angular-cli.html

Install PrimeNG and FontAwesome:

```
cd [name of the app]
npm install primeng --save
npm install font-awesome --save
```

Add the PrimeNG and font-awesome stylesheets (three lines) into ```.angular-cli.json```, it should look like this after editing:

```json
      "styles": [
        "styles.css",
        "../node_modules/primeng/resources/primeng.min.css",
        "../node_modules/primeng/resources/themes/omega/theme.css",
        "../node_modules/font-awesome/css/font-awesome.min.css"
      ],
```

Here you can change themes (replace ```omega```)!

Dependencies have been added into ```package.json``` for you:

```json
  "dependencies": {
    ...
    "font-awesome": "^4.7.0",
    "primeng": "^2.0.5",
    ...
  },
```

Now you should be able to run the default app that just says "app works!":

```
ng serve --open
```

## Add a button with click-listener
### Fixing ```app.module.ts``` (in ```src/app```)
We want to import a PrimeNG Button. This happens in ``app.module.ts``. Initially this looks like

```typescript
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { FormsModule } from '@angular/forms';
import { HttpModule } from '@angular/http';

import { AppComponent } from './app.component';

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    FormsModule,
    HttpModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }

```

We will first have to 

1. Add an ```import`` statement for ```ButtonModule```.
1. Add the imported PrimeNG module to the ```imports:``` list.

The file should then look like:

```
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { FormsModule } from '@angular/forms';
import { HttpModule } from '@angular/http';

// primeng stuff
import {ButtonModule} from 'primeng/primeng';

import { AppComponent } from './app.component';

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    FormsModule,
    HttpModule,
    ButtonModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

### Add button to html in ```app.component.html```

After adding the button it should look somewhat like this (only the last line is added, the title was added by Angular-cli):

```html
<h1>
  {{title}}
</h1>
<button pButton type="button" (click)="buttClick()" label="Click">Clickus dickus</button>
```

### Adding click handler in ```app.component.ts```

After addomg the ```buttClick()``` method it should look like this:

```
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  title = 'app works!';

  buttClick() {
    this.title = 'Button Clicked!';
  }
  
}
```

Everything else in the file was generated by Angular-cli.

### Run the application and click the button

```
ng serve --open
```

The button should be styled somehow, and the title should change to "Button Clicked!" when the button is clicked.`

## Running with Spring Boot

https://dzone.com/articles/recipe-for-getting-started-with-spring-boot-and-an

### Change angular-cli build location in ```angular-cli.json```

The file has a section that looks like

```
  "apps": [
    {
      "root": "src",
      "outDir": "dist",
      "assets": [
        "assets",
        "favicon.ico"
      ],
```

We must change ```outDir``` to ```src/main/resources/static``` so that it looks like this:

```
  "apps": [
    {
      "root": "src",
      "outDir": "src/main/resources/static",
      "assets": [
        "assets",
        "favicon.ico"
      ],
```

This makes Spring webserver able to serve the stuff. First we need to build (happens automatically with ```ng serve```):

```
ng build
```

Also make sure ```pom.xml``` contains

```
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>
```

And run with Spring Boot:

```mvn spring-boot:run```



