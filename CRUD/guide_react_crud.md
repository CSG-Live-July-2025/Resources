# Guide: React CRUD practice

## Backend setup

- In your terminal, navigate to your Rails backend app and run your server: `rails server`
- Make sure each RESTful route (index, create, show, update, destroy) is working by manually testing each request.
- Make sure to configure the [rack-cors](https://github.com/cyu/rack-cors) gem to allow web requests from `localhost:5173`.

## Frontend setup

- In a new terminal window, navigate to your Actualize directory and create a new project:
  ```
  npm create vite@latest
  ```

  _When prompted, enter a project name, select the React framework, select the JavaScript variant_

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
  
- In your text editor, make a new file <strong>src/Header.jsx</strong>:

  ```jsx
  export function Header() {
    return (
      <header>
        <nav>
          <a href="#">Home</a> | <a href="#">Link</a>
        </nav>
      </header>
    )
  }
  ```

- In your text editor, make a new file <strong>src/<ins>Photos</ins>Page.jsx</strong>:

  ```jsx
  export function PhotosPage() {
    return (
      <main>
        <h1>Welcome to React!</h1>
      </main>
    )
  }
  ```

- In your text editor, make a new file <strong>src/Footer.jsx</strong>:

  ```jsx
  export function Footer() {
    return (
      <footer>
        <p>Copyright 2025</p>
      </footer>
    )
  }
  ```

- In your text editor, open the file src/App.jsx and replace the code with the following:

  ```jsx
  import axios from "axios";
  import { Header } from "./Header";
  import { PhotosPage } from "./PhotosPage";
  import { Footer } from "./Footer";

  // Sets the default URL so we don't have to type http://localhost:3000 every time
  // axios.defaults.baseURL = "http://localhost:3000";

  function App() {
    return (
      <div style={{ textAlign: 'center' }}>
        <Header />
        <PhotosPage />
        <Footer />
      </div>
    )
  }

  export default App;
  ```

- In your text editor, open the file src/index.css and replace all the CSS code with the following:

  ```css
  html {
    scroll-behavior: smooth;
  }
  ```
  
- In your browser, visit http://localhost:5173 to make sure your app is working!

  > In your terminal, use git to save a version of your code.
  > ```bash
  > git init
  > git add --all
  > git commit -m "Initial commit"
  > ```

## CRUD Single Page Application

- Index action

  - <details><summary>Make a new <strong>src/<ins>Photos</ins>Index.jsx</strong> file that returns placeholder HTML</strong></summary>
  
      ```jsx
      export function PhotosIndex() {
        return (
          <div>
            <h1>All photos</h1>
          </div>
        );
      }
      ```
      </details>

  - <details><summary>In <strong>src/<ins>Photos</ins>Page.jsx</strong>, import the <strong>src/<ins>Photos</ins>Index.jsx</strong> component</summary>
      
      ```diff
      + import { PhotosIndex } from "./PhotosIndex";
  
        export function PhotosPage() {
          return (
            <main>
      -       <h1>Welcome to React!</h1>
      +       <PhotosIndex />
            </main>
          );
        }
      ```
      </details>
      
    > Check the browser to see the placeholder HTML from the index component.
    
  - <details><summary>In <strong>src/<ins>Photos</ins>Page.jsx</strong>, pass placeholder data to the index component as props</summary>
      
      ```diff
        import { PhotosIndex } from "./PhotosIndex";
  
        export function PhotosPage() {
      +   const photos = [
      +     {id: 1, name: "First", url: "https://via.placeholder.com/150", width: 150, height: 150},
      +     {id: 2, name: "Second", url: "https://via.placeholder.com/300", width: 300, height: 300},
      +   ];
  
          return (
            <main>
      -       <PhotosIndex />
      +       <PhotosIndex photos={photos} />
            </main>
          );
        }
      ```
      </details>
      
  - <details><summary>In <strong>src/<ins>Photos</ins>Index.jsx</strong>, use the props to display some information</summary>
      
      ```diff
      - export function PhotosIndex() {
      + export function PhotosIndex({ photos }) {
          return (
            <div>
      -       <h1>All photos</h1>
      +       <h1>All photos ({photos.length} total)</h1>
            </div>
          );
        }
      ```
      </details>
      
    > Check the browser to see the data in the index component.
      
  - In <strong>src/<ins>Photos</ins>Index.jsx</strong>, map through the array given in props
      
      ```diff
        export function PhotosIndex({ photos }) {
          return (
            <div>
              <h1>All photos ({photos.length} total)</h1>
      +       {photos.map((photo) => (
      +         <div key={photo.id}>
      +           <h2>{photo.name}</h2>
      +           <img src={photo.url} />
      +           <p>Width: {photo.width}</p>
      +           <p>Height: {photo.height}</p>
      +         </div>
      +       ))}
            </div>
          );
        }
      ```
      
    > Check the browser to see the placeholder data in the index component.
    
  - In <strong>src/<ins>Photos</ins>Page.jsx</strong>, use the useState and useEffect hooks to define the data via a backend web request
      
      ```diff
      + import axios from "axios";
      + import { useState, useEffect } from "react";
        import { PhotosIndex } from "./PhotosIndex";
  
        export function PhotosPage() {
      -   const photos = [
      -     {id: 1, name: "First", url: "https://via.placeholder.com/150", width: 150, height: 150},
      -     {id: 2, name: "Second", url: "https://via.placeholder.com/300", width: 300, height: 300},
      -   ];
  
      +   const [photos, setPhotos] = useState([]);

      +   const handleIndex = () => {
      +     console.log("handleIndex");
      +     axios.get("/photos.json").then((response) => {
      +       console.log(response.data);
      +       setPhotos(response.data);
      +     });
      +   };

      +   // calls handleIndex whenever the page loads
      +   useEffect(handleIndex, []);
  
          return (
            <main>
              <PhotosIndex photos={photos} />
            </main>
          );
        }
      ```
      
    > Check the browser to see the real data from the backend in the index component.
    > 
    > In your terminal, use git to save a version of your code.
    > ```bash
    > git add --all
    > git commit -m "Add index action"
    > ```
- New/Create actions

  - <details><summary>Make a new <strong>src/<ins>Photos</ins>New.jsx</strong> file that returns an HTML form</strong></summary>
  
      ```jsx
      export function PhotosNew() {
        return (
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
        );
      }
      ```
      </details>

  - <details><summary>In <strong>src/<ins>Photos</ins>Page.jsx</strong>, import the <strong>src/<ins>Photos</ins>New.jsx</strong> component</summary>
      
      ```diff
        import axios from "axios";
        import { useState, useEffect } from "react";
        import { PhotosIndex } from "./PhotosIndex";
      + import { PhotosNew } from "./PhotosNew";

        export function PhotosPage() {
          const [photos, setPhotos] = useState([]);

          const handleIndex = () => {
            console.log("handleIndex");
            axios.get("/photos.json").then((response) => {
              console.log(response.data);
              setPhotos(response.data);
            });
          };

          useEffect(handleIndex, []);

          return (
            <main>
      +       <PhotosNew />
              <PhotosIndex photos={photos} />
            </main>
          );
        }
      ```
      </details>
      
    > Check the browser to see the HTML form in the new component.

  - <details><summary>In <strong>src/<ins>Photos</ins>Page.jsx</strong>, make a placeholder create function and pass it to the new component</summary>
      
      ```diff
  
        import axios from "axios";
        import { useState, useEffect } from "react";
        import { PhotosIndex } from "./PhotosIndex";
        import { PhotosNew } from "./PhotosNew";

        export function PhotosPage() {
          const [photos, setPhotos] = useState([]);

          const handleIndex = () => {
            console.log("handleIndex");
            axios.get("/photos.json").then((response) => {
              console.log(response.data);
              setPhotos(response.data);
            });
          };

      +   const handleCreate = () => {
      +     console.log("handleCreate");
      +   };
  
          useEffect(handleIndex, []);

          return (
            <main>
      -       <PhotosNew />
      +       <PhotosNew onCreate={handleCreate} />
              <PhotosIndex photos={photos} />
            </main>
          );
        }
      ```
      </details>

  - <details><summary>In <strong>src/<ins>Photos</ins>New.jsx</strong>, handle the submit event to run the given props create function</strong></summary>
  
      ```diff
      - export function PhotosNew() {
      + export function PhotosNew({ onCreate }) {

      +   const handleSubmit = (event) => {
      +     event.preventDefault();
      +     onCreate();
      +   };

          return (
            <div>
              <h1>New Photo</h1>
      -       <form>
      +       <form onSubmit={handleSubmit}>
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
          );
        }
      ```
      </details>
      
    > Check the browser console to see the message when you submit the form.

  - In <strong>src/<ins>Photos</ins>Page.jsx</strong>, make a web request within the create function
      
      ```diff
  
        import axios from "axios";
        import { useState, useEffect } from "react";
        import { PhotosIndex } from "./PhotosIndex";
        import { PhotosNew } from "./PhotosNew";

        export function PhotosPage() {
          const [photos, setPhotos] = useState([]);

          const handleIndex = () => {
            console.log("handleIndex");
            axios.get("/photos.json").then((response) => {
              console.log(response.data);
              setPhotos(response.data);
            });
          };


      -   const handleCreate = () => {
      +   const handleCreate = (params, successCallback) => {
            console.log("handleCreate");
      +     axios.post("/photos.json", params).then((response) => {
      +       // adds the new object to the currnet list of objects
      +       setPhotos([...photos, response.data]);
      +       successCallback();
      +     });
          };
  
          useEffect(handleIndex, []);

          return (
            <main>
              <PhotosNew onCreate={handleCreate} />
              <PhotosIndex photos={photos} />
            </main>
          );
        }
      ```

  - In <strong>src/<ins>Photos</ins>New.jsx</strong>, pass the form parameters into the create function</strong>
  
      ```diff
        export function PhotosNew({ onCreate }) {

          const handleSubmit = (event) => {
            event.preventDefault(); // Stops the form from doing its default behavior (refreshing the page)
      -     onCreate();
      +     const form = event.target; // Gets the form element that triggered this event
      +     const params = new FormData(form); // Creates a FormData object containing all the form's input values
      +     const successCallback = () => form.reset(); // Creates a function that will clear the form when called
      +     onCreate(params, successCallback); // Calls the onCreate function passed from the parent with the form data and reset function
          };

          return (
            <div>
              <h1>New Photo</h1>
              <form>
              <form onSubmit={handleSubmit}>
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
          );
        }
      ```
      
    > Check the browser to fill in the HTML form and create a new item.
    > 
    > In your terminal, use git to save a version of your code.
    > ```bash
    > git add --all
    > git commit -m "Add new/create actions"
    > ```
    
- Show action

  - <details><summary>In <strong>src/<ins>Photos</ins>Page.jsx</strong>, make a placeholder show function and pass it to the index component</summary>
  
    ```diff
      import axios from "axios";
      import { useState, useEffect } from "react";
      import { PhotosIndex } from "./PhotosIndex";
      import { PhotosNew } from "./PhotosNew";
      import { Modal } from "./Modal";

      export function PhotosPage() {
        const [photos, setPhotos] = useState([]);
        const [isPhotosShowVisible, setIsPhotosShowVisible] = useState(true);
        const [currentPhoto, setCurrentPhoto] = useState({});
  
        const handleIndex = () => {
          console.log("handleIndex");
          axios.get("/photos.json").then((response) => {
            console.log(response.data);
            setPhotos(response.data);
          });
        };

        const handleCreate = (params, successCallback) => {
          console.log("handleCreate");
          axios.post("/photos.json", params).then((response) => {
            setPhotos([...photos, response.data]);
            successCallback();
          });
        };
  
    +   const handleShow = (photo) => {
    +     console.log("handleShow", photo);
    +   };

        useEffect(handleIndex, []);

        return (
          <main>
            <PhotosNew onCreate={handleCreate} />
    -       <PhotosIndex photos={photos} />
    +       <PhotosIndex photos={photos} onShow={handleShow} />
          </main>
        );
      }
    ```
    </details>
  
  - <details><summary>In <strong>src/<ins>Photos</ins>Index.jsx</strong>, make a button to run the show function</summary>
  
    ```diff
    - export function PhotosIndex({ photos }) {
    + export function PhotosIndex({ photos, onShow }) {
        return (
          <div>
            <h1>All photos ({photos.length} total)</h1>
            {photos.map((photo) => (
              <div key={photo.id}>
                <h2>{photo.name}</h2>
                <img src={photo.url} />
                <p>Width: {photo.width}</p>
                <p>Height: {photo.height}</p>
    +           <button onClick={() => onShow(photo)}>More info</button>
              </div>
            ))}
          </div>
        );
      }
    ```
    </details>
    
    > Check the browser console to see the message when you click each button.
    > 
  - Make a new <strong>src/Modal.css</strong> file with the following content:
  
    ```css
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
    ```

  - Make a new <strong>src/Modal.jsx</strong> file with the following content:
  
    ```jsx
    import "./Modal.css";

    export function Modal({ children, show, onClose }) {
      if (show) {
        return (
          <div className="modal-background">
            <section className="modal-main">
              {/* Renders any content passed between the Modal component's opening and closing tags */}
              {children}
              <button className="close" type="button" onClick={onClose}>
                &#x2715;
              </button>
            </section>
          </div>
        );
      }
    }
    ```

  - <details><summary>In <strong>src/<ins>Photos</ins>Page.jsx</strong>, import the modal component</summary>
  
    ```diff
      import axios from "axios";
      import { useState, useEffect } from "react";
      import { PhotosIndex } from "./PhotosIndex";
      import { PhotosNew } from "./PhotosNew";
    + import { Modal } from "./Modal";

      export function PhotosPage() {
        const [photos, setPhotos] = useState([]);

        const handleIndex = () => {
          console.log("handleIndex");
          axios.get("/photos.json").then((response) => {
            console.log(response.data);
            setPhotos(response.data);
          });
        };

        const handleCreate = (params, successCallback) => {
          console.log("handleCreate");
          axios.post("/photos.json", params).then((response) => {
            setPhotos([...photos, response.data]);
            successCallback();
          });
        };
  
        const handleShow = (photo) => {
          console.log("handleShow", photo);
        };
  
        useEffect(handleIndex, []);

        return (
          <main>
            <PhotosNew onCreate={handleCreate} />
            <PhotosIndex photos={photos} />
    +       <Modal show={true}>
    +         <h1>Test</h1>
    +       </Modal>
          </main>
        );
      }
    ```
    </details>
    
    > Check the browser to see the modal with placeholder HTML

  - <details><summary>In <strong>src/<ins>Photos</ins>Page.jsx</strong>, make state and functions to show and hide the modal</summary>
  
    ```diff
      import axios from "axios";
      import { useState, useEffect } from "react";
      import { PhotosIndex } from "./PhotosIndex";
      import { PhotosNew } from "./PhotosNew";
      import { Modal } from "./Modal";

      export function PhotosPage() {
        const [photos, setPhotos] = useState([]);
    +   const [isPhotosShowVisible, setIsPhotosShowVisible] = useState(false);
  
        const handleIndex = () => {
          console.log("handleIndex");
          axios.get("/photos.json").then((response) => {
            console.log(response.data);
            setPhotos(response.data);
          });
        };

        const handleCreate = (params, successCallback) => {
          console.log("handleCreate");
          axios.post("/photos.json", params).then((response) => {
            setPhotos([...photos, response.data]);
            successCallback();
          });
        };
  
        const handleShow = (photo) => {
          console.log("handleShow", photo);
    +     setIsPhotosShowVisible(true);
        };
  
        useEffect(handleIndex, []);

        return (
          <main>
            <PhotosNew onCreate={handleCreate} />
            <PhotosIndex photos={photos} />
    -       <Modal show={true}>
    +       <Modal show={isPhotosShowVisible} onClose={() => setIsPhotosShowVisible(false)}>
              <h1>Test</h1>
            </Modal>
          </main>
        );
      }
    ```
    </details>
    
    > Check the browser to test showing and closing the modal with the appropriate buttons
            
  - <details><summary>Make a new <strong>src/<ins>Photos</ins>Show.jsx</strong> file that renders attributes from props</summary>
  
    ```jsx
    export function PhotosShow({ photo }) {
      return (
        <div>
          <h1>Photo information</h1>
          <p>Name: {photo.name}</p>
          <p>Url: {photo.url}</p>
          <p>Width: {photo.width}</p>
          <p>Height: {photo.height}</p>
        </div>
      );
    }
    ```
    </details>

  - <details><summary>In <strong>src/<ins>Photos</ins>Page.jsx</strong>, import the show component and use it in the modal</summary>
  
    ```diff
      import axios from "axios";
      import { useState, useEffect } from "react";
      import { PhotosIndex } from "./PhotosIndex";
      import { PhotosNew } from "./PhotosNew";
    + import { PhotosShow } from "./PhotosShow";
      import { Modal } from "./Modal";

      export function PhotosPage() {
        const [photos, setPhotos] = useState([]);
        const [isPhotosShowVisible, setIsPhotosShowVisible] = useState(false);
    +   const [currentPhoto, setCurrentPhoto] = useState({});
  
        const handleIndex = () => {
          console.log("handleIndex");
          axios.get("/photos.json").then((response) => {
            console.log(response.data);
            setPhotos(response.data);
          });
        };

        const handleCreate = (params, successCallback) => {
          console.log("handleCreate");
          axios.post("/photos.json", params).then((response) => {
            setPhotos([...photos, response.data]);
            successCallback();
          });
        };
  
        const handleShow = (photo) => {
          console.log("handleShow", photo);
          setIsPhotosShowVisible(true);
    +     setCurrentPhoto(photo);
        };
  
        useEffect(handleIndex, []);

        return (
          <main>
            <PhotosNew onCreate={handleCreate} />
            <PhotosIndex photos={photos} onShow={handleShow} />
            <Modal show={isPhotosShowVisible} onClose={() => setIsPhotosShowVisible(false)}>
    -         <h1>Test</h1>
    +         <PhotosShow photo={currentPhoto} />
            </Modal>
          </main>
        );
      }
    ```
    </details>
    
    > Check the browser to see the selected item attributes within the modal
    > 
    > In your terminal, use git to save a version of your code.
    > ```bash
    > git add --all
    > git commit -m "Add show action"
    > ```

- Edit/Update actions

  - <details><summary>Modify <strong>src/<ins>Photos</ins>Show.jsx</strong> to include an edit form with default values</summary>
  
    ```diff
      export function PhotosShow({ photo }) {
        return (
          <div>
            <h1>Photo information</h1>
            <p>Name: {photo.name}</p>
            <p>Url: {photo.url}</p>
            <p>Width: {photo.width}</p>
            <p>Height: {photo.height}</p>
    +       <form>
    +         <div>
    +           Name: <input defaultValue={photo.name} name="name" type="text" />
    +         </div>
    +         <div>
    +           Url: <input defaultValue={photo.url} name="url" type="text" />
    +         </div>
    +         <div>
    +           Width: <input defaultValue={photo.width} name="width" type="text" />
    +         </div>
    +         <div>
    +           Height: <input defaultValue={photo.height} name="height" type="text" />
    +         </div>
    +         <button type="submit">Update</button>
    +       </form>
          </div>
        );
      }
    ```
    </details>

    > Check the browser to see the edit HTML form within the modal

  - <details><summary>In <strong>src/<ins>Photos</ins>Page.jsx</strong>, make a placeholder update function and pass it into the show component</summary>
  
    ```diff
      import axios from "axios";
      import { useState, useEffect } from "react";
      import { PhotosIndex } from "./PhotosIndex";
      import { PhotosNew } from "./PhotosNew";
      import { PhotosShow } from "./PhotosShow";
      import { Modal } from "./Modal";

      export function PhotosPage() {
        const [photos, setPhotos] = useState([]);
        const [isPhotosShowVisible, setIsPhotosShowVisible] = useState(false);
        const [currentPhoto, setCurrentPhoto] = useState({});
  
        const handleIndex = () => {
          console.log("handleIndex");
          axios.get("/photos.json").then((response) => {
            console.log(response.data);
            setPhotos(response.data);
          });
        };

        const handleCreate = (params, successCallback) => {
          console.log("handleCreate");
          axios.post("/photos.json", params).then((response) => {
            setPhotos([...photos, response.data]);
            successCallback();
          });
        };
  
        const handleShow = (photo) => {
          console.log("handleShow", photo);
          setIsPhotosShowVisible(true);
          setCurrentPhoto(photo);
        };
            
    +   const handleUpdate = () => {
    +     console.log("handleUpdate");
    +   };
  
        useEffect(handleIndex, []);

        return (
          <main>
            <PhotosNew onCreate={handleCreate} />
            <PhotosIndex photos={photos} onShow={handleShow} />
            <Modal show={isPhotosShowVisible} onClose={() => setIsPhotosShowVisible(false)}>
    -         <PhotosShow photo={currentPhoto} />
    +         <PhotosShow photo={currentPhoto} onUpdate={handleUpdate} />
            </Modal>
          </main>
        );
      }
    ```
    </details>

  - <details><summary>In <strong>src/<ins>Photos</ins>Show.jsx</strong>, handle the form submit to run the props update function</summary>
  
    ```diff
    - export function PhotosShow({ photo }) {
    + export function PhotosShow({ photo, onUpdate }) {
            
    +   const handleSubmit = (event) => {
    +     event.preventDefault();
    +     onUpdate();
    +   };
            
        return (
          <div>
            <h1>Photo information</h1>
            <p>Name: {photo.name}</p>
            <p>Url: {photo.url}</p>
            <p>Width: {photo.width}</p>
            <p>Height: {photo.height}</p>
    -       <form>
    +       <form onSubmit={handleSubmit}>
              <div>
                Name: <input defaultValue={photo.name} name="name" type="text" />
              </div>
              <div>
                Url: <input defaultValue={photo.url} name="url" type="text" />
              </div>
              <div>
                Width: <input defaultValue={photo.width} name="width" type="text" />
              </div>
              <div>
                Height: <input defaultValue={photo.height} name="height" type="text" />
              </div>
              <button type="submit">Update</button>
            </form>
          </div>
        );
      }
    ```
    </details>

    > Check the browser console to see the message when you submit the form.

  - In <strong>src/<ins>Photos</ins>Page.jsx</strong>, make a web request in the update function
  
    ```diff
      import axios from "axios";
      import { useState, useEffect } from "react";
      import { PhotosIndex } from "./PhotosIndex";
      import { PhotosNew } from "./PhotosNew";
      import { PhotosShow } from "./PhotosShow";
      import { Modal } from "./Modal";

      export function PhotosPage() {
        const [photos, setPhotos] = useState([]);
        const [isPhotosShowVisible, setIsPhotosShowVisible] = useState(false);
        const [currentPhoto, setCurrentPhoto] = useState({});
  
        const handleIndex = () => {
          console.log("handleIndex");
          axios.get("/photos.json").then((response) => {
            console.log(response.data);
            setPhotos(response.data);
          });
        };

        const handleCreate = (params, successCallback) => {
          console.log("handleCreate");
          axios.post("/photos.json", params).then((response) => {
            setPhotos([...photos, response.data]);
            successCallback();
          });
        };
  
        const handleShow = (photo) => {
          console.log("handleShow", photo);
          setIsPhotosShowVisible(true);
          setCurrentPhoto(photo);
        };
            
    -   const handleUpdate = () => {
    +   const handleUpdate = (photo, params, successCallback) => {
          console.log("handleUpdate");
    +     axios.patch(`/photos/${photo.id}.json`, params).then((response) => {
    +       setPhotos(photos.map(p => p.id === response.data.id ? response.data : p));
    +       successCallback();
    +       setIsPhotosShowVisible(false);
    +     });
        };
  
        useEffect(handleIndex, []);

        return (
          <main>
            <PhotosNew onCreate={handleCreate} />
            <PhotosIndex photos={photos} onShow={handleShow} />
            <Modal show={isPhotosShowVisible} onClose={() => setIsPhotosShowVisible(false)}>
              <PhotosShow photo={currentPhoto} onUpdate={handleUpdate} />
            </Modal>
          </main>
        );
      }
    ```

  - In <strong>src/<ins>Photos</ins>Show.jsx</strong>, pass the form parameters to the update function
  
    ```diff
      export function PhotosShow({ photo, onUpdate }) {
            
        const handleSubmit = (event) => {
          event.preventDefault();
    -     onUpdate();
    +     const form = event.target;
    +     const params = new FormData(form);
    +     const successCallback = () => form.reset();
    +     onUpdate(photo, params, successCallback);
        };
            
        return (
          <div>
            <h1>Photo information</h1>
            <p>Name: {photo.name}</p>
            <p>Url: {photo.url}</p>
            <p>Width: {photo.width}</p>
            <p>Height: {photo.height}</p>
            <form onSubmit={handleSubmit}>
              <div>
                Name: <input defaultValue={photo.name} name="name" type="text" />
              </div>
              <div>
                Url: <input defaultValue={photo.url} name="url" type="text" />
              </div>
              <div>
                Width: <input defaultValue={photo.width} name="width" type="text" />
              </div>
              <div>
                Height: <input defaultValue={photo.height} name="height" type="text" />
              </div>
              <button type="submit">Update</button>
            </form>
          </div>
        );
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

  - <details><summary>In <strong>src/<ins>Photos</ins>Page.jsx</strong>, make a placeholder destroy function and pass it into the show component</summary>
  
    ```diff
      import axios from "axios";
      import { useState, useEffect } from "react";
      import { PhotosIndex } from "./PhotosIndex";
      import { PhotosNew } from "./PhotosNew";
      import { PhotosShow } from "./PhotosShow";
      import { Modal } from "./Modal";

      export function PhotosPage() {
        const [photos, setPhotos] = useState([]);
        const [isPhotosShowVisible, setIsPhotosShowVisible] = useState(false);
        const [currentPhoto, setCurrentPhoto] = useState({});

        const handleIndex = () => {
          console.log("handleIndex");
          axios.get("/photos.json").then((response) => {
            console.log(response.data);
            setPhotos(response.data);
          });
        };

        const handleCreate = (params, successCallback) => {
          console.log("handleCreate");
          axios.post("/photos.json", params).then((response) => {
            setPhotos([...photos, response.data]);
            successCallback();
          });
        };

        const handleShow = (photo) => {
          console.log("handleShow", photo);
          setIsPhotosShowVisible(true);
          setCurrentPhoto(photo);
        };

        const handleUpdate = (photo, params, successCallback) => {
          console.log("handleUpdate", params);
          axios.patch(`/photos/${photo.id}.json`, params).then((response) => {
            setPhotos(photos.map(p => p.id === response.data.id ? response.data : p));
            successCallback();
            setIsPhotosShowVisible(false);
          });
        };

    +   const handleDestroy = (photo) => {
    +     console.log("handleDestroy", photo);
    +   };

        useEffect(handleIndex, []);

        return (
          <main>
            <PhotosNew onCreate={handleCreate} />
            <PhotosIndex photos={photos} onShow={handleShow} />
            <Modal show={isPhotosShowVisible} onClose={() => setIsPhotosShowVisible(false)}>
    -         <PhotosShow photo={currentPhoto} onUpdate={handleUpdate} />
    +         <PhotosShow photo={currentPhoto} onUpdate={handleUpdate} onDestroy={handleDestroy} />
            </Modal>
          </main>
        );
      }
    ```
    </details>

  - <details><summary>In <strong>src/<ins>Photos</ins>Show.jsx</strong>, make a button to run the props destroy function on click</summary>
  
    ```diff
    - export function PhotosShow({ photo, onUpdate }) {
    + export function PhotosShow({ photo, onUpdate, onDestroy }) {
        const handleSubmit = (event) => {
          event.preventDefault();
          const form = event.target;
          const params = new FormData(form);
          const successCallback = () => form.reset();
          onUpdate(photo, params, successCallback);
        };

        return (
          <div>
            <h1>Photo information</h1>
            <form onSubmit={handleSubmit}>
              <div>
                Name: <input defaultValue={photo.name} name="name" type="text" />
              </div>
              <div>
                Url: <input defaultValue={photo.url} name="url" type="text" />
              </div>
              <div>
                Width: <input defaultValue={photo.width} name="width" type="text" />
              </div>
              <div>
                Height: <input defaultValue={photo.height} name="height" type="text" />
              </div>
              <button type="submit">Update</button>
            </form>
    +       <button onClick={() => onDestroy(photo)}>Destroy</button>
          </div>
        );
      }
    ```
    </details>

    > Check the browser console to see the message when you click the button.

  - In <strong>src/<ins>Photos</ins>Page.jsx</strong>, make a web request in the destroy function
  
    ```diff
      import axios from "axios";
      import { useState, useEffect } from "react";
      import { PhotosIndex } from "./PhotosIndex";
      import { PhotosNew } from "./PhotosNew";
      import { PhotosShow } from "./PhotosShow";
      import { Modal } from "./Modal";

      export function PhotosPage() {
        const [photos, setPhotos] = useState([]);
        const [isPhotosShowVisible, setIsPhotosShowVisible] = useState(false);
        const [currentPhoto, setCurrentPhoto] = useState({});

        const handleIndex = () => {
          console.log("handleIndex");
          axios.get("/photos.json").then((response) => {
            console.log(response.data);
            setPhotos(response.data);
          });
        };

        const handleCreate = (params, successCallback) => {
          console.log("handleCreate");
          axios.post("/photos.json", params).then((response) => {
            setPhotos([...photos, response.data]);
            successCallback();
          });
        };

        const handleShow = (photo) => {
          console.log("handleShow", photo);
          setIsPhotosShowVisible(true);
          setCurrentPhoto(photo);
        };

        const handleUpdate = (id, params, successCallback) => {
          console.log("handleUpdate", params);
          axios.patch(`/photos/${id}.json`, params).then((response) => {
            setPhotos(photos.map(p => p.id === response.data.id ? response.data : p));
            successCallback();
            setIsPhotosShowVisible(false);
          });
        };

        const handleDestroy = (photo) => {
          console.log("handleDestroy", photo);
    +     axios.delete(`/photos/${photo.id}.json`).then(() => {
    +       setPhotos(photos.filter((p) => p.id !== photo.id));
    +       setIsPhotosShowVisible(false);
    +     });
        };

        useEffect(handleIndex, []);

        return (
          <main>
            <PhotosNew onCreate={handleCreate} />
            <PhotosIndex photos={photos} onShow={handleShow} />
            <Modal show={isPhotosShowVisible} onClose={() => setIsPhotosShowVisible(false)}>
              <PhotosShow photo={currentPhoto} onUpdate={handleUpdate} onDestroy={handleDestroy} />
            </Modal>
          </main>
        );
      }
    ```

    > Check the browser to destroy a photo by clicking the destroy button
    > 
    > In your terminal, use git to save a version of your code.
    > ```bash
    > git add --all
    > git commit -m "Add destroy action"
    > ```
    