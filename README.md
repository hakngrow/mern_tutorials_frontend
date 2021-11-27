## Tutorials Listing app using MERN stack - React Webapp

### Features

We will build a tutorials listing web application using React in that:

- Each tutorial has an id, title, description, and published status.
- We can create, retrieve, update, and delete tutorials.
- There is a search bar for finding tutorials by title.
- The app will consume REST APIs from a `node.js` backend.  For the repo of the backend, click [here](https://github.com/hakngrow/mern_tutorials_backend).

### React application architecture

![Project Architecture](/public/images/architecture.jpg)

 The `App.js` component is a container with [React Router](https://reactrouter.com/). It has a navigation bar (from [bootstrap](https://www.npmjs.com/package/bootstrap)) that links to routes paths.
 
| Component | Function |
| --- | --- |
| TutorialsList | Retrieves and displays a list of all tutorials |
| Tutorial | Displays a form for editing a tutorial’s data based on `:id` |
| AddTutorial | Displays a form for submission a new tutorial |

