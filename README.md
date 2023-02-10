# Angular Leaflet


Application example built with [Angular](https://angular.io/) 15 and adding the map Leaflet component using the [leaflet](https://www.npmjs.com/package/leaflet) library.

This tutorial was posted on my [blog](https://rodrigo.kamada.com.br/blog/adicionando-o-componente-de-mapa-leaflet-em-uma-aplicacao-angular) in portuguese and on the [DEV Community](https://dev.to/rodrigokamada/adding-the-map-leaflet-component-to-an-angular-application-d13) in english.



[![Website](https://shields.braskam.com/v1/shields?name=website&format=rectangle&size=small&radius=5)](https://rodrigo.kamada.com.br)
[![LinkedIn](https://shields.braskam.com/v1/shields?name=linkedin&format=rectangle&size=small&radius=5)](https://www.linkedin.com/in/rodrigokamada)
[![Twitter](https://shields.braskam.com/v1/shields?name=twitter&format=rectangle&size=small&radius=5&socialAccount=rodrigokamada)](https://twitter.com/rodrigokamada)
[![Instagram](https://shields.braskam.com/v1/shields?name=instagram&format=rectangle&size=small&radius=5)](https://www.instagram.com/rodrigokamada/)



## Prerequisites


Before you start, you need to install and configure the tools:

* [git](https://git-scm.com/)
* [Node.js and npm](https://nodejs.org/)
* [Angular CLI](https://angular.io/cli)
* IDE (e.g. [Visual Studio Code](https://code.visualstudio.com/) or [WebStorm](https://www.jetbrains.com/webstorm/))



## Getting started


### Create and configure the account on the Mapbox


**1.** Let's create the account. Access the site [https://www.mapbox.com/](https://www.mapbox.com/) and click on the button *Sign up*.

![Mapbox - Home page](https://res.cloudinary.com/rodrigokamada/image/upload/v1637580196/Blog/angular-leaflet/mapbox-step1.png)

**2.** Fill in the fields *Username*, *Email*, *Password*, *First name*, *Last name*, click on the checkbox *I agree to the Mapbox Terms of Service and Privacy Policy.* and click on the button *Get started*.

![Mapbox - Sign up](https://res.cloudinary.com/rodrigokamada/image/upload/v1637580197/Blog/angular-leaflet/mapbox-step2.png)

**3.** Check the registered email.

![Mapbox - Check email](https://res.cloudinary.com/rodrigokamada/image/upload/v1637580197/Blog/angular-leaflet/mapbox-step3.png)

**4.** Click on the link in the email sent.

![Mapbox - Confirm email](https://res.cloudinary.com/rodrigokamada/image/upload/v1637580197/Blog/angular-leaflet/mapbox-step4.png)

**5.** Copy the token displayed in the *Dashboard* menu and, in my case, the token was displayed `pk.eyJ1IjoiYnJhc2thbSIsImEiOiJja3NqcXBzbWoyZ3ZvMm5ybzA4N2dzaDR6In0.RUAYJFnNgOnnZAw` because this token will be configured in the Angular application.

![Mapbox - Dashboard](https://res.cloudinary.com/rodrigokamada/image/upload/v1637580197/Blog/angular-leaflet/mapbox-step5.png)

**6.** Ready! Account created and token generated.


### Create the Angular application


**1.** Let's create the application with the Angular base structure using the `@angular/cli` with the route file and the SCSS style format.

```powershell
ng new angular-leaflet
? Would you like to add Angular routing? Yes
? Which stylesheet format would you like to use? SCSS   [ https://sass-lang.com/documentation/syntax#scss                ]
CREATE angular-leaflet/README.md (1073 bytes)
CREATE angular-leaflet/.editorconfig (274 bytes)
CREATE angular-leaflet/.gitignore (604 bytes)
CREATE angular-leaflet/angular.json (3339 bytes)
CREATE angular-leaflet/package.json (1090 bytes)
CREATE angular-leaflet/tsconfig.json (783 bytes)
CREATE angular-leaflet/.browserslistrc (703 bytes)
CREATE angular-leaflet/karma.conf.js (1445 bytes)
CREATE angular-leaflet/tsconfig.app.json (287 bytes)
CREATE angular-leaflet/tsconfig.spec.json (333 bytes)
CREATE angular-leaflet/src/favicon.ico (948 bytes)
CREATE angular-leaflet/src/index.html (313 bytes)
CREATE angular-leaflet/src/main.ts (372 bytes)
CREATE angular-leaflet/src/polyfills.ts (2820 bytes)
CREATE angular-leaflet/src/styles.scss (80 bytes)
CREATE angular-leaflet/src/test.ts (788 bytes)
CREATE angular-leaflet/src/assets/.gitkeep (0 bytes)
CREATE angular-leaflet/src/environments/environment.prod.ts (51 bytes)
CREATE angular-leaflet/src/environments/environment.ts (658 bytes)
CREATE angular-leaflet/src/app/app-routing.module.ts (245 bytes)
CREATE angular-leaflet/src/app/app.module.ts (393 bytes)
CREATE angular-leaflet/src/app/app.component.scss (0 bytes)
CREATE angular-leaflet/src/app/app.component.html (24617 bytes)
CREATE angular-leaflet/src/app/app.component.spec.ts (1139 bytes)
CREATE angular-leaflet/src/app/app.component.ts (233 bytes)
✔ Packages installed successfully.
    Successfully initialized git.
```

**2.** Install and configure the Bootstrap CSS framework. Do steps 2 and 3 of the post *[Adding the Bootstrap CSS framework to an Angular application](https://github.com/rodrigokamada/angular-bootstrap)*.

**3.** Configure the Mapbox token in the `src/environments/environment.ts` and `src/environments/environment.prod.ts` files as below.

```typescript
mapbox: {
  accessToken: 'pk.eyJ1IjoiYnJhc2thbSIsImEiOiJja3NqcXBzbWoyZ3ZvMm5ybzA4N2dzaDR6In0.RUAYJFnNgOnn80wXkrV9ZA',
},
```

**4.** Create the `src/assets/images` folder and copy the `marker-icon.png` and `marker-shadow.png` files.

![Marker Icon](https://res.cloudinary.com/rodrigokamada/image/upload/v1637581626/Blog/angular-leaflet/marker-icon.png)
![Marker Shadow](https://res.cloudinary.com/rodrigokamada/image/upload/v1637581626/Blog/angular-leaflet/marker-shadow.png)

**5.** Install the `leaflet` and `@types/leaflet` libraries.

```powershell
npm install leaflet @types/leaflet
```

**6.** Configure the `leaflet` library. Change the `angular.json` file and add the `leaflet.css` file as below.

```json
"styles": [
  "node_modules/bootstrap/scss/bootstrap.scss",
  "node_modules/bootstrap-icons/font/bootstrap-icons.css",
  "node_modules/leaflet/dist/leaflet.css",
  "src/styles.scss"
],
```

**7.** Remove the contents of the `AppComponent` class from the `src/app/app.component.ts` file. Import the `leaflet` service and create the `getCurrentPosition`, `loadMap` methods as below.

```typescript
import { AfterViewInit, Component } from '@angular/core';
import { Observable, Subscriber } from 'rxjs';
import * as L from 'leaflet';

import { environment } from '../environments/environment';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.scss'],
})
export class AppComponent implements AfterViewInit {

  map: any;

  constructor() {
  }

  public ngAfterViewInit(): void {
    this.loadMap();
  }

  private getCurrentPosition(): any {
    return new Observable((observer: Subscriber<any>) => {
      if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition((position: any) => {
          observer.next({
            latitude: position.coords.latitude,
            longitude: position.coords.longitude,
          });
          observer.complete();
        });
      } else {
        observer.error();
      }
    });
  }

  private loadMap(): void {
    this.map = L.map('map').setView([0, 0], 1);
    L.tileLayer('https://api.mapbox.com/styles/v1/{id}/tiles/{z}/{x}/{y}?access_token={accessToken}', {
      attribution: 'Map data &copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors, Imagery © <a href="https://www.mapbox.com/">Mapbox</a>',
      maxZoom: 18,
      id: 'mapbox/streets-v11',
      tileSize: 512,
      zoomOffset: -1,
      accessToken: environment.mapbox.accessToken,
    }).addTo(this.map);

    this.getCurrentPosition()
    .subscribe((position: any) => {
      this.map.flyTo([position.latitude, position.longitude], 13);

      const icon = L.icon({
        iconUrl: 'assets/images/marker-icon.png',
        shadowUrl: 'assets/images/marker-shadow.png',
        popupAnchor: [13, 0],
      });

      const marker = L.marker([position.latitude, position.longitude], { icon }).bindPopup('Angular Leaflet');
      marker.addTo(this.map);
    });
  }

}
```

**8.** Remove the contents of the `src/app/app.component.html` file. Add the map `div` tag as below.

```html
<div class="container-fluid py-3">
  <h1>Angular Leaflet</h1>

  <div id="map"></div>
</div>
```

**9.** Add the style in the `src/app/app.component.scss` file as below.

```css
#map {
  height: 400px;
  width: 100%;
  max-width: 600px;
}
```

**10.** Run the application with the command below.

```powershell
npm start

> angular-leaflet@1.0.0 start
> ng serve

✔ Browser application bundle generation complete.

Initial Chunk Files | Names         |      Size
vendor.js           | vendor        |   2.81 MB
styles.css          | styles        | 280.54 kB
polyfills.js        | polyfills     | 128.51 kB
scripts.js          | scripts       |  76.67 kB
main.js             | main          |  12.03 kB
runtime.js          | runtime       |   6.63 kB

                    | Initial Total |   3.30 MB

Build at: 2021-08-20T10:40:47.188Z - Hash: 030dfe6c9ea7ff5d80c2 - Time: 12256ms

** Angular Live Development Server is listening on localhost:4200, open your browser on http://localhost:4200/ **


✔ Compiled successfully.
```

**11.** Ready! Access the URL `http://localhost:4200/` and check if the application is working. See the application working on [GitHub Pages](https://rodrigokamada.github.io/angular-leaflet/) and [Stackblitz](https://stackblitz.com/edit/angular15-leaflet).

![Angular Leaflet](https://res.cloudinary.com/rodrigokamada/image/upload/v1637580179/Blog/angular-leaflet/angular-leaflet.png)



## Cloning the application

**1.** Clone the repository.

```powershell
git clone git@github.com:rodrigokamada/angular-leaflet.git
```

**2.** Install the dependencies.

```powershell
npm ci
```

**3.** Run the application.

```powershell
npm start
```
