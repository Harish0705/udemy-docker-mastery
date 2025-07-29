FROM node:6-alpine 
# Base image: Uses Node.js version 6 on Alpine Linux.
# Alpine is a minimal, security-oriented distro, ideal for small Docker images.

EXPOSE 3000
#Declares that the container will listen on port 3000.

RUN apk add --no-cache tini
# Installs `tini`** using Alpine’s package manager `apk`.
# `--no-cache`: Prevents caching of index files to keep the image small.
# `tini`** is a minimal init system that handles **zombie processes** and signal forwarding. Prevents signal issues in containers.

WORKDIR /usr/src/app
# Sets the working directory inside the container.
# All subsequent commands (COPY, RUN, CMD) will run relative to this path.

COPY package.json package.json
# Copies only the `package.json` file into the container’s current `WORKDIR`.
# Done first to **leverage Docker’s build cache**—npm install will re-run only if `package.json` changes.

RUN npm install && npm cache clean --force
# Installs dependencies listed in package.json.
# Then clears the npm cache to reduce image size and avoid leftover files.

COPY . .
# Copies **all project files** from the current directory (host) into the container’s `WORKDIR` (`/usr/src/app`).
# Should ideally have a `.dockerignore` to avoid copying unnecessary files (e.g., `.git`, `node_modules`, etc.).

CMD [ "/sbin/tini", "--", "node", "./bin/www" ]

# Sets the default command the container runs.
# Uses tini as the entrypoint to properly manage child processes.
# Launches the app using: node ./bin/www
