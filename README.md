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
| File | /src/components/TutorialsList.js ||
| Folder | /src/services |
| File | /src/services/TutorialService.js |







