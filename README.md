# Express TypeScript Boilerplate
This repo is a starting point for backend development with Nodejs. It includes Docker and is CI/CD optimized. 

## I. Installation


### Manual Method

#### 1. Clone this repo

```
$ git clone https://github.com/kenselasie/NodeJS_Boilerplate.git your-app-name
$ cd your-app-name
```

#### 2. Install dependencies

```
$ npm i
```

## II. Configuration

#### Update Docker repository for actions
```
$ npm run setup-actions
```

## III. Development

### Start dev server
Starting the dev server also starts MongoDB as a service in a docker container using the compose script at `docker-compose.dev.yml`.

```
$ npm run dev
```
Running the above commands results in 
* 🌏**API Server** running at `http://localhost:3000`
* ⚙️**Swagger UI** at `http://localhost:3000/dev/api-docs`
* 🛢️**MongoDB** running at `mongodb://localhost:27017`

## IV. Packaging and Deployment

The mongo container is only only available in dev environment. When you build and deploy the docker image, be sure to provide the correct **[environment variables](#environment)**.

#### 1. Build and run without Docker

```
$ npm run build && npm run start
```

#### 2. Run with docker

```
$ docker build -t api-server .
$ docker run -t -i \
      --env NODE_ENV=production \
      --env MONGO_URL=mongodb://host.docker.internal:27017/books \
      -p 3000:3000 \
      api-server
```

#### 3. Run with docker-compose

```
$ docker-compose up
```


---

## Environment
To edit environment variables, create a file with name `.env` and copy the contents from `.env.default` to start with.

| Var Name  | Type  | Default | Description  |
|---|---|---|---|
| NODE_ENV  | string  | `development` |API runtime environment. eg: `staging`  |
|  PORT | number  | `3000` | Port to run the API server on |
|  MONGO_URL | string  | `mongodb://localhost:27017/books` | URL for MongoDB |

## Logging
The application uses [winston](https://github.com/winstonjs/winston) as the default logger. The configuration file is at `src/logger.ts`.
* All logs are saved in `./logs` directory and at `/logs` in the docker container.
* The `docker-compose` file has a volume attached to container to expose host directory to the container for writing logs.
* Console messages are prettified
* Each line in error log file is a stringified JSON.


### Directory Structure

```
+-- scripts
|   +-- dev.sh
|   +-- setup-github-actions.sh
+-- src
|   +-- controllers
|   |   +-- book
|   |   |   +-- add.ts
|   |   |   +-- all.ts
|   |   |   +-- index.ts
|   |   |   +-- search.ts
|   +-- errors
|   |   +-- application-error.ts
|   |   +-- bad-request.ts
|   +-- lib
|   |   +-- console-logger
|   |   |   +-- index.ts
|   |   |   +-- winston-transport.ts
|   |   +-- safe-mongo-connection.ts
|   +-- middleware
|   |   +-- request-middleware.ts
|   +-- models
|   |   +-- plugins
|   |   |   +-- timestamp-plugin.ts
|   |   +-- Book.ts
|   +-- public
|   |   +-- index.html
|   +-- app.ts
|   +-- mongo-connection.ts
|   +-- routes.ts
|   +-- server.ts
+-- .env.default
+-- .eslintrc.json
+-- .gitignore
+-- docker-compose.dev.yml
+-- docker-compose.yml
+-- Dockerfile
+-- jest.config.js
+-- LICENSE
+-- nodemon.json
+-- openapi.json
+-- package-lock.json
+-- package.json
+-- README.md
+-- tsconfig.json
```
