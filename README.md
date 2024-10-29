# Docker

Curs - 18.10.2024

## How to work with Docker and Yarn and Vite

### Create a new application

- Step 1: Open Docker Desktop

- Step 2: Open Terminal

- Step 3: Go to the project folder
  - `cd <path-to-folder>`

- Step 4: Create the project docker structure
  - Create file `.docker/Dockerfile`

  ```dockerfile
  FROM node:20-alpine3.18

  RUN corepack enable
  RUN corepack prepare yarn@4.5.0 --activate
  RUN yarn set version 4.5.0
  RUN yarn -v
  RUN yarn config set --home enableTelemetry 0

  RUN apk add --no-cache git
  RUN apk add --no-cache openssh
  ```

  - Create file `compose.yaml`

  ```yaml
  services:
  app:
    build: .docker
    stdin_open: true
    tty: true
    container_name: app.curs1
    environment:
      NODE_ENV: development
      CHOKIDAR_USEPOLLING: true
      CHOKIDAR_INTERVAL: 100
    ports:
      - "5173:5173"
    expose:
      - "5173"
    volumes:
      - .:/app
      # exclude
      - /app/.git/
    working_dir: /app
    user: 1000:1000
  ```

- Step 5: Rename the container `container_name` from `composer.yaml` as you want

- Step 6: Start the container
  - `docker compose up -d`

- Step 7: Start the docker terminal
  - `docker compose exec app sh`
  - `app` is the `app` from the `composer.yaml` service

- Step 8: To get out of the container terminal, just write `exit` in terminal

- Step 9: To close and remove container, please type the following command:
  - `docker compose down`

- Step 10: If I want to install something in the container using `.docker/Dockerfile`, I also need to run:

  ```bash
  docker compose down
  docker compose build
  ```

- Step 11: In the docker terminal (see Step 7), please install a new vite vanilla project likewise:
  - `yarn create vite`
  - it will ask for the project name and other things like:
    - Project name: _curs1_
    - Select a framework: **Vanilla**
    - Select a variant: **JavaScript**

- Step 12: `vite` will create a new project folder, go in that folder:
  - `cd curs1`

- Step 13: Run the install command to install the project packages
  - `yarn install` or simple `yarn`

- Step 14: Change the `package.json` file to use `host` option
  - `"dev": "vite --host"` instead of `"dev": "vite"`

- Step 15: Run the script that you have in `package.json` file
  - `yarn dev`

### How to add a new library

You can add a new library by typing the following command:

  ```bash
  yarn add bootstrap
  yarn add tailwindcss
  ```

### How to remove a library

You can remove a library by typing the following command:

  ```bash
  yarn remove bootstrap
  yarn remove tailwindcss
  ```

### Comands
