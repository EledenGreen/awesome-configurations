<!-- toc -->

- [Frontend](#frontend)
  - [Vite](#vite)
  - [Axios](#axios)
  - [json-server](#json-server)
  - [ESLint](#eslint)
  - [Prettier](#prettier)
  - [Token](#token)
  - [State Management System (Redux/Toolkit)](#state-management-system-reduxtoolkit)
    - [Redux Configuration for React:](#redux-configuration-for-react)
  - [React Router](#react-router)
- [Backend](#backend)
  - [Node HTTP Server](#node-http-server)
  - [Express](#express)
  - [CORS](#cors)
  - [Express Router](#express-router)
  - [Middleware Setup](#middleware-setup)
  - [VS Code REST Client for Testing](#vs-code-rest-client-for-testing)
  - [Lint](#lint)
  - [Prettier (Backend)](#prettier-backend)
  - [Encryption using bcrypt](#encryption-using-bcrypt)
  - [JSON Web Token](#json-web-token)
- [Database](#database)
- [Testing](#testing)
  - [Backend Integration Testing](#backend-integration-testing)
  - [Vitest Frontend Testing #configuration](#vitest-frontend-testing-%23configuration)
  - [Playwright Configuration #configuration](#playwright-configuration-%23configuration)
- [Styling](#styling)
- [Deploying to Render](#deploying-to-render)

<!-- tocstop -->

## Frontend

### Vite

- [Vite](https://vitejs.dev/) framework
  ```shell
  npm create vite@latest name -- --template react
  ```
  For easy transpiling and bundling.

### Axios

- [Axios](https://axios-http.com/docs/intro)
  ```shell
  npm install axios
  ```
  For AJAX communication.

### json-server

- [json-server](https://github.com/typicode/json-server/tree/v0) — **for local server, good for frontend testing**
  ```shell
  npm install json-server --save-dev
  ```
  ```jsx
  "server": "json-server -p3001 --watch db.json"
  ```

### ESLint

- [ESLint](https://eslint.org/)
  // Vite has already ESLint config installed.

### Prettier

- [Prettier](https://prettier.io/)
  ```shell
  npm install --save-dev eslint-config-prettier
  ```

### Token

In frontend, the token is fetched from backend (generated in backend) than used.

### State Management System (Redux/Toolkit)

- #### Redux Configuration for React:
  ```bash
  npm install redux
  ```
  [Redux](https://redux.js.org/) is a state management library for JavaScript applications, particularly useful in large or complex applications where managing state across multiple components becomes challenging.
  ```bash
  npm install react-redux
  ```
  [react-redux](https://react-redux.js.org/) is the official Redux binding for React. It provides a way to connect your Redux store with your React components. This connection allows React components to access the Redux store's state and dispatch actions to update the state. React-Redux simplifies the integration of Redux into React applications by providing hooks and higher-order components.
  ```bash
  npm install @reduxjs/toolkit
  ```
  [redux-toolkit](https://redux-toolkit.js.org/) is a set of tools and best practices designed to make Redux development more efficient and maintainable. It includes utilities like `createSlice` for defining Redux reducers and actions in a more concise manner, `configureStore` for setting up the Redux store with sensible defaults, and `createAsyncThunk` for handling asynchronous actions. Redux Toolkit aims to reduce boilerplate code and promote good Redux practices.

### React Router

- [React Router](https://reactrouter.com/)
  ```shell
  npm install react-router-dom
  ```

## Backend

- ### Node HTTP Server
  - Implementing server with Node's built-in http
    ```shell
    npm init
    ```
    ```jsx
    "start": "node --watch index.js"
    ```
  - Using Node's library: Express (recommended)
    ```shell
    npm init
    ```
    ```jsx
    "start": "node --watch index.js" // not using nodemon
    ```
- ### Express

  - [express](https://expressjs.com/)
    ```shell
    npm install express
    ```
    ```shell
    npm install express-async-errors
    ```
    - This reduces the need to use try/catch.
  - `index.js`

    ```jsx
    const express = require('express')
    const app = express()

    app.use(express.json())
    ```

    - To access the data easily, we need the help of the Express json-parser that we can use with the command `app.use(express.json())`.
    - Without the json-parser, the body property would be undefined. The json-parser takes the JSON data of a request, transforms it into a JavaScript object, and then attaches it to the body property of the request object before the route handler is called.

- ### CORS

  - [CORS](https://www.npmjs.com/package/cors)

    ```shell
    npm install cors
    ```

    ```jsx
    const cors = require('cors')

    app.use(cors())
    ```

- ### Express Router

  - [Router](http://expressjs.com/en/api.html#router)

    ```jsx
    const notesRouter = require('express').Router()

    module exports = notesRouter
    ```

- ### Middleware Setup
- ### VS Code REST Client for Testing
- ### Lint
  - Too many configs can be confusing. Currently using this:
    ```shell
    npm init @eslint/conifg@latest
    ```
    - Note: Standard is a formatter, so use Prettier instead.
  - Create `.eslintignore` and add: `dist`
  - Script:
    ```json
    "scripts": {
      "lint": "eslint ."
    }
    ```
- ### Prettier (Backend)
  - Install Prettier config:
    ```shell
    npm install --save-dev eslint-config-prettier
    ```
- ### Encryption using bcrypt
  - [bcrypt](https://www.npmjs.com/package/bcrypt)
    ```shell
    npm install bcrypt
    ```
    ```jsx
    const bcrypt = require('bcrypt')
    const saltRounds = 10
    const password = await bcrypt.hash(password, saltRounds)
    ```
- ### JSON Web Token

  - [JSON web token](https://www.npmjs.com/package/jsonwebtoken)

    ```shell
    npm install jsonwebtoken
    ```

    ```jsx
    const jwt = require('jsonwebtoken')

    const token = jwt.sign(userForToken, process.env.SECRET)
    ```

  - Set SECRET in `.env` to any string:
    ```jsx
    SECRET = empty
    ```

## Database

- Setup MongoDB Atlas
- [mongoose](https://mongoosejs.com/)
  ```shell
  npm install mongoose
  ```
  - Elegant [mongodb](https://www.mongodb.com/) object modeling for [node.js](https://nodejs.org/en/).
  - `app.js`
    ```jsx
    mongoose.set('strictQuery', false)
    mongoose.connect(url)
    // ...
    ```
    ```jsx
    mongoose.connection.close()
    ```
    - Note: This is not used in production because:
      - **In long-running applications** (like web servers): No need to call `mongoose.connection.close()` within the main application flow.
      - **In application shutdown or testing scenarios**: Yes, it's a good practice to close the MongoDB connection.
- Environment variables using [dotenv](https://www.npmjs.com/package/dotenv)
  ```shell
  npm install dotenv
  ```
  ```jsx
  MONGODB_URI=mongodb+srv://fullstack:thepasswordishere@cluster0.o1opl.mongodb.net/noteApp?retryWrites=true&w=majority
  PORT=3001
  ```
  - Add .env file to .gitignore.
  - At `index.js`
    ```jsx
    require('dotenv').config()
    ```
- Format objects
  ```jsx
  noteSchema.set('toJSON', {
    transform: (document, returnedObject) => {
      returnedObject.id = returnedObject._id.toString()
      delete returnedObject._id
      delete returnedObject.__v
    },
  })
  ```
- Use of _morgan_ dropped — logger for backend communication.

## Testing

- #### Backend Integration Testing

  - In Windows, separate modes script needs _cross-env_ library to work:
    ```shell
    npm install cross-env
    ```
  - Separate modes

    ```json
    "scripts": {
      "start": "cross-env NODE_ENV=production node index.js",
      "dev": "cross-env NODE_ENV=development nodemon index.js",
      "test": "cross-env NODE_ENV=test node --test",
      // ...
    }
    ```

    - _utils/config.js_

      ```jsx
      const MONGODB_URI =
        process.env.NODE_ENV === 'test'
          ? process.env.TEST_MONGODB_URI
          : process.env.MONGODB_URI

      module.exports = {
        MONGODB_URI,
        PORT,
      }
      ```

    - _.env_

      ```jsx
      MONGODB_URI=mongodb+srv://fullstack:thepasswordishere@cluster0.o1opl.mongodb.net/noteApp?retryWrites=true&w=majority
      PORT=3001

      TEST_MONGODB_URI=mongodb+srv://fullstack:thepasswordishere@cluster0.o1opl.mongodb.net/testNoteApp?retryWrites=true&w=majority
      ```

  - Database
    - Recommended Docker container (don't know how to set up yet)
    - Currently using MongoDB Atlas, not an optimal solution in situations where many people are developing the same application.
  - [supertest](https://github.com/visionmedia/supertest) package to help us write our tests for testing the API.
    ```shell
    npm install --save-dev supertest
    ```
    - _.test.js_
      ```jsx
      const supertest = require('supertest')
      const api = supertest(app)
      ```
  - Using node's built-in `node:test`.
  - Use _.test.js_ file extension.
  - [Lodash](https://lodash.com/) — **use if needed**.
  - Test commands
    ```shell
    npm test -- --test-only
    ```
    ```shell
    npm test -- tests/note_api.test.js
    ```
    ```shell
    npm run test -- --test-name-pattern="notes"
    ```
  - express-async library to handle exceptions under the hood.
    ```shell
    npm install express-async-errors
    ```
    ```jsx
    require('express-async-errors')
    ```

- Frontend (not an integration test)

  - [Vitest](https://vitest.dev/)

    - #### Vitest Frontend Testing #configuration

      - Install
        ```shell
        npm install --save-dev vitest jsdom
        ```
        ```shell
        npm install --save-dev @testing-library/react @testing-library/jest-dom
        ```
      - Add script to the _package.json_
        ```json
        {
          "scripts": {
            // ...
            "test": "vitest run"
          }
          // ...
        }
        ```
      - Create _testSetup.js_

        ```javascript
        import { afterEach } from 'vitest'
        import { cleanup } from '@testing-library/react'
        import '@testing-library/jest-dom/vitest'

        afterEach(() => {
          cleanup()
        })
        ```

        This resets jsdom, which is simulating the browser.

      - Expand _vite.config.js_
        ```javascript
        export default defineConfig({
          // ...
          test: {
            environment: 'jsdom',
            globals: true,
            setupFiles: './testSetup.js',
          },
        })
        ```
      - Resolve eslint error

        ```shell
        npm install --save-dev eslint-plugin-vitest-globals
        ```

        And enable plugin at _.eslint.cjs_

        ```javascript
        module.exports = {
          root: true,
          env: {
            browser: true,
            es2020: true,

            'vitest-globals/env': true, //<<<<<-------------
          },
          extends: [
            'eslint:recommended',
            'plugin:react/recommended',
            'plugin:react/jsx-runtime',
            'plugin:react-hooks/recommended',

            'plugin:vitest-globals/recommended', //<<<<<------------
          ],
          // ...
        }
        ```

      - Enable _user-event_
        ```shell
        npm install --save-dev @testing-library/user-event
        ```
      - To check **test coverage**
        ```shell
        npm test -- --coverage
        ```
        -

- E2E test using Playwright

  - #### Playwright Configuration #configuration

    - In a new directory, (Playwright doesn't need to be in the project directory)
      ```shell
      npm init playwright@latest
      ```
    - Installation answers:
      ![image.png](./assets/playwright.png)
    - In _package.json_
      ```json
      {
        // ...
        "scripts": {
          "test": "playwright test",
          "test:report": "playwright show-report"
        }
        // ...
      }
      ```
    - npm script for the _backend_, which will enable it to be started in testing mode, i.e. so that _NODE_ENV_ gets the value _test_.
      ```json
      {
        // ...
        "scripts": {
          "start": "NODE_ENV=production node index.js",
          "dev": "NODE_ENV=development nodemon index.js",
          "build:ui": "rm -rf build && cd ../frontend/ && npm run build && cp -r build ../backend",
          "deploy": "fly deploy",
          "deploy:full": "npm run build:ui && npm run deploy",
          "logs:prod": "fly logs",
          "lint": "eslint .",
          "test": "NODE_ENV=test node --test",
          "start:test": "NODE_ENV=test node index.js" //<<<----------------
        }
        // ...
      }
      ```
    - Change browser:
      ```shell
      npm test -- --project chromium
      ```
    - Configure _playwright.config.js_
      ```javascript
      module.exports = defineConfig({
        timeout: 3000,
        fullyParallel: false,
        workers: 1,
        // ...
      })
      ```
    - To start _backend_ in _test_ mode
      ```shell
      npm run start:test
      ```
    - baseUrl
      The frontend address _http:localhost:5173_ and the backend address _http:localhost:3001_ are hardcoded for tests. Of these, the address of the backend is actually useless because a proxy has been defined in the Vite configuration of the frontend, which forwards all requests made by the frontend to the address _http:localhost:5173/api_ to the backend.
      So we can replace all the addresses in the tests from [http://localhost:3001/api/..](http://localhost:3001/) to [http://localhost:5173/api/..](http://localhost:5173/).
      In _playwright.config.js_
      ```javascript
      module.exports = defineConfig({
        // ...
        use: {
          baseURL: 'http://localhost:5173',
        },
        // ...
      })
      ```
    - **Playwright VS Code Extension**
      - Install extension.
      - Open test on sidebar.
      - Tests can be run individually.
      - Supports live debugging.
      - Trace viewer.
      - Browser.

  - Testing
    - Start Frontend in dev mode
      ```shell
      npm run dev
      ```
    - Start Backend in test mode
      ```shell
      npm run start:test
      ```
      - Note: This is different from `npm run test` where it executes backend testing. Here the backend is running in test mode (i.e., connecting to the test database).
    - Run Playwright test
      - Commands
        ```shell
        npm run test
        ```
        ```shell
        npm run test:report
        ```
        ```shell
        npm run test -- --ui
        ```
        ```shell
        npm test -- -g "login fails with wrong password"
        ```
        - For single test. We can also use `only` and run _npm run test_.
        ```shell
        npm test -- -g'importance can be changed' --debug
        ```
        ```shell
        npm run test -- --trace on
        ```
        ```shell
        npm playwright codegen http://localhost:5173/
        ```
        - Playwright [test generator](https://playwright.dev/docs/codegen-intro).
      - VS Code extension — **recommended**.

## Styling

- [Tailwind CSS](https://tailwindcss.com/) for Vite framework
  - Installation
    ```shell
    npm install -D tailwindcss postcss autoprefixer
    npx tailwindcss init -p
    ```
  - Configuration in your `tailwind.config.js` file.
    ```javascript
    /** @type {import('tailwindcss').Config} */
    export default {
      content: ['./index.html', './src/**/*.{js,ts,jsx,tsx}'],
      theme: {
        extend: {},
      },
      plugins: [],
    }
    ```
  - Add the `@tailwind` directives for each of Tailwind’s layers to your `./src/index.css` file.
    ```css
    @tailwind base;
    @tailwind components;
    @tailwind utilities;
    ```

## Deploying to Render

- Relative URL
  - In frontend services, change the base URL to use a relative path:
    ```javascript
    baseUrl = '/api/notes'
    ```
  - Note: This change will cause the frontend to not work in dev mode. To resolve this, configure a proxy in `vite.config.js`:
    ```javascript
    server: {
      proxy: {
        '/api': {
          target: 'http://localhost:3001',
          changeOrigin: true,
        }
      }
    }
    ```
- At backend
  ```javascript
  const PORT = process.env.PORT || 3001
  app.listen(PORT, () => {
    console.log(`Server running on port ${PORT}`)
  })
  ```
- npm run build (Frontend)
  - Streamlining from the backend to automate the build process:
    ```javascript
    "build:ui": "rm -rf dist && cd ../frontendDirectory && npm run build && cp -r dist ../backendDirectory"
    ```
- At backend middleware
  ```javascript
  app.use(express.static('dist'))
  ```
  - Whenever Express receives an HTTP GET request, it first checks if the `dist` directory contains a file corresponding to the request's address. If a correct file is found, Express returns it. Now, HTTP GET requests to the address www.serversaddress.com/index.html or www.serversaddress.com will show the React frontend. GET requests to the address www.serversaddress.com/api/notes will be handled by the backend code.
- Change the root repository where backend src is in Render
  ![Render Root Repository](./assets/github.png)  
  ![Render Settings](./assets/render.png)
