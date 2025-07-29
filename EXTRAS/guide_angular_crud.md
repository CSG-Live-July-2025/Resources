# Guide: Angular CRUD practice

## HTTP Client: Angular HttpClient vs Axios

**Important Note for React/Vue Developers:** If you're coming from React or Vue.js where Axios is commonly used, you might wonder why we're not using Axios in Angular. Here's why:

- **Angular HttpClient is the standard** - It's built into Angular and designed specifically for the framework
- **Returns Observables, not Promises** - This integrates perfectly with Angular's reactive patterns and async pipe
- **No additional dependencies** - Keeps your bundle size smaller
- **Built-in features** - Includes interceptors, XSRF protection, and TypeScript support out of the box
- **Angular ecosystem integration** - Works seamlessly with Angular's change detection and lifecycle

While you *can* use Axios in Angular, HttpClient is the conventional and recommended approach that you'll see in all Angular documentation and tutorials.

## Code Change Format

**Important:** This guide uses diff format to show code changes. When you see:
```diff
  existing code...
+ new line to add
- old line to remove
```
Only copy the actual code lines (without `+` or `-` prefixes) into your files.

## Backend setup

- In your terminal, navigate to your Rails backend app and run your server: `rails server`
- Make sure each RESTful route (index, create, show, update, destroy) is working by manually testing each request.
- Make sure to configure the [rack-cors](https://github.com/cyu/rack-cors) gem to allow web requests from `'http://localhost:4200'`.

## Frontend setup

**Important Note:** Angular uses TypeScript by default, not plain JavaScript. TypeScript is a superset of JavaScript that adds static type checking and other features. If you're coming from a JavaScript background, don't worry - the syntax is very similar, and TypeScript helps catch errors early and provides better IDE support. All the code examples in this guide use TypeScript.

- In a new terminal window, navigate to your Actualize directory and create a new project:
  ```
  npm create vite@latest
  ```

  _When prompted:_
  - _Enter your project name (e.g., `my-angular-app`)_
  - _Select "Angular" as the framework_
  - _Select "Angular" as the variant_
  - _Answer the following prompts:_
    - _Would you like to share pseudonymous usage data? Choose your preference_
    - _Do you want to create a 'zoneless' application? **No**_
    - _Do you want to enable Server-Side Rendering (SSR) and Static Site Generation (SSG/Prerendering)? **No**_

- In the terminal, go to your new directory, install dependencies:
  ```
  cd my-angular-app
  npm install
  ```

- **Important Setup Step**: Modern Angular (17+) uses standalone components by default, but this guide teaches the traditional NgModule approach which is still widely used and better for learning Angular's architecture. If you see errors about "Component X is standalone, and cannot be declared in an NgModule", follow these steps to convert your app to use NgModules:

  - Install the required dependency:
    ```
    npm install @angular/platform-browser-dynamic
    ```

  - Replace the contents of **src/main.ts**:
    ```typescript
    import { platformBrowserDynamic } from '@angular/platform-browser-dynamic';
    import { AppModule } from './app/app.module';

    platformBrowserDynamic().bootstrapModule(AppModule)
      .catch((err: any) => console.error(err));
    ```

    **Note:** At this point, you'll see a linter error about `Cannot find module './app/app.module'` - this is expected since we haven't created the `app.module.ts` file yet. The error will be resolved in the next steps when we create the module file.

  - Delete these files if they exist (they're for standalone setup):
    ```
    rm src/app/app.ts
    rm src/app/app.config.ts  
    rm src/app/app.routes.ts
    ```

  - **For Angular 20+ users**: Generate components using Angular CLI (recommended):
    ```
    npx ng generate component header --standalone=false --skip-tests --inline-template --inline-style --skip-import
    npx ng generate component footer --standalone=false --skip-tests --inline-template --inline-style --skip-import
    npx ng generate component photos-page --standalone=false --skip-tests --inline-template --inline-style --skip-import
    ```

    **Note**: We use the `--skip-import` flag because we haven't created the `app.module.ts` file yet, so Angular CLI can't automatically add the components to a module. We'll manually add them to the module when we create it in the next step.

    **Note**: Angular CLI will generate components in subdirectories with class names like `Header`, `Footer`, `PhotosPage` (not `HeaderComponent`, etc.).

    **Important**: If you encounter an error like "Component X is standalone, and cannot be declared in an NgModule", make sure all your components have `standalone: false` in their `@Component` decorator. This is crucial for the NgModule approach we're using in this guide.

  - **Create the new app component**: Create a new file **src/app/app.component.ts**:
    ```typescript
    import { Component } from '@angular/core';

    @Component({
      selector: 'app-root',
      standalone: false,  // Important: Add this for NgModule compatibility
      template: `
        <div>
          <h1>Welcome to Angular!</h1>
        </div>
      `
    })
    export class AppComponent { }
    ```

  - **Create the app module**: Create a new file **src/app/app.module.ts**:

    ```typescript
    import { NgModule } from '@angular/core';
    import { BrowserModule } from '@angular/platform-browser';
    import { HttpClientModule } from '@angular/common/http';
    import { FormsModule } from '@angular/forms';

    import { AppComponent } from './app.component';
    import { Header } from './header/header';
    import { PhotosPage } from './photos-page/photos-page';
    import { Footer } from './footer/footer';

    @NgModule({
      declarations: [
        AppComponent,
        Header,
        PhotosPage,
        Footer
      ],
      imports: [
        BrowserModule,
        HttpClientModule,
        FormsModule
      ],
      providers: [],
      bootstrap: [AppComponent]
    })
    export class AppModule { }
    ```

    **Note:** Creating this file resolves the linter error in `main.ts` since the `AppModule` now exists and can be imported.

  - Update the generated components with the guide's templates (see component sections below)

- Now start the dev server:
  ```
  npm start
  ```



- In the terminal, open your text editor for the frontend app:
  ```
  code .
  ```

  > **Note**: We're using Angular CLI to generate components with `ng generate component` (or `ng g c` for short). This is the recommended approach for Angular development. The CLI creates components with proper Angular conventions and handles the NgModule declarations automatically. We use the `--standalone=false --skip-tests --inline-template --inline-style` flags to match this tutorial's structure.
  
- **Update the generated components**: Replace the default template content in each generated component:

  **src/app/header/header.ts**:
  ```typescript
  import { Component } from '@angular/core';

  @Component({
    selector: 'app-header',
    standalone: false,
    template: `
      <header>
        <nav>
          <a href="#" onclick="return false;">Photos</a> | <a href="#" onclick="return false;">New Photo</a>
        </nav>
      </header>
    `,
    styles: ``
  })
  export class Header { }
  ```

  _Note: The header links use `href="#"` for now - we'll wire these up with `routerLink` after we enable routing. You can ignore any broken links or page refreshes until we reach the Routes section below._

  **src/app/photos-page/photos-page.ts**:
  ```typescript
  import { Component } from '@angular/core';

  @Component({
    selector: 'app-photos-page',
    standalone: false,
    template: `
      <main>
        <h1>Welcome to Angular!</h1>
      </main>
    `,
    styles: ``
  })
  export class PhotosPage { }
  ```

  **src/app/footer/footer.ts**:
  ```typescript
  import { Component } from '@angular/core';

  @Component({
    selector: 'app-footer',
    standalone: false,
    template: `
      <footer>
        <p>Copyright {{ today | date:'yyyy' }}</p>
      </footer>
    `,
    styles: ``
  })
  export class Footer {
    today = new Date();
  }
  ```

  > **💡 Strict Mode Note:** If you scaffolded with `ng new my-angular-app --strict`, you'll need to import `FormsModule` in any standalone components, or consider using reactive forms for better type safety.

- **Update the app component**: Update **src/app/app.component.ts** to include the other components:

  ```typescript
  import { Component } from '@angular/core';

  @Component({
    selector: 'app-root',
    standalone: false,  // Important: Add this for NgModule compatibility
    template: `
      <div>
        <app-header></app-header>
        <app-photos-page></app-photos-page>
        <app-footer></app-footer>
      </div>
    `
  })
  export class AppComponent { }
  ```

- In your text editor, open the file **src/styles.css** and replace all the CSS code with the following:

  ```css
  html {
    scroll-behavior: smooth;
  }
  ```
  
- In your browser, visit http://localhost:4200 to make sure your app is working!

  > In your terminal, use git to save a version of your code.
  > ```bash
  > git init
  > git add --all
  > git commit -m "Initial commit"
  > ```

## Create shared models

- In your text editor, create a new directory **src/app/models/** and a new file **src/app/models/photo.ts**:

  ```typescript
  export interface Photo {
    id: number;
    name: string;
    url: string;
    width: number;
    height: number;
  }
  ```


## CRUD Single Page Application

- Index action

  - <details><summary>Make a new <strong>src/app/photos-index.component.ts</strong> file that returns placeholder HTML</strong></summary>
  
      ```typescript
      import { Component } from '@angular/core';

      @Component({
        selector: 'app-photos-index',
        standalone: false,
        template: `
          <div>
            <h1>All photos</h1>
          </div>
        `
      })
      export class PhotosIndexComponent { }
      ```
      </details>

  - <details><summary>In <strong>src/app/app.module.ts</strong>, import the <strong>PhotosIndexComponent</strong></summary>
      
      ```diff
        import { NgModule } from '@angular/core';
        import { BrowserModule } from '@angular/platform-browser';
        import { HttpClientModule } from '@angular/common/http';
        import { FormsModule } from '@angular/forms';

        import { AppComponent } from './app.component';
        import { HeaderComponent } from './header.component';
        import { PhotosPageComponent } from './photos-page.component';
        import { FooterComponent } from './footer.component';
      + import { PhotosIndexComponent } from './photos-index.component';

        @NgModule({
          declarations: [
            AppComponent,
            HeaderComponent,
            PhotosPageComponent,
            FooterComponent,
      +     PhotosIndexComponent
          ],
          imports: [
            BrowserModule,
            HttpClientModule,
            FormsModule
          ],
          providers: [],
          bootstrap: [AppComponent]
        })
        export class AppModule { }
      ```
      </details>

  - <details><summary>In <strong>src/app/photos-page.component.ts</strong>, import and use the <strong>PhotosIndexComponent</strong></summary>
      
      ```diff
        import { Component } from '@angular/core';

        @Component({
          selector: 'app-photos-page',
          template: `
            <main>
      -       <h1>Welcome to Angular!</h1>
      +       <app-photos-index></app-photos-index>
            </main>
          `
        })
        export class PhotosPageComponent { }
      ```
      </details>
      
    > Check the browser to see the placeholder HTML from the index component.
    
  - In <strong>src/app/photos-page/photos-page.ts</strong>, define placeholder data and pass it to the index component:
      
      ```diff
        import { Component } from '@angular/core';
      + import { Photo } from '../models/photo';

        @Component({
          selector: 'app-photos-page',
          standalone: false,
          template: `
            <main>
      -       <app-photos-index></app-photos-index>
      +       <app-photos-index [photos]="photos"></app-photos-index>
            </main>
          `,
          styles: ``
        })
      - export class PhotosPage { }
      + export class PhotosPage {
      +   photos: Photo[] = [
      +     {id: 1, name: "First", url: "https://via.placeholder.com/150", width: 150, height: 150},
      +     {id: 2, name: "Second", url: "https://via.placeholder.com/300", width: 300, height: 300},
      +   ];
      + }
      ```
      
  - In <strong>src/app/photos-index.component.ts</strong>, accept and display the photos input:
      
      ```diff
      - import { Component } from '@angular/core';
      + import { Component, Input } from '@angular/core';
      + import { Photo } from './models/photo';

        @Component({
          selector: 'app-photos-index',
          template: `
            <div>
      -       <h1>All photos</h1>
      +       <h1>All photos ({{photos.length}} total)</h1>
            </div>
          `
        })
        export class PhotosIndexComponent {
      +   @Input() photos: Photo[] = [];
        }
      ```
      
    > Check the browser to see the data count in the index component.
      
  - In <strong>src/app/photos-index.component.ts</strong>, loop through the array given in inputs:
      
      ```diff
        import { Component, Input } from '@angular/core';
        import { Photo } from './models/photo';

        @Component({
          selector: 'app-photos-index',
          template: `
            <div>
              <h1>All photos ({{photos.length}} total)</h1>
      +       <div *ngFor="let photo of photos">
      +         <h2>{{photo.name}}</h2>
      +         <img [src]="photo.url" />
      +         <p>Width: {{photo.width}}</p>
      +         <p>Height: {{photo.height}}</p>
      +       </div>
            </div>
          `
        })
        export class PhotosIndexComponent { 
          @Input() photos: Photo[] = [];
        }
      ```

      ☝️ **Important:** Make sure the `interface Photo` is only defined in `models/photo.ts` and never redeclared in components.
      
    > Check the browser to see the placeholder data in the index component.
    
    > Test in the browser to see the hardcoded photos displayed in the index component.

- Show action

  - In <strong>src/app/photos-page/photos-page.ts</strong>, make a placeholder show function and pass it to the index component:
  
    ```diff
      import { Component } from '@angular/core';
      import { Photo } from '../models/photo';

      @Component({
        selector: 'app-photos-page',
        standalone: false,
        template: `
          <main>
    -       <app-photos-index [photos]="photos"></app-photos-index>
    +       <app-photos-index [photos]="photos" (show)="handleShow($event)"></app-photos-index>
          </main>
        `,
        styles: ``
      })
      export class PhotosPage {
        photos: Photo[] = [
          {id: 1, name: "First", url: "https://via.placeholder.com/150", width: 150, height: 150},
          {id: 2, name: "Second", url: "https://via.placeholder.com/300", width: 300, height: 300},
        ];

    +   handleShow(photo: Photo) {
    +     console.log("handleShow", photo);
    +   }
      }
    ```

  - In <strong>src/app/photos-index.component.ts</strong>, make a button to run the show function:
  
    ```diff
    - import { Component, Input } from '@angular/core';
    + import { Component, Input, Output, EventEmitter } from '@angular/core';
      import { Photo } from './models/photo';

      @Component({
        selector: 'app-photos-index',
        standalone: false,
        template: `
          <div>
            <h1>All photos ({{photos.length}} total)</h1>
            <div *ngFor="let photo of photos">
              <h2>{{photo.name}}</h2>
              <img [src]="photo.url" />
              <p>Width: {{photo.width}}</p>
              <p>Height: {{photo.height}}</p>
    +         <button (click)="show.emit(photo)">More info</button>
            </div>
          </div>
        `
      })
      export class PhotosIndexComponent { 
        @Input() photos: Photo[] = [];
    +   @Output() show = new EventEmitter<Photo>();
      }
    ```
    
    > Check the browser console to see the message when you click each button.

  - Make a new <strong>src/app/modal.component.ts</strong> file with the following content:
  
    ```typescript
    import { Component, Input, Output, EventEmitter } from '@angular/core';

    @Component({
      selector: 'app-modal',
      standalone: false,
      template: `
        <div *ngIf="show" class="modal-background">
          <section class="modal-main">
            <ng-content></ng-content>
            <button class="close" type="button" (click)="close.emit()">
              &#x2715;
            </button>
          </section>
        </div>
      `,
      styles: [`
        .modal-background {
          display: block;
          position: fixed;
          top: 0;
          left: 0;
          width:100%;
          height: 100%;
          background: rgba(0, 0, 0, 0.6);
          z-index: 1000;
        }

        .modal-main {
          position: fixed;
          background: white;
          width: 80%;
          height: auto;
          top: 50%;
          left: 50%;
          transform: translate(-50%,-50%);
          padding: 1em;
        }

        .modal-main button.close {
          font-size: 2em;
          background: none;
          border: none;
          position: absolute;
          top: 0em;
          right: 0.2em;
        }
      `]
    })
    export class ModalComponent {
      @Input() show: boolean = false;
      @Output() close = new EventEmitter();
    }
    ```

  - In <strong>src/app/app.module.ts</strong>, import the modal component:
  
    ```diff
      import { NgModule } from '@angular/core';
      import { BrowserModule } from '@angular/platform-browser';
      import { HttpClientModule } from '@angular/common/http';
      import { FormsModule } from '@angular/forms';

      import { AppComponent } from './app.component';
      import { Header } from './header/header';
      import { PhotosPage } from './photos-page/photos-page';
      import { Footer } from './footer/footer';
      import { PhotosIndexComponent } from './photos-index.component';
    + import { ModalComponent } from './modal.component';

      @NgModule({
        declarations: [
          AppComponent,
          Header,
          PhotosPage,
          Footer,
          PhotosIndexComponent,
    +     ModalComponent
        ],
        imports: [
          BrowserModule,
          HttpClientModule,
          FormsModule
        ],
        providers: [],
        bootstrap: [AppComponent]
      })
      export class AppModule { }
    ```

  - In <strong>src/app/photos-page/photos-page.ts</strong>, make state and functions to show and hide the modal:
  
    ```diff
      import { Component } from '@angular/core';
      import { Photo } from '../models/photo';

      @Component({
        selector: 'app-photos-page',
        standalone: false,
        template: `
          <main>
            <app-photos-index [photos]="photos" (show)="handleShow($event)"></app-photos-index>
    +       <app-modal [show]="isPhotosShowVisible" (close)="isPhotosShowVisible = false">
    +         <h1>Test</h1>
    +       </app-modal>
          </main>
        `,
        styles: ``
      })
      export class PhotosPage {
        photos: Photo[] = [
          {id: 1, name: "First", url: "https://via.placeholder.com/150", width: 150, height: 150},
          {id: 2, name: "Second", url: "https://via.placeholder.com/300", width: 300, height: 300},
        ];
    +   isPhotosShowVisible: boolean = false;
    +   currentPhoto: Photo = {} as Photo;

        handleShow(photo: Photo) {
          console.log("handleShow", photo);
    +     this.isPhotosShowVisible = true;
    +     this.currentPhoto = photo;
        }
      }
    ```
    
    > Check the browser to test showing and closing the modal with the appropriate buttons

  - Make a new <strong>src/app/photos-show.component.ts</strong> file that renders attributes from props:
  
    ```typescript
    import { Component, Input } from '@angular/core';
    import { Photo } from './models/photo';

    @Component({
      selector: 'app-photos-show',
      standalone: false,
      template: `
        <div>
          <h1>Photo information</h1>
          <p>Name: {{photo.name}}</p>
          <p>Url: {{photo.url}}</p>
          <p>Width: {{photo.width}}</p>
          <p>Height: {{photo.height}}</p>
        </div>
      `
    })
    export class PhotosShowComponent {
      @Input() photo: Photo = {} as Photo;
    }
    ```

  - In <strong>src/app/app.module.ts</strong>, import the show component:
  
    ```diff
      import { NgModule } from '@angular/core';
      import { BrowserModule } from '@angular/platform-browser';
      import { HttpClientModule } from '@angular/common/http';
      import { FormsModule } from '@angular/forms';

      import { AppComponent } from './app.component';
      import { Header } from './header/header';
      import { PhotosPage } from './photos-page/photos-page';
      import { Footer } from './footer/footer';
      import { PhotosIndexComponent } from './photos-index.component';
      import { ModalComponent } from './modal.component';
    + import { PhotosShowComponent } from './photos-show.component';

      @NgModule({
        declarations: [
          AppComponent,
          Header,
          PhotosPage,
          Footer,
          PhotosIndexComponent,
          ModalComponent,
    +     PhotosShowComponent
        ],
        imports: [
          BrowserModule,
          HttpClientModule,
          FormsModule
        ],
        providers: [],
        bootstrap: [AppComponent]
      })
      export class AppModule { }
    ```

  - In <strong>src/app/photos-page/photos-page.ts</strong>, use the show component in the modal:
  
    ```diff
      import { Component } from '@angular/core';
      import { Photo } from '../models/photo';

      @Component({
        selector: 'app-photos-page',
        standalone: false,
        template: `
          <main>
            <app-photos-index [photos]="photos" (show)="handleShow($event)"></app-photos-index>
            <app-modal [show]="isPhotosShowVisible" (close)="isPhotosShowVisible = false">
    -         <h1>Test</h1>
    +         <app-photos-show [photo]="currentPhoto"></app-photos-show>
            </app-modal>
          </main>
        `,
        styles: ``
      })
      export class PhotosPage {
        photos: Photo[] = [
          {id: 1, name: "First", url: "https://via.placeholder.com/150", width: 150, height: 150},
          {id: 2, name: "Second", url: "https://via.placeholder.com/300", width: 300, height: 300},
        ];
        isPhotosShowVisible: boolean = false;
        currentPhoto: Photo = {} as Photo;

        handleShow(photo: Photo) {
          console.log("handleShow", photo);
          this.isPhotosShowVisible = true;
          this.currentPhoto = photo;
        }
      }
    ```
    
    > Check the browser to see the selected item attributes within the modal
    > 
    > In your terminal, use git to save a version of your code.
    > ```bash
    > git add --all
    > git commit -m "Add show action"
    > ```

## Add Service Layer for Dynamic Data

Now that you understand how the components work together, let's make the app dynamic by connecting it to a backend API using Angular's HttpClient. We'll start by making the Index and Show operations dynamic, then add Create, Update, and Delete functionality.

- **Create the service layer**: In your text editor, create a new directory **src/app/services/** and a new file **src/app/services/photos.service.ts**:

  ```typescript
  import { Injectable } from '@angular/core';
  import { HttpClient } from '@angular/common/http';
  import { Observable } from 'rxjs';
  import { Photo } from '../models/photo';

  @Injectable({
    providedIn: 'root'
  })
  export class PhotosService {
    private baseUrl = 'http://localhost:3000/photos.json';
    // 💡 For production: move this to environment.ts (e.g., environment.apiUrl)
    // 📂 Import paths: Use ./models/photo from same directory, ../models/photo from subdirectories

    constructor(private http: HttpClient) {}

    getPhotos(): Observable<Photo[]> {
      return this.http.get<Photo[]>(this.baseUrl);
    }

    createPhoto(params: any): Observable<Photo> {
      return this.http.post<Photo>(this.baseUrl, params);
    }

    updatePhoto(id: number, params: any): Observable<Photo> {
      return this.http.patch<Photo>(`http://localhost:3000/photos/${id}.json`, params);
    }

    deletePhoto(id: number): Observable<any> {
      return this.http.delete(`http://localhost:3000/photos/${id}.json`);
    }
  }
  ```

  > **Note**: Angular's "fat service, thin component" pattern keeps HTTP logic in services rather than components. In production apps, you'd also add error handling with `catchError` and subscription management with `takeUntil` to prevent memory leaks.

- **Update PhotosPage to use the service**: Replace the hardcoded data with HTTP requests in **src/app/photos-page/photos-page.ts**:

  ```diff
  - import { Component } from '@angular/core';
  + import { Component, OnInit } from '@angular/core';
    import { Photo } from '../models/photo';
  + import { PhotosService } from '../services/photos.service';

    @Component({
      selector: 'app-photos-page',
      standalone: false,
      template: `
        <main>
          <app-photos-index [photos]="photos" (show)="handleShow($event)"></app-photos-index>
          <app-modal [show]="isPhotosShowVisible" (close)="isPhotosShowVisible = false">
            <app-photos-show [photo]="currentPhoto"></app-photos-show>
          </app-modal>
        </main>
      `,
      styles: ``
    })
  - export class PhotosPage {
  + export class PhotosPage implements OnInit {
  -   photos: Photo[] = [
  -     {id: 1, name: "First", url: "https://via.placeholder.com/150", width: 150, height: 150},
  -     {id: 2, name: "Second", url: "https://via.placeholder.com/300", width: 300, height: 300},
  -   ];
  +   photos: Photo[] = [];
      isPhotosShowVisible: boolean = false;
      currentPhoto: Photo = {} as Photo;

  +   constructor(private photosService: PhotosService) {}

  +   ngOnInit() {
  +     this.handleIndex();
  +   }

  +   handleIndex() {
  +     console.log("handleIndex");
  +     this.photosService.getPhotos().subscribe((response) => {
  +       console.log(response);
  +       this.photos = response;
  +     });
  +   }

      handleShow(photo: Photo) {
        console.log("handleShow", photo);
        this.isPhotosShowVisible = true;
        this.currentPhoto = photo;
      }
    }
  ```

  > Check the browser to see the real data from the backend in your Angular app! 
  > 
  > In your terminal, use git to save a version of your code.
  > ```bash
  > git add --all
  > git commit -m "Add dynamic data with service layer"
  > ```

## Add Create, Update, and Delete Operations

Now that Index and Show are working with the backend, let's add the remaining CRUD operations:

- New/Create actions

  - <details><summary>Make a new <strong>src/app/photos-new.component.ts</strong> file that returns an HTML form</strong></summary>
  
      ```typescript
      import { Component } from '@angular/core';

      @Component({
        selector: 'app-photos-new',
        standalone: false,
        template: `
          <div>
            <h1>New Photo</h1>
            <form>
              <div>
                Name: <input name="name" type="text" />
              </div>
              <div>
                Url: <input name="url" type="text" />
              </div>
              <div>
                Width: <input name="width" type="text" />
              </div>
              <div>
                Height: <input name="height" type="text" />
              </div>
              <button type="submit">Create</button>
            </form>
          </div>
        `
      })
      export class PhotosNewComponent { }
      ```
      </details>

  - <details><summary>In <strong>src/app/app.module.ts</strong>, import the <strong>PhotosNewComponent</strong></summary>
      
      ```diff
        import { NgModule } from '@angular/core';
        import { BrowserModule } from '@angular/platform-browser';
        import { HttpClientModule } from '@angular/common/http';
        import { FormsModule } from '@angular/forms';

        import { AppComponent } from './app.component';
        import { HeaderComponent } from './header.component';
        import { PhotosPageComponent } from './photos-page.component';
        import { FooterComponent } from './footer.component';
        import { PhotosIndexComponent } from './photos-index.component';
      + import { PhotosNewComponent } from './photos-new.component';

        @NgModule({
          declarations: [
            AppComponent,
            HeaderComponent,
            PhotosPageComponent,
            FooterComponent,
            PhotosIndexComponent,
      +     PhotosNewComponent
          ],
          imports: [
            BrowserModule,
            HttpClientModule,
            FormsModule
          ],
          providers: [],
          bootstrap: [AppComponent]
        })
        export class AppModule { }
      ```
      </details>

  - <details><summary>In <strong>src/app/photos-page.component.ts</strong>, import and use the <strong>PhotosNewComponent</strong></summary>
      
      ```diff
        import { Component, OnInit } from '@angular/core';
        import { Photo } from './models/photo';
        import { PhotosService } from './services/photos.service';

        @Component({
          selector: 'app-photos-page',
          template: `
            <main>
      +       <app-photos-new></app-photos-new>
              <app-photos-index [photos]="photos"></app-photos-index>
            </main>
          `
        })
        export class PhotosPageComponent implements OnInit {
          photos: Photo[] = [];

          constructor(private photosService: PhotosService) {}

          ngOnInit() {
            this.handleIndex();
          }

          handleIndex() {
            console.log("handleIndex");
            this.photosService.getPhotos().subscribe((response) => {
              console.log(response);
              this.photos = response;
            });
          }
        }
      ```
      </details>
      
    > Check the browser to see the HTML form in the new component.

  - In <strong>src/app/photos-page/photos-page.ts</strong>, add the create functionality:
      
      ```diff
        import { Component, OnInit } from '@angular/core';
        import { Photo } from '../models/photo';
        import { PhotosService } from '../services/photos.service';

        @Component({
          selector: 'app-photos-page',
          standalone: false,
          template: `
            <main>
      +       <app-photos-new (create)="handleCreate($event)"></app-photos-new>
              <app-photos-index [photos]="photos" (show)="handleShow($event)"></app-photos-index>
              <app-modal [show]="isPhotosShowVisible" (close)="isPhotosShowVisible = false">
                <app-photos-show [photo]="currentPhoto"></app-photos-show>
              </app-modal>
            </main>
          `,
          styles: ``
        })
        export class PhotosPage implements OnInit {
          photos: Photo[] = [];
          isPhotosShowVisible: boolean = false;
          currentPhoto: Photo = {} as Photo;

          constructor(private photosService: PhotosService) {}

          ngOnInit() {
            this.handleIndex();
          }

          handleIndex() {
            console.log("handleIndex");
            this.photosService.getPhotos().subscribe((response) => {
              console.log(response);
              this.photos = response;
            });
          }

      +   handleCreate(params: any) {
      +     console.log("handleCreate", params);
      +     this.photosService.createPhoto(params).subscribe((response) => {
      +       this.photos.push(response);
      +     });
      +   }

          handleShow(photo: Photo) {
            console.log("handleShow", photo);
            this.isPhotosShowVisible = true;
            this.currentPhoto = photo;
          }
        }
      ```

  - <details><summary>In <strong>src/app/photos-new.component.ts</strong>, handle the submit event to emit the create event</strong></summary>
  
      ```diff
      - import { Component } from '@angular/core';
      + import { Component, Output, EventEmitter } from '@angular/core';

        @Component({
          selector: 'app-photos-new',
          template: `
            <div>
              <h1>New Photo</h1>
      -       <form>
      +       <form (ngSubmit)="handleSubmit($event)">
                <div>
      -           Name: <input name="name" type="text" />
      +           Name: <input name="name" type="text" [(ngModel)]="newPhotoParams.name" />
                </div>
                <div>
      -           Url: <input name="url" type="text" />
      +           Url: <input name="url" type="text" [(ngModel)]="newPhotoParams.url" />
                </div>
                <div>
      -           Width: <input name="width" type="text" />
      +           Width: <input name="width" type="text" [(ngModel)]="newPhotoParams.width" />
                </div>
                <div>
      -           Height: <input name="height" type="text" />
      +           Height: <input name="height" type="text" [(ngModel)]="newPhotoParams.height" />
                </div>
                <button type="submit">Create</button>
              </form>
            </div>
          `
        })
        export class PhotosNewComponent {
      +   @Output() create = new EventEmitter();
      +   newPhotoParams: any = {};

      +   handleSubmit(event: Event) {
      +     event.preventDefault();
      +     this.create.emit(this.newPhotoParams);
      +     this.newPhotoParams = {};
      +   }
        }
      ```
      </details>
      
    > Check the browser console to see the message when you submit the form.

  - In <strong>src/app/photos-page.component.ts</strong>, add logic to create a new photo with hardcoded data
      
      ```diff
        import { Component } from '@angular/core';
        import { Photo } from './models/photo';

        @Component({
          selector: 'app-photos-page',
          template: `
            <main>
              <app-photos-new (create)="handleCreate($event)"></app-photos-new>
              <app-photos-index [photos]="photos"></app-photos-index>
            </main>
          `
        })
        export class PhotosPageComponent {
          photos: Photo[] = [
            {id: 1, name: "First", url: "https://via.placeholder.com/150", width: 150, height: 150},
            {id: 2, name: "Second", url: "https://via.placeholder.com/300", width: 300, height: 300},
          ];

          handleCreate(params: any) {
            console.log("handleCreate", params);
      +     const newPhoto: Photo = {
      +       id: this.photos.length + 1,
      +       name: params.name,
      +       url: params.url,
      +       width: parseInt(params.width),
      +       height: parseInt(params.height)
      +     };
      +     this.photos.push(newPhoto);
          }
        }
      ```
      
    > Check the browser to fill in the HTML form and create a new item.
    > 
    > In your terminal, use git to save a version of your code.
    > ```bash
    > git add --all
    > git commit -m "Add new/create actions"
    > ```

- Edit/Update actions

  - <details><summary>Modify <strong>src/app/photos-show.component.ts</strong> to include an edit form with default values</summary>
  
    ```diff
    - import { Component, Input } from '@angular/core';
    + import { Component, Input, Output, EventEmitter, OnChanges } from '@angular/core';
    + import { Photo } from './models/photo';

      @Component({
        selector: 'app-photos-show',
        template: `
          <div>
            <h1>Photo information</h1>
            <p>Name: {{photo.name}}</p>
            <p>Url: {{photo.url}}</p>
            <p>Width: {{photo.width}}</p>
            <p>Height: {{photo.height}}</p>
    +       <form (ngSubmit)="handleSubmit($event)">
    +         <div>
    +           Name: <input [(ngModel)]="editPhotoParams.name" name="name" type="text" />
    +         </div>
    +         <div>
    +           Url: <input [(ngModel)]="editPhotoParams.url" name="url" type="text" />
    +         </div>
    +         <div>
    +           Width: <input [(ngModel)]="editPhotoParams.width" name="width" type="text" />
    +         </div>
    +         <div>
    +           Height: <input [(ngModel)]="editPhotoParams.height" name="height" type="text" />
    +         </div>
    +         <button type="submit">Update</button>
    +       </form>
          </div>
        `
      })
    - export class PhotosShowComponent {
    + export class PhotosShowComponent implements OnChanges {
        @Input() photo: Photo = {} as Photo;
    +   @Output() update = new EventEmitter();
    +   editPhotoParams: Photo = {} as Photo;

    +   ngOnChanges() {
    +     this.editPhotoParams = { ...this.photo };
    +   }

    +   handleSubmit(event: Event) {
    +     event.preventDefault();
    +     this.update.emit({ photo: this.photo, params: this.editPhotoParams });
    +   }
      }
    ```
    </details>

    > Check the browser to see the edit HTML form within the modal

  - <details><summary>In <strong>src/app/photos-page.component.ts</strong>, make a placeholder update function and pass it into the show component</summary>
  
    ```diff
      import { Component, OnInit } from '@angular/core';
      import { Photo } from './models/photo';
      import { PhotosService } from './services/photos.service';

      @Component({
        selector: 'app-photos-page',
        template: `
          <main>
            <app-photos-new (create)="handleCreate($event)"></app-photos-new>
            <app-photos-index [photos]="photos" (show)="handleShow($event)"></app-photos-index>
            <app-modal [show]="isPhotosShowVisible" (close)="isPhotosShowVisible = false">
    -         <app-photos-show [photo]="currentPhoto"></app-photos-show>
    +         <app-photos-show [photo]="currentPhoto" (update)="handleUpdate($event)"></app-photos-show>
            </app-modal>
          </main>
        `
      })
      export class PhotosPageComponent implements OnInit {
        photos: Photo[] = [];
        isPhotosShowVisible: boolean = false;
        currentPhoto: Photo = {} as Photo;

        constructor(private photosService: PhotosService) {}

        ngOnInit() {
          this.handleIndex();
        }

        handleIndex() {
          console.log("handleIndex");
          this.photosService.getPhotos().subscribe((response) => {
            console.log(response);
            this.photos = response;
          });
        }

        handleCreate(params: any) {
          console.log("handleCreate");
          this.photosService.createPhoto(params).subscribe((response) => {
            this.photos.push(response);
          });
        }

        handleShow(photo: Photo) {
          console.log("handleShow", photo);
          this.isPhotosShowVisible = true;
          this.currentPhoto = photo;
        }
            
    +   handleUpdate(event: any) {
    +     console.log("handleUpdate");
    +   }
      }
    ```
    </details>

    > Check the browser console to see the message when you submit the form.

  - In <strong>src/app/photos-page/photos-page.ts</strong>, add update and destroy handlers:
  
    ```diff
      import { Component, OnInit } from '@angular/core';
      import { Photo } from '../models/photo';
      import { PhotosService } from '../services/photos.service';

      @Component({
        selector: 'app-photos-page',
        standalone: false,
        template: `
          <main>
            <app-photos-new (create)="handleCreate($event)"></app-photos-new>
            <app-photos-index [photos]="photos" (show)="handleShow($event)"></app-photos-index>
            <app-modal [show]="isPhotosShowVisible" (close)="isPhotosShowVisible = false">
    -         <app-photos-show [photo]="currentPhoto"></app-photos-show>
    +         <app-photos-show [photo]="currentPhoto" (update)="handleUpdate($event)"></app-photos-show>
            </app-modal>
          </main>
        `,
        styles: ``
      })
      export class PhotosPage implements OnInit {
        photos: Photo[] = [];
        isPhotosShowVisible: boolean = false;
        currentPhoto: Photo = {} as Photo;

        constructor(private photosService: PhotosService) {}

        ngOnInit() {
          this.handleIndex();
        }

        handleIndex() {
          console.log("handleIndex");
          this.photosService.getPhotos().subscribe((response) => {
            console.log(response);
            this.photos = response;
          });
        }

        handleCreate(params: any) {
          console.log("handleCreate", params);
          this.photosService.createPhoto(params).subscribe((response) => {
            this.photos.push(response);
          });
        }

        handleShow(photo: Photo) {
          console.log("handleShow", photo);
          this.isPhotosShowVisible = true;
          this.currentPhoto = photo;
        }

    +   handleUpdate(event: any) {
    +     console.log("handleUpdate", event);
    +     const { photo, params } = event;
    +     this.photosService.updatePhoto(photo.id, params).subscribe((response) => {
    +       this.photos = this.photos.map(p => p.id === response.id ? response : p);
    +       this.isPhotosShowVisible = false;
    +     });
    +   }
      }
    ```

    > Check the browser to edit an attribute using the HTML form and submit the form
    > 
    > In your terminal, use git to save a version of your code.
    > ```bash
    > git add --all
    > git commit -m "Add edit/update actions"
    > ```
  
- Destroy action

  - In <strong>src/app/photos-show.component.ts</strong>, add a destroy button:
  
    ```diff
      import { Component, Input, Output, EventEmitter, OnChanges } from '@angular/core';
      import { Photo } from './models/photo';

      @Component({
        selector: 'app-photos-show',
        standalone: false,
        template: `
          <div>
            <h1>Photo information</h1>
            <p>Name: {{photo.name}}</p>
            <p>Url: {{photo.url}}</p>
            <p>Width: {{photo.width}}</p>
            <p>Height: {{photo.height}}</p>
            <form (ngSubmit)="handleSubmit($event)">
              <div>
                Name: <input [(ngModel)]="editPhotoParams.name" name="name" type="text" />
              </div>
              <div>
                Url: <input [(ngModel)]="editPhotoParams.url" name="url" type="text" />
              </div>
              <div>
                Width: <input [(ngModel)]="editPhotoParams.width" name="width" type="text" />
              </div>
              <div>
                Height: <input [(ngModel)]="editPhotoParams.height" name="height" type="text" />
              </div>
              <button type="submit">Update</button>
            </form>
    +       <button (click)="destroy.emit(photo)">Destroy</button>
          </div>
        `
      })
      export class PhotosShowComponent implements OnChanges {
        @Input() photo: Photo = {} as Photo;
        @Output() update = new EventEmitter();
    +   @Output() destroy = new EventEmitter<Photo>();
        editPhotoParams: Photo = {} as Photo;

        ngOnChanges() {
          this.editPhotoParams = { ...this.photo };
        }

        handleSubmit(event: Event) {
          event.preventDefault();
          this.update.emit({ photo: this.photo, params: this.editPhotoParams });
        }
      }
    ```

    > Check the browser console to see the message when you click the destroy button.

  - In <strong>src/app/photos-page/photos-page.ts</strong>, add the destroy handler:
  
    ```diff
      import { Component, OnInit } from '@angular/core';
      import { Photo } from '../models/photo';
      import { PhotosService } from '../services/photos.service';

      @Component({
        selector: 'app-photos-page',
        standalone: false,
        template: `
          <main>
            <app-photos-new (create)="handleCreate($event)"></app-photos-new>
            <app-photos-index [photos]="photos" (show)="handleShow($event)"></app-photos-index>
            <app-modal [show]="isPhotosShowVisible" (close)="isPhotosShowVisible = false">
    -         <app-photos-show [photo]="currentPhoto" (update)="handleUpdate($event)"></app-photos-show>
    +         <app-photos-show [photo]="currentPhoto" (update)="handleUpdate($event)" (destroy)="handleDestroy($event)"></app-photos-show>
            </app-modal>
          </main>
        `,
        styles: ``
      })
      export class PhotosPage implements OnInit {
        photos: Photo[] = [];
        isPhotosShowVisible: boolean = false;
        currentPhoto: Photo = {} as Photo;

        constructor(private photosService: PhotosService) {}

        ngOnInit() {
          this.handleIndex();
        }

        handleIndex() {
          console.log("handleIndex");
          this.photosService.getPhotos().subscribe((response) => {
            console.log(response);
            this.photos = response;
          });
        }

        handleCreate(params: any) {
          console.log("handleCreate", params);
          this.photosService.createPhoto(params).subscribe((response) => {
            this.photos.push(response);
          });
        }

        handleShow(photo: Photo) {
          console.log("handleShow", photo);
          this.isPhotosShowVisible = true;
          this.currentPhoto = photo;
        }

        handleUpdate(event: any) {
          console.log("handleUpdate", event);
          const { photo, params } = event;
          this.photosService.updatePhoto(photo.id, params).subscribe((response) => {
            this.photos = this.photos.map(p => p.id === response.id ? response : p);
            this.isPhotosShowVisible = false;
          });
        }

    +   handleDestroy(photo: Photo) {
    +     console.log("handleDestroy", photo);
    +     this.photosService.deletePhoto(photo.id).subscribe(() => {
    +       this.photos = this.photos.filter((p) => p.id !== photo.id);
    +       this.isPhotosShowVisible = false;
    +     });
    +   }
      }
    ```

    > Check the browser to edit and delete photos!
    > 
    > In your terminal, use git to save a version of your code.
    > ```bash
    > git add --all
    > git commit -m "Add destroy action"
    > ```

    👉 **Congratulations!** At this point you've implemented all 5 CRUD verbs (Create, Read, Update, Delete) with a backend API. Your Angular app should feel like a mini-Unsplash with a fully functional photo gallery!

## Optional improvements

Consider these enhancements for production apps:

- **Separate template files**: Replace inline `template:` with `templateUrl: './component.html'` for better editor support and larger templates
- **Component CSS**: Use `styleUrls: ['./component.css']` instead of inline styles for better organization  
- **SCSS/Less support**: Angular CLI supports `ng new --style=scss` for advanced styling features
- **Error handling**: Add `.pipe(catchError())` to HTTP calls for user-friendly error messages
- **Unsubscribe management**: Use `takeUntil()` operator to prevent memory leaks from active subscriptions
- **Form validation**: Add Angular reactive forms with validators for better user experience
- **Loading states**: Show spinners or skeleton screens during HTTP requests

## Routes

- In your terminal, install the Angular router package:
  ```bash
  npm install @angular/router
  ```

- Create a new file **src/app/app-routing.module.ts** to define the routes:
  ```typescript
  import { NgModule } from '@angular/core';
  import { RouterModule, Routes } from '@angular/router';

  import { PhotosPage } from './photos-page/photos-page';
  import { PhotosNewComponent } from './photos-new.component';
  
  const routes: Routes = [
    { path: 'photos', component: PhotosPage },
    { path: 'photos/new', component: PhotosNewComponent },
    { path: '', redirectTo: '/photos', pathMatch: 'full' },
  ];
  
  @NgModule({
    imports: [RouterModule.forRoot(routes)],
    exports: [RouterModule]
  })
  export class AppRoutingModule { }
  ```

- Update **src/app/app.module.ts** to import and use Angular router:
  ```diff
    import { NgModule } from '@angular/core';
    import { BrowserModule } from '@angular/platform-browser';
    import { HttpClientModule } from '@angular/common/http';
    import { FormsModule } from '@angular/forms';
  + import { AppRoutingModule } from './app-routing.module';

    import { AppComponent } from './app.component';
    import { Header } from './header/header';
    import { PhotosPage } from './photos-page/photos-page';
    import { Footer } from './footer/footer';
    import { PhotosIndexComponent } from './photos-index.component';
    import { PhotosNewComponent } from './photos-new.component';
    import { PhotosShowComponent } from './photos-show.component';
    import { ModalComponent } from './modal.component';

    @NgModule({
      declarations: [
        AppComponent,
        Header,
        PhotosPage,
        Footer,
        PhotosIndexComponent,
        PhotosNewComponent,
        PhotosShowComponent,
        ModalComponent
      ],
      imports: [
        BrowserModule,
  +     AppRoutingModule,
        HttpClientModule,
        FormsModule
      ],
      providers: [],
      bootstrap: [AppComponent]
    })
    export class AppModule { }
  ```
  
- Update **src/app/app.component.ts** to use the `<router-outlet>` component:
  ```diff
    import { Component } from '@angular/core';

    @Component({
      selector: 'app-root',
      standalone: false,
      template: `
        <div>
          <app-header></app-header>
  -       <app-photos-page></app-photos-page>
  +       <router-outlet></router-outlet>
          <app-footer></app-footer>
        </div>
      `
    })
    export class AppComponent { }
  ```
  
- Modify **src/app/header.component.ts** to replace `<a>` elements with `routerLink` directives (example below):
  ```typescript
  import { Component } from '@angular/core';

  @Component({
    selector: 'app-header',
    template: `
      <header>
        <nav>
          <a routerLink="/photos">All photos</a>
          |
          <a routerLink="/photos/new">New Photo</a>
        </nav>
      </header>
    `
  })
  export class HeaderComponent { }
  ``` 