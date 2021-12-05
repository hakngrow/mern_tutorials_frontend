## Tutorials Listing app using MERN stack - React Webapp

### Features

We will build a tutorials listing web application using React in that:

- Each tutorial has an id, title, description, and published status.
- We can create, retrieve, update, and delete tutorials.
- There is a search bar for finding tutorials by title.
- The app will consume REST APIs from a `node.js` backend.  For the repo of the backend, click [here](https://github.com/hakngrow/mern_tutorials_backend).

### Project Architecture

![Project Architecture](/public/images/architecture.jpg)

| Component | Function |
| --- | --- |
| App | Container with [React Router](https://reactrouter.com/) that links routes paths to components |
| TutorialsList | Retrieves and displays a list of all tutorials |
| Tutorial | Displays a form for editing a tutorial’s data based on `:id` |
| AddTutorial | Displays a form for submission a new tutorial |
| TutorialService | Use [`axios`](https://www.npmjs.com/package/axios) to make HTTP requests and receive responses from backend REST APIs |

### Project Structure

![Project Structure](/public/images/structure.jpg)

| File | Description |
| --- | --- |
| `package.json` | Contains 4 dependency modules: `react`, `react-router-dom`, `axios` and `bootstrap` |
| `http-common.js` | Initializes `axios` with HTTP base URL and headers |
| `.env` | Configures port for this React app |

### Project Setup

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


### Install and Setup `bootstrap`

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

### Add `bootstrap` Navigation Bar 

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

### Install and Setup `axios`

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


### Create Tutorials Data Service

Next we will create a data service that uses the `axios` module above to send HTTP requests to the REST APIs at the configured `baseURL`.

We call 'axios' (imported as 'http') `get`, `post`, `put`, `delete` functions corresponding to the HTTP GET, POST, PUT, DELETE methods to make CRUD operations.

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













