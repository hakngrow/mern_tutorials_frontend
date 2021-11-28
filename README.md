## Tutorials Listing app using MERN stack - React Webapp

### Features

We will build a tutorials listing web application using React in that:

- Each tutorial has an id, title, description, and published status.
- We can create, retrieve, update, and delete tutorials.
- There is a search bar for finding tutorials by title.
- The app will consume REST APIs from a `node.js` backend.  For the repo of the backend, click [here](https://github.com/hakngrow/mern_tutorials_backend).

### React application architecture

![Project Architecture](/public/images/architecture.jpg)

| Component | Function |
| --- | --- |
| App | Container with [React Router](https://reactrouter.com/) that links routes paths to components |
| TutorialsList | Retrieves and displays a list of all tutorials |
| Tutorial | Displays a form for editing a tutorial’s data based on `:id` |
| AddTutorial | Displays a form for submission a new tutorial |
| TutorialService | Use [`axios`](https://www.npmjs.com/package/axios) to make HTTP requests and receive responses from backend REST APIs |

### Project structure

![Project Structure](/public/images/structure.jpg)

| File(s) | Description |
| --- | --- |
| `package.json` | Contains 4 dependency modules: `react`, `react-router-dom`, `axios` & `bootstrap` |
| `http-common.js` | Initializes `axios` with HTTP base URL and headers |
| `.env` | Configures port for this React app |
