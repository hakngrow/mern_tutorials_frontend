## Tutorials Listing app using MERN stack - React Webapp

### 1. Features

We will build a tutorials listing web application using React in that:

- Each tutorial has an id, title, description, and published status.
- We can create, retrieve, update, and delete tutorials.
- There is a search bar for finding tutorials by title.
- The app will consume REST APIs from a `node.js` backend.  For the repo of the backend, click [here](https://github.com/hakngrow/mern_tutorials_backend).

### 2. Project Architecture

![Project Architecture](/public/images/architecture.jpg)

| Component | Function |
| --- | --- |
| App | Container with [React Router](https://reactrouter.com/) that links routes paths to components |
| TutorialsList | Retrieves and displays a list of all tutorials |
| Tutorial | Displays a form for editing a tutorial’s data based on `:id` |
| AddTutorial | Displays a form for submission a new tutorial |
| TutorialService | Use [`axios`](https://www.npmjs.com/package/axios) to make HTTP requests and receive responses from backend REST APIs |

### 3. Project Structure

![Project Structure](/public/images/structure.jpg)

| File | Description |
| --- | --- |
| `package.json` | Contains 4 dependency modules: `react`, `react-router-dom`, `axios` and `bootstrap` |
| `http-common.js` | Initializes `axios` with HTTP base URL and headers |
| `.env` | Configures port for this React app |

### 4. Project Setup

At the directory you wish to store the project, start a terminal or command line session.

Run the command `npx create-react-app tutorials-frontend`.

After the process is done, create the following folders/files:

| Type | Name |
| --- | --- |
| Folder | /src/components |
| File | /src/components/AddTutorial.js |
| File | /src/components/Tutorial.js |
| File | /src/components/TutorialsList.js |
| Folder | /src/services |
| File | /src/services/TutorialService.js |
| File | /src/http-common.js |


### 5. Install and Setup `bootstrap`

Run the command `npm install bootstrap` to install the `bootstrap` module for styling.

Modify `/src/App.js` as below to import the `bootstrap` CSS:

```
import React from "react";

import "bootstrap/dist/css/bootstrap.min.css";

function App() {
  return (
    ...
  );
}

export default App;
```

### Install and Setup React Router

Run the command `npm install react-router-dom` to install React Router.

Modify `/src/index.js` to wrap the `App` component in the `BrowserRouter` component as below:

```
import React from 'react';
import ReactDOM from 'react-dom';
import { BrowserRouter } from "react-router-dom";

import App from './App';

ReactDOM.render(
  <BrowserRouter>
    <App />
  </BrowserRouter>,
  document.getElementById('root')
);
```

### 6. Add `bootstrap` Navigation Bar 

Modify `App.js` to include a navigation bar with React Router `<Link>` anchor tags.  Each `<link>` tag will have a path to link to (e.g. `/tutorials`).  The path links to a `<Route>` within the `<Switch>` tag. And each `<Route>` tag points to a React component (e.g. `/src/components/TutorialsList.js`).

```
import React from "react";
import { Switch, Route, Link } from "react-router-dom";

import "bootstrap/dist/css/bootstrap.min.css";
import "./App.css";

import AddTutorial from "./components/AddTutorial";
import Tutorial from "./components/Tutorial";
import TutorialsList from "./components/TutorialsList";

function App() {
  return (
    <div>
      <nav className="navbar navbar-expand navbar-dark bg-dark">
        <a href="/tutorials" className="navbar-brand">
          Home
        </a>
        <div className="navbar-nav mr-auto">
          <li className="nav-item">
            <Link to={"/tutorials"} className="nav-link">
              Tutorials
            </Link>
          </li>
          <li className="nav-item">
            <Link to={"/add"} className="nav-link">
              Add
            </Link>
          </li>
        </div>
      </nav>

      <div className="container mt-3">
        <Switch>
          <Route exact path={["/", "/tutorials"]} component={TutorialsList} />
          <Route exact path="/add" component={AddTutorial} />
          <Route path="/tutorials/:id" component={Tutorial} />
        </Switch>
      </div>
    </div>
  );
}

export default App;
```

### 7. Install and Setup `axios`

Run the command `npm install axios` to install the `axios` module.

Modify `/src/http-common.js` as below:

```
import axios from "axios";

export default axios.create({
  baseURL: "http://localhost:8080/api",
  headers: {
    "Content-type": "application/json"
  }
});
```

Change the `baseURL` to where your REST APIs are hosted.  For more information about `axios`, click [here](https://axios-http.com/).

### 8. Create Tutorials Data Service

Next we will create a data service that uses the `axios` module above to send HTTP requests to the REST APIs at the configured `baseURL`.

We call the `axios` (imported as 'http') `get`, `post`, `put`, `delete` functions corresponding to the HTTP GET, POST, PUT, DELETE methods to make CRUD operations.

The service exports the following CRUD and finder operations:
- CREATE: create
- RETRIEVE: getAll, get
- UPDATE: update
- DELETE: remove, removeAll
- FINDER: findByTitle

```
import http from "../http-common";

const getAll = () => {
  return http.get("/tutorials");
};

const get = (id) => {
  return http.get(`/tutorials/${id}`);
};

const create = (data) => {
  return http.post("/tutorials", data);
};

const update = (id, data) => {
  return http.put(`/tutorials/${id}`, data);
};

const remove = (id) => {
  return http.delete(`/tutorials/${id}`);
};

const removeAll = () => {
  return http.delete(`/tutorials`);
};

const findByTitle = (title) => {
  return http.get(`/tutorials?title=${title}`);
};

const services = {
  getAll,
  get,
  create,
  update,
  remove,
  removeAll,
  findByTitle,
};

export default services;
```

 ### 9. Create React Components
 
 Next we need to build 3 React components corresponding to the 3 `<Route>` tags we defined [earlier](https://github.com/hakngrow/mern_tutorials_frontend/blob/master/README.md#6-add-bootstrap-navigation-bar).
 
 #### 9.1 `AddTutorial` Component
 
 The `AddTutorial` component renders a form to create a new tutorial with 2 fields: `title` and `description`.
 
![AddTutorial Component](/public/images/AddTutorial.jpg)

Modify the `/src/components/AddTutorial.js` as follow:

```
import React, { useState } from "react";
import TutorialDataService from "../services/TutorialService";

const AddTutorial = () => {
  const initialTutorialState = {
    id: null,
    title: "",
    description: "",
    published: false,
  };
  const [tutorial, setTutorial] = useState(initialTutorialState);
  const [submitted, setSubmitted] = useState(false);

  const handleInputChange = (event) => {
    const { name, value } = event.target;
    setTutorial({ ...tutorial, [name]: value });
  };

  const saveTutorial = () => {
    var data = {
      title: tutorial.title,
      description: tutorial.description,
    };

    TutorialDataService.create(data)
      .then((response) => {
        setTutorial({
          id: response.data.id,
          title: response.data.title,
          description: response.data.description,
          published: response.data.published,
        });
        setSubmitted(true);
        console.log(response.data);
      })
      .catch((e) => {
        console.log(e);
      });
  };

  const newTutorial = () => {
    setTutorial(initialTutorialState);
    setSubmitted(false);
  };

  return (
    <div className="submit-form">
      {submitted ? (
        <div>
          <h4>You submitted successfully!</h4>
          <button className="btn btn-success" onClick={newTutorial}>
            Add
          </button>
        </div>
      ) : (
        <div>
          <div className="form-group">
            <label htmlFor="title">Title</label>
            <input
              type="text"
              className="form-control"
              id="title"
              required
              value={tutorial.title}
              onChange={handleInputChange}
              name="title"
            />
          </div>

          <div className="form-group">
            <label htmlFor="description">Description</label>
            <input
              type="text"
              className="form-control"
              id="description"
              required
              value={tutorial.description}
              onChange={handleInputChange}
              name="description"
            />
          </div>

          <button onClick={saveTutorial} className="btn btn-success">
            Submit
          </button>
        </div>
      )}
    </div>
  );
};

export default AddTutorial;
```

First, we define and set initial state of `tutorial` and  `submitted`.

Next, we create the `handleInputChange()` function to track the values of the input and set the `tutorial` state for changes. We also have a function to get `tutorial` state and send the POST request to the REST API. It calls the `TutorialService.create()` function.

On return, we check the `submitted` state, if it is true, we show the `Add` button for creating a new tutorial again. Otherwise, a form with a `Submit` button will be display.

#### 9.2 `TutorialsList` Component

The `TutorialsList` component has the following features:
- A search bar for finding tutorials by `title`
- A list of tutorial titles displayed on the left with a `Remove All` button at the bottom
- A selected tutorial's `title`, `description` and `status` is displayed on the right with a `Edit` button at the button

![TutorialsList Component](/public/images/TutorialsList.jpg)

In this component, we have following states:
- `searchTitle` - The input value of the tutorial title search bar
- `tutorials` - The list of tutorials in the list
- `currentTutorial` - The selected tutorial 
- `currentIndex` - The list index of the selcted tutorial

We will need to use 3 `TutorialService` functions:
- `getAll()` - To retrieve all tutorials
- `removeAll()` - To delete all tutorials
- `findByTitle()` - To find tutorials by `title`

Modify `/src/components/TutorialsList.js` as follows:
```
import React, { useState, useEffect } from "react";
import TutorialService from "../services/TutorialService";
import { Link } from "react-router-dom";

const TutorialsList = () => {
  const [tutorials, setTutorials] = useState([]);
  const [currentTutorial, setCurrentTutorial] = useState(null);
  const [currentIndex, setCurrentIndex] = useState(-1);
  const [searchTitle, setSearchTitle] = useState("");

  useEffect(() => {
    retrieveTutorials();
  }, []);

  const onChangeSearchTitle = (e) => {
    const searchTitle = e.target.value;
    setSearchTitle(searchTitle);
  };

  const retrieveTutorials = () => {
    TutorialService.getAll()
      .then((response) => {
        setTutorials(response.data);
        console.log(response.data);
      })
      .catch((e) => {
        console.log(e);
      });
  };

  const refreshList = () => {
    retrieveTutorials();
    setCurrentTutorial(null);
    setCurrentIndex(-1);
  };

  const setActiveTutorial = (tutorial, index) => {
    setCurrentTutorial(tutorial);
    setCurrentIndex(index);
  };

  const removeAllTutorials = () => {
    TutorialService.removeAll()
      .then((response) => {
        console.log(response.data);
        refreshList();
      })
      .catch((e) => {
        console.log(e);
      });
  };

  const findByTitle = () => {
    TutorialService.findByTitle(searchTitle)
      .then((response) => {
        setTutorials(response.data);
        console.log(response.data);
      })
      .catch((e) => {
        console.log(e);
      });
  };

  return (
    // ...
  );
};

export default TutorialsList;
```

We use the `useEffect` hook with an empty dependency array to retrieve a list of all tutorials when the component mounts.

To implement the UI elements of the component, we use the following JSX:

```
...
import { Link } from "react-router-dom";

const TutorialsList = () => {
  ...

  return (
    <div className="list row">
      <div className="col-md-8">
        <div className="input-group mb-3">
          <input
            type="text"
            className="form-control"
            placeholder="Search by title"
            value={searchTitle}
            onChange={onChangeSearchTitle}
          />
          <div className="input-group-append">
            <button
              className="btn btn-outline-secondary"
              type="button"
              onClick={findByTitle}
            >
              Search
            </button>
          </div>
        </div>
      </div>
      <div className="col-md-6">
        <h4>Tutorials List</h4>

        <ul className="list-group">
          {tutorials &&
            tutorials.map((tutorial, index) => (
              <li
                className={
                  "list-group-item " + (index === currentIndex ? "active" : "")
                }
                onClick={() => setActiveTutorial(tutorial, index)}
                key={index}
              >
                {tutorial.title}
              </li>
            ))}
        </ul>

        <button
          className="m-3 btn btn-sm btn-danger"
          onClick={removeAllTutorials}
        >
          Remove All
        </button>
      </div>
      <div className="col-md-6">
        {currentTutorial ? (
          <div>
            <h4>Tutorial</h4>
            <div>
              <label>
                <strong>Title:</strong>
              </label>{" "}
              {currentTutorial.title}
            </div>
            <div>
              <label>
                <strong>Description:</strong>
              </label>{" "}
              {currentTutorial.description}
            </div>
            <div>
              <label>
                <strong>Status:</strong>
              </label>{" "}
              {currentTutorial.published ? "Published" : "Pending"}
            </div>

            <Link
              to={"/tutorials/" + currentTutorial.id}
              className="badge badge-warning"
            >
              Edit
            </Link>
          </div>
        ) : (
          <div>
            <br />
            <p>Please click on a Tutorial...</p>
          </div>
        )}
      </div>
    </div>
  );
};

export default TutorialsList;
```

On clicking the `Edit` button, the app will render the `Tutorial` component.  We use the React Router `<Link>` tag to access that component with the path URL `/tutorials/:id`, where `id` is the `id` of the selected tutorial.

#### 9.3 `Tutorial` Component

For retrieving, updating and deleting a tutorial, this component 3 `TutorialService` functions:
- `get()` - To retireve a tutorial by `id`
- `update()` - To edit a tutorial
- `delete()` - To remove a tutorial by `id`




















