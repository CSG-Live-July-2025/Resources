# Guide: Vue.js CRUD practice

## Backend setup

- In your terminal, navigate to your Rails backend app and run your server: `rails server`
- Make sure each RESTful route (index, create, show, update, destroy) is working by manually testing each request.
- Make sure to configure the [rack-cors](https://github.com/cyu/rack-cors) gem to allow web requests from `localhost:5173` (instructions [here](https://gist.github.com/peterxjang/77d6243cf85103b027a56b401b62b289)).

## Frontend setup

- In a new terminal window, navigate to your Actualize directory and create a new project:
  ```
  npm create vite@latest
  ```

  _When prompted, enter a project name, select the Vue framework, select the JavaScript variant_

- In the terminal, go to your new directory, install the dependencies, and start the dev server:
  ```
  cd <name-of-your-app>
  npm install
  npm run dev
  ```

- In a new terminal tab, install the axios library:
  ```
  npm install axios --save
  ```

- In the terminal, open your text editor for the frontend app:
  ```
  code .
  ```

- In your text editor, make a new file src/components/Header.vue:

  ```vue
  <script></script>

  <template>
    <header>
      <nav>
        <a href="#">Home</a> | <a href="#">Link</a>
      </nav>
    </header>
  </template>

  <style></style>
  ```

- In your text editor, make a new file src/components/Content.vue:

  ```vue
  <script></script>

  <template>
    <main>
      <h1>Welcome to Vue!</h1>
    </main>
  </template>

  <style></style>
  ```

- In your text editor, make a new file src/components/Footer.vue:

  ```vue
  <script></script>

  <template>
    <footer>
      <p>Copyright 2022</p>
    </footer>
  </template>

  <style></style>
  ```

- In your text editor, open the file src/App.vue and replace the code with the following:

  ```vue
  <script>
  import axios from "axios";
  import Header from "./components/Header.vue";
  import Content from "./components/Content.vue";
  import Footer from "./components/Footer.vue";

  axios.defaults.baseURL = "http://localhost:3000";
  axios.defaults.withCredentials = true;

  export default {
    name: "app",
    components: {
      Header,
      Content,
      Footer,
    },
  };
  </script>

  <template>
    <div>
      <Header />
      <Content />
      <Footer />
    </div>
  </template>

  <style></style>
  ```

- In your text editor, open the file src/style.css and replace all the CSS code with the following:

  ```css
  html {
    scroll-behavior: smooth;
  }
  ```
  
- In your browser, visit http://localhost:5173 to make sure your app is working!

## CRUD Single Page Application

- Index action

  - <details><summary>Make a new <strong>src/components/<ins>Photos</ins>Index.vue</strong> file that returns placeholder HTML</strong></summary>
  
      ```vue
      <script></script>

      <template>
        <div>
          <h1>All photos</h1>
        </div>
      </template>

      <style></style>
      ```
      </details>

  - <details><summary>In <strong>src/components/Content.vue</strong>, import the <strong>src/components/<ins>Photos</ins>Index.vue</strong> component</summary>
      
      ```diff
        <script>
      + import PhotosIndex from "./PhotosIndex.vue";

      + export default {
      +   components: {
      +     PhotosIndex,
      +   },
      + };
        </script>

        <template>
          <main>
      -     <h1>Welcome to Vue!</h1>
      +     <PhotosIndex />
          </main>
        </template>

        <style></style>
      ```
      </details>
      
    > Check the browser to see the placeholder HTML from the index component.
    
  - <details><summary>In <strong>src/components/Content.vue</strong>, pass placeholder data to the index component as props</summary>
      
      ```diff
        <script>
        import PhotosIndex from "./PhotosIndex.vue";

        export default {
          components: {
            PhotosIndex,
          },
      +   data: function () {
      +     return {
      +       photos: [
      +         { id: 1, name: "First", url: "https://via.placeholder.com/150", width: 150, height: 150 },
      +         { id: 2, name: "Second", url: "https://via.placeholder.com/300", width: 300, height: 300 },
      +       ],
      +     };
      +   },
        };
        </script>

        <template>
          <main>
      -     <PhotosIndex />
      +     <PhotosIndex v-bind:photos="photos" />
          </main>
        </template>

        <style></style>

      ```
      </details>
      
  - <details><summary>In <strong>src/components/<ins>Photos</ins>Index.vue</strong>, loop through the array given in props</summary>
      
      ```diff
        <script>
      + export default {
      +   props: {
      +     photos: Array,
      +   },
      + };
        </script>

        <template>
          <div>
            <h1>All photos</h1>
      +     <div v-for="photo in photos" v-bind:key="photo.id">
      +       <h2>{{ photo.name }}</h2>
      +       <img v-bind:src="photo.url" />
      +       <p>Width: {{ photo.width }}</p>
      +       <p>Height: {{ photo.height }}</p>
      +     </div>
          </div>
        </template>

        <style></style>
      ```
      </details>
      
    > Check the browser to see the placeholder data in the index component.
    
  - <details><summary>In <strong>src/components/Content.vue</strong>, define the data via a backend web request</summary>
      
      ```diff
        <script>
      + import axios from "axios";
        import PhotosIndex from "./PhotosIndex.vue";

        export default {
          components: {
            PhotosIndex,
          },
          data: function () {
            return {
              photos: [],
            };
          },
      +   created: function () {
      +     this.handleIndexPhotos();
      +   },
      +   methods: {
      +     handleIndexPhotos: function () {
      +       axios.get("/photos.json").then((response) => {
      +         console.log("photos index", response);
      +         this.photos = response.data;
      +       });
      +     },
      +   },
        };
        </script>

        <template>
          <main>
            <PhotosIndex v-bind:photos="photos" />
          </main>
        </template>

        <style></style>
      ```
      </details>
      
    > Check the browser to see the real data from the backend in the index component.
    
- New/Create actions

  - <details><summary>Make a new <strong>src/components/<ins>Photos</ins>New.vue</strong> file that returns an HTML form</strong></summary>
  
      ```vue
      <script></script>

      <template>
        <div>
          <h1>New Photo</h1>
          <form>
            <div>
              Name:
              <input name="name" type="text" />
            </div>
            <div>
              Url:
              <input name="url" type="text" />
            </div>
            <div>
              Width:
              <input name="width" type="text" />
            </div>
            <div>
              Height:
              <input name="height" type="text" />
            </div>
            <button type="submit">Create photo</button>
          </form>
        </div>
      </template>

      <style></style>
      ```
      </details>

  - <details><summary>In <strong>src/components/Content.vue</strong>, import the <strong>src/components/<ins>Photos</ins>New.vue</strong> component</summary>
      
      ```diff
        <script>
        import axios from "axios";
        import PhotosIndex from "./PhotosIndex.vue";
      + import PhotosNew from "./PhotosNew.vue";

        export default {
          components: {
            PhotosIndex,
      +     PhotosNew,
          },
          data: function () {
            return {
              photos: [],
            };
          },
          created: function () {
            this.handleIndexPhotos();
          },
          methods: {
            handleIndexPhotos: function () {
              axios.get("/photos.json").then((response) => {
                console.log("photos index", response);
                this.photos = response.data;
              });
            },
          },
        };
        </script>

        <template>
          <main>
      +     <PhotosNew />
            <PhotosIndex v-bind:photos="photos" />
          </main>
        </template>

        <style></style>
      ```
      </details>
      
    > Check the browser to see the HTML form in the new component.

  - <details><summary>In <strong>src/components/Content.vue</strong>, add an event listener on the new component to run a create function</summary>
      
      ```diff
        <script>
        import axios from "axios";
        import PhotosIndex from "./PhotosIndex.vue";
        import PhotosNew from "./PhotosNew.vue";

        export default {
          components: {
            PhotosIndex,
            PhotosNew,
          },
          data: function () {
            return {
              photos: [],
            };
          },
          created: function () {
            this.handleIndexPhotos();
          },
          methods: {
            handleIndexPhotos: function () {
              axios.get("/photos.json").then((response) => {
                console.log("photos index", response);
                this.photos = response.data;
              });
            },
      +     handleCreatePhoto: function (params) {
      +       axios
      +         .post("/photos.json", params)
      +         .then((response) => {
      +           console.log("photos create", response);
      +           this.photos.push(response.data);
      +         })
      +         .catch((error) => {
      +           console.log("photos create error", error.response);
      +         });
      +     },
          },
        };
        </script>

        <template>
          <main>
      -     <PhotosNew />
      +     <PhotosNew v-on:createPhoto="handleCreatePhoto" />
            <PhotosIndex v-bind:photos="photos" />
          </main>
        </template>

        <style></style>
      ```
      </details>

  - <details><summary>In <strong>src/components/<ins>Photos</ins>New.vue</strong>, handle the submit event to emit the create event</strong></summary>
  
      ```diff
        <script>
      + export default {
      +   data: function () {
      +     return {
      +       newPhotoParams: {},
      +     };
      +   },
      +   methods: {
      +     handleSubmit: function () {
      +       this.$emit("createPhoto", this.newPhotoParams);
      +       this.newPhotoParams = {};
      +     },
      +   },
      + };
        </script>

        <template>
          <div>
            <h1>New Photo</h1>
      -     <form>
      +     <form v-on:submit.prevent="handleSubmit">
              <div>
                Name:
      -         <input type="text" />
      +         <input type="text" v-model="newPhotoParams.name" />
              </div>
              <div>
                Url:
      -         <input type="text" />
      +         <input type="text" v-model="newPhotoParams.url" />
              </div>
              <div>
                Width:
      -         <input type="text" />
      +         <input type="text" v-model="newPhotoParams.width" />
              </div>
              <div>
                Height:
      -         <input type="text" />
      +         <input type="text" v-model="newPhotoParams.height" />
              </div>
              <button type="submit">Create photo</button>
            </form>
          </div>
        </template>

        <style></style>
      ```
      </details>
      
    > Check the browser to fill in the HTML form and create a new item.
    
- Show action

  - <details><summary>Make a new <strong>src/components/Modal.vue</strong> file with the following content:</summary>
  
    ```vue
    <script>
    export default {
      props: {
        show: Boolean,
      },
    };
    </script>

    <template>
      <div v-if="show" class="modal-background">
        <section class="modal-main">
          <slot></slot>
          <button class="close" type="button" v-on:click="$emit('close')">&#x2715;</button>
        </section>
      </div>
    </template>

    <style>
    .modal-background {
      display: block;
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
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
      transform: translate(-50%, -50%);
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
    </style>
    ```
    </details>

  - <details><summary>In <strong>src/components/Content.vue</strong>, import the modal component</summary>
  
    ```diff
      <script>
      import axios from "axios";
      import PhotosIndex from "./PhotosIndex.vue";
      import PhotosNew from "./PhotosNew.vue";
    + import Modal from "./Modal.vue";

      export default {
        components: {
          PhotosIndex,
          PhotosNew,
    +     Modal,
        },
        data: function () {
          return {
            photos: [],
          };
        },
        created: function () {
          this.handleIndexPhotos();
        },
        methods: {
          handleIndexPhotos: function () {
            axios.get("/photos.json").then((response) => {
              console.log("photos index", response);
              this.photos = response.data;
            });
          },
          handleCreatePhoto: function (params) {
            axios
              .post("/photos.json", params)
              .then((response) => {
                console.log("photos create", response);
                this.photos.push(response.data);
              })
              .catch((error) => {
                console.log("photos create error", error.response);
              });
          },
        },
      };
      </script>

      <template>
        <main>
          <PhotosNew v-on:createPhoto="handleCreatePhoto" />
          <PhotosIndex v-bind:photos="photos" />
    +     <Modal v-bind:show="true">
    +       <h1>Test</h1>
    +     </Modal>
        </main>
      </template>

      <style></style>
    ```
    </details>
    
    > Check the browser to see the modal with placeholder HTML

  - <details><summary>In <strong>src/comonents/Content.vue</strong>, make state and functions to show and hide the modal</summary>
  
    ```diff
      <script>
      import axios from "axios";
      import PhotosIndex from "./PhotosIndex.vue";
      import PhotosNew from "./PhotosNew.vue";
      import Modal from "./Modal.vue";

      export default {
        components: {
          PhotosIndex,
          PhotosNew,
          Modal,
        },
        data: function () {
          return {
            photos: [],
    +       currentPhoto: {},
    +       isPhotosShowVisible: false,
          };
        },
        created: function () {
          this.handleIndexPhotos();
        },
        methods: {
          handleIndexPhotos: function () {
            axios.get("/photos.json").then((response) => {
              console.log("photos index", response);
              this.photos = response.data;
            });
          },
          handleCreatePhoto: function (params) {
            axios
              .post("/photos.json", params)
              .then((response) => {
                console.log("photos create", response);
                this.photos.push(response.data);
              })
              .catch((error) => {
                console.log("photos create error", error.response);
              });
          },
    +     handleShowPhoto: function (photo) {
    +       console.log("handleShowPhoto", photo);
    +       this.currentPhoto = photo;
    +       this.isPhotosShowVisible = true;
    +     },
    +     handleClose: function () {
    +       this.isPhotosShowVisible = false;
    +     },
        },
      };
      </script>

      <template>
        <main>
          <PhotosNew v-on:createPhoto="handleCreatePhoto" />
    -     <PhotosIndex v-bind:photos="photos" />
    +     <PhotosIndex v-bind:photos="photos" v-on:showPhoto="handleShowPhoto" />
    -     <Modal v-bind:show="true">
    +     <Modal v-bind:show="isPhotosShowVisible" v-on:close="handleClose">
            <h1>Test</h1>
          </Modal>
        </main>
      </template>

      <style></style>
    ```
    </details>
  
  - <details><summary>In <strong>src/components/<ins>Photos</ins>Index.vue</strong>, make a button to show the modal</summary>
  
    ```diff
      <script>
      export default {
        props: {
          photos: Array,
        },
      };
      </script>

      <template>
        <div>
          <h1>All photos</h1>
          <div v-for="photo in photos" v-bind:key="photo.id">
            <h2>{{ photo.name }}</h2>
            <img v-bind:src="photo.url" />
            <p>Width: {{ photo.width }}</p>
            <p>Height: {{ photo.height }}</p>
    +       <button v-on:click="$emit('showPhoto', photo)">More info</button>
          </div>
        </div>
      </template>

      <style></style>
    ```
    </details>
            
    > Check the browser to test showing and closing the modal with the appropriate buttons
            
  - <details><summary>Make a new <strong>src/components/<ins>Photos</ins>Show.vue</strong> file that renders attributes from props</summary>
  
    ```vue
    <script>
    export default {
      props: {
        photo: Object,
      },
    };
    </script>

    <template>
      <div>
        <h1>Photo information</h1>
        <p>Name: {{ photo.name }}</p>
        <p>Url: {{ photo.url }}</p>
        <p>Width: {{ photo.width }}</p>
        <p>Height: {{ photo.height }}</p>
      </div>
    </template>

    <style></style>
    ```
    </details>

  - <details><summary>In <strong>src/components/Content.vue</strong>, import the show component and use it in the modal</summary>
  
    ```diff
      <script>
      import axios from "axios";
      import PhotosIndex from "./PhotosIndex.vue";
      import PhotosNew from "./PhotosNew.vue";
    + import PhotosShow from "./PhotosShow.vue";
      import Modal from "./Modal.vue";

      export default {
        components: {
          PhotosIndex,
          PhotosNew,
    +     PhotosShow,
          Modal,
        },
        data: function () {
          return {
            photos: [],
            currentPhoto: {},
            isPhotosShowVisible: false,
          };
        },
        created: function () {
          this.handleIndexPhotos();
        },
        methods: {
          handleIndexPhotos: function () {
            axios.get("/photos.json").then((response) => {
              console.log("photos index", response);
              this.photos = response.data;
            });
          },
          handleCreatePhoto: function (params) {
            axios
              .post("/photos.json", params)
              .then((response) => {
                console.log("photos create", response);
                this.photos.push(response.data);
              })
              .catch((error) => {
                console.log("photos create error", error.response);
              });
          },
          handleShowPhoto: function (photo) {
            console.log("handleShowPhoto", photo);
            this.currentPhoto = photo;
            this.isPhotosShowVisible = true;
          },
          handleClose: function () {
            this.isPhotosShowVisible = false;
          },
        },
      };
      </script>

      <template>
        <main>
          <PhotosNew v-on:createPhoto="handleCreatePhoto" />
          <PhotosIndex v-bind:photos="photos" v-on:showPhoto="handleShowPhoto" />
          <Modal v-bind:show="isPhotosShowVisible" v-on:close="handleClose">
    -       <h1>Test</h1>
    +       <PhotosShow v-bind:photo="currentPhoto" />
          </Modal>
        </main>
      </template>

      <style></style>
    ```
    </details>
    
    > Check the browser to see the selected item attributes within the modal

- Edit/Update actions

  - <details><summary>Modify <strong>src/components/<ins>Photos</ins>Show.vue</strong> to include an edit form with default values</summary>
  
    ```diff
      <script>
      export default {
        props: {
          photo: Object,
        },
    +   data: function () {
    +     return {
    +       editPhotoParams: {},
    +     };
    +   },
    +   created: function () {
    +     this.editPhotoParams = { ...this.photo };
    +   },
      };
      </script>

      <template>
        <div>
          <h1>Photo information</h1>
          <p>Name: {{ photo.name }}</p>
          <p>Url: {{ photo.url }}</p>
          <p>Width: {{ photo.width }}</p>
          <p>Height: {{ photo.height }}</p>
    +     <form>
    +       <div>
    +         Name:
    +         <input v-model="editPhotoParams.name" type="text" />
    +       </div>
    +       <div>
    +         Url:
    +         <input v-model="editPhotoParams.url" type="text" />
    +       </div>
    +       <div>
    +         Width:
    +         <input v-model="editPhotoParams.width" type="text" />
    +       </div>
    +       <div>
    +         Height:
    +         <input v-model="editPhotoParams.height" type="text" />
    +       </div>
    +       <button type="submit">Update photo</button>
    +     </form>
        </div>
      </template>

      <style></style>
    ```
    </details>

    > Check the browser to see the edit HTML form within the modal

  - <details><summary>In <strong>src/components/Content.vue</strong>, make an update function and pass it into the show component</summary>
  
    ```diff
      <script>
      import axios from "axios";
      import PhotosIndex from "./PhotosIndex.vue";
      import PhotosNew from "./PhotosNew.vue";
      import PhotosShow from "./PhotosShow.vue";
      import Modal from "./Modal.vue";

      export default {
        components: {
          PhotosIndex,
          PhotosNew,
          PhotosShow,
          Modal,
        },
        data: function () {
          return {
            photos: [],
            currentPhoto: {},
            isPhotosShowVisible: false,
          };
        },
        created: function () {
          this.handleIndexPhotos();
        },
        methods: {
          handleIndexPhotos: function () {
            axios.get("/photos.json").then((response) => {
              console.log("photos index", response);
              this.photos = response.data;
            });
          },
          handleCreatePhoto: function (params) {
            axios
              .post("/photos.json", params)
              .then((response) => {
                console.log("photos create", response);
                this.photos.push(response.data);
              })
              .catch((error) => {
                console.log("photos create error", error.response);
              });
          },
          handleShowPhoto: function (photo) {
            console.log("handleShowPhoto", photo);
            this.currentPhoto = photo;
            this.isPhotosShowVisible = true;
          },
    +     handleUpdatePhoto: function (id, params) {
    +       console.log("handleUpdatePhoto", id, params);
    +       axios
    +         .patch(`/photos/${id}.json`, params)
    +         .then((response) => {
    +           console.log("photos update", response);
    +           this.photos = this.photos.map((photo) => {
    +             if (photo.id === response.data.id) {
    +               return response.data;
    +             } else {
    +               return photo;
    +             }
    +           });
    +           this.handleClose();
    +         })
    +         .catch((error) => {
    +           console.log("photos update error", error.response);
    +         });
    +     },
          handleClose: function () {
            this.isPhotosShowVisible = false;
          },
        },
      };
      </script>

      <template>
        <main>
          <PhotosNew v-on:createPhoto="handleCreatePhoto" />
          <PhotosIndex v-bind:photos="photos" v-on:showPhoto="handleShowPhoto" />
          <Modal v-bind:show="isPhotosShowVisible" v-on:close="handleClose">
    -       <PhotosShow v-bind:photo="currentPhoto" />
    +       <PhotosShow v-bind:photo="currentPhoto" v-on:updatePhoto="handleUpdatePhoto" />
          </Modal>
        </main>
      </template>

      <style></style>
    ```
    </details>

  - <details><summary>In <strong>src/components/<ins>Photos</ins>Show.vue</strong>, handle the form submit to run the props update function</summary>
  
    ```diff
      <script>
      export default {
        props: {
          photo: Object,
        },
        data: function () {
          return {
            editPhotoParams: {},
          };
        },
        created: function () {
          this.editPhotoParams = { ...this.photo };
        },
    +   methods: {
    +     handleSubmit: function () {
    +       this.$emit("updatePhoto", this.photo.id, this.editPhotoParams);
    +     },
    +   },
      };
      </script>

      <template>
        <div>
          <h1>Photo information</h1>
          <p>Name: {{ photo.name }}</p>
          <p>Url: {{ photo.url }}</p>
          <p>Width: {{ photo.width }}</p>
          <p>Height: {{ photo.height }}</p>
    -     <form>
    +     <form v-on:submit.prevent="handleSubmit">
            <div>
              Name:
              <input v-model="editPhotoParams.name" type="text" />
            </div>
            <div>
              Url:
              <input v-model="editPhotoParams.url" type="text" />
            </div>
            <div>
              Width:
              <input v-model="editPhotoParams.width" type="text" />
            </div>
            <div>
              Height:
              <input v-model="editPhotoParams.height" type="text" />
            </div>
            <button type="submit">Update photo</button>
          </form>
        </div>
      </template>

      <style></style>
    ```
    </details>

    > Check the browser to edit an attribute using the HTML form and submit the form
            
- Destroy action

  - <details><summary>In <strong>src/components/Content.vue</strong>, make a destroy function and pass it into the show component</summary>
  
    ```diff
      <script>
      import axios from "axios";
      import PhotosIndex from "./PhotosIndex.vue";
      import PhotosNew from "./PhotosNew.vue";
      import PhotosShow from "./PhotosShow.vue";
      import Modal from "./Modal.vue";

      export default {
        components: {
          PhotosIndex,
          PhotosNew,
          PhotosShow,
          Modal,
        },
        data: function () {
          return {
            photos: [],
            currentPhoto: {},
            isPhotosShowVisible: false,
          };
        },
        created: function () {
          this.handleIndexPhotos();
        },
        methods: {
          handleIndexPhotos: function () {
            axios.get("/photos.json").then((response) => {
              console.log("photos index", response);
              this.photos = response.data;
            });
          },
          handleCreatePhoto: function (params) {
            axios
              .post("/photos.json", params)
              .then((response) => {
                console.log("photos create", response);
                this.photos.push(response.data);
              })
              .catch((error) => {
                console.log("photos create error", error.response);
              });
          },
          handleShowPhoto: function (photo) {
            console.log("handleShowPhoto", photo);
            this.currentPhoto = photo;
            this.isPhotosShowVisible = true;
          },
          handleUpdatePhoto: function (id, params) {
            console.log("handleUpdatePhoto", id, params);
            axios
              .patch(`/photos/${id}.json`, params)
              .then((response) => {
                console.log("photos update", response);
                this.photos = this.photos.map((photo) => {
                  if (photo.id === response.data.id) {
                    return response.data;
                  } else {
                    return photo;
                  }
                });
                this.handleClose();
              })
              .catch((error) => {
                console.log("photos update error", error.response);
              });
          },
    +     handleDestroyPhoto: function (photo) {
    +       axios.delete(`/photos/${photo.id}.json`).then((response) => {
    +         console.log("photos destroy", response);
    +         var index = this.photos.indexOf(photo);
    +         this.photos.splice(index, 1);
    +         this.handleClose();
    +       });
    +     },
          handleClose: function () {
            this.isPhotosShowVisible = false;
          },
        },
      };
      </script>

      <template>
        <main>
          <PhotosNew v-on:createPhoto="handleCreatePhoto" />
          <PhotosIndex v-bind:photos="photos" v-on:showPhoto="handleShowPhoto" />
          <Modal v-bind:show="isPhotosShowVisible" v-on:close="handleClose">
    -       <PhotosShow v-bind:photo="currentPhoto" v-on:updatePhoto="handleUpdatePhoto" />
    +       <PhotosShow v-bind:photo="currentPhoto" v-on:updatePhoto="handleUpdatePhoto" v-on:destroyPhoto="handleDestroyPhoto" />
          </Modal>
        </main>
      </template>

      <style></style>
    ```
    </details>

  - <details><summary>In <strong>src/components/<ins>Photos</ins>Show.vue</strong>, make a button to run the props destroy function on click</summary>
  
    ```diff
      <script>
      export default {
        props: {
          photo: Object,
        },
        data: function () {
          return {
            editPhotoParams: {},
          };
        },
        created: function () {
          this.editPhotoParams = { ...this.photo };
        },
        methods: {
          handleSubmit: function () {
            this.$emit("updatePhoto", this.photo.id, this.editPhotoParams);
          },
        },
      };
      </script>

      <template>
        <div>
          <h1>Photo information</h1>
          <p>Name: {{ photo.name }}</p>
          <p>Url: {{ photo.url }}</p>
          <p>Width: {{ photo.width }}</p>
          <p>Height: {{ photo.height }}</p>
          <form v-on:submit.prevent="handleSubmit">
            <div>
              Name:
              <input v-model="editPhotoParams.name" type="text" />
            </div>
            <div>
              Url:
              <input v-model="editPhotoParams.url" type="text" />
            </div>
            <div>
              Width:
              <input v-model="editPhotoParams.width" type="text" />
            </div>
            <div>
              Height:
              <input v-model="editPhotoParams.height" type="text" />
            </div>
            <button type="submit">Update photo</button>
          </form>
    +     <button v-on:click="$emit('destroyPhoto', photo)">Destroy photo</button>
        </div>
      </template>

      <style></style>
    ```
    </details>

    > Check the browser to destroy a photo by clicking the destroy button

## Routes

- In your terminal, install vue router
  ```bash
  npm install vue-router@4
  ```

- Make a new file `src/router.js` to define the routes (example below)
  ```javascript
  import { createWebHistory, createRouter } from "vue-router";

  import Signup from "./components/Signup.vue";
  import Login from "./components/Login.vue";
  import LogoutLink from "./components/LogoutLink.vue";
  import PhotosIndex from "./components/PhotosIndex.vue";
  import PhotosNew from "./components/PhotosNew.vue";
  
  const routes = [
    { path: "/signup", component: Signup },
    { path: "/login", component: Login },
    { path: "/logout", component: LogoutLink },
    { path: "/photos", component: PhotosIndex },
    { path: "/photos/new", component: PhotosNew },
  ];
  
  const router = createRouter({
    history: createWebHistory(),
    routes,
  });
  
  export default router;
  ```

- Modify `src/main.js` to import and use Vue router
  ```diff
    import { createApp } from "vue";
    import "./style.css";
    import App from "./App.vue";
  + import router from "./router";
    
  - createApp(App).mount("#app");
  + createApp(App).use(router).mount("#app");
  ```
  
- Modify `src/App.vue` to use the `<RouterView>` component
  ```diff
    <script>
    import Header from "./components/Header.vue";
    import Content from "./components/Content.vue";
    import Footer from "./components/Footer.vue";
    
    export default {
      name: "app",
      components: {
        Header,
        Content,
        Footer,
      },
    };
    </script>
    
    <template>
      <div>
        <Header />
  -     <Content />
  +     <RouterView />
        <Footer />
      </div>
    </template>
    
    <style></style>
  ```
  
- Modify `src/components/Header.vue` to replace `<a>` elements with `<RouterLink>` elements (example below):
  ```html
  <script></script>
  
  <template>
    <header>
      <nav>
        <RouterLink to="/photos">All photos</RouterLink>
        |
        <RouterLink to="/photos/new">New Photo</RouterLink>
        |
        <RouterLink to="/signup">Signup</RouterLink>
        |
        <RouterLink to="/login">Login</RouterLink>
        |
        <RouterLink to="/logout">Logout</RouterLink>
      </nav>
    </header>
  </template>
  
  <style></style>
  ```
