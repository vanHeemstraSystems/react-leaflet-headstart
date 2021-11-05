# 200 - Docker for Development Environment

Create a file called ```Dockerfile.dev``` in the ```leaflet``` directory.

```
$ cd containers/app/leaflet
$ touch Dockerfile.dev
```

Add the following content to this ```Dockerfile.dev``` file:

```
ARG IMAGE_REPOSITORY
# pull official base image
FROM ${IMAGE_REPOSITORY}/node:13.12.0-alpine

# See https://stackoverflow.com/questions/29261811/use-docker-compose-env-variable-in-dockerbuild-file
ARG PROXY_USER
ARG PROXY_PASSWORD
ARG PROXY_FQDN
ARG PROXY_PORT

ENV HTTP_PROXY="http://${PROXY_USER}:${PROXY_PASSWORD}@${PROXY_FQDN}:${PROXY_PORT}"
ENV HTTPS_PROXY="http://${PROXY_USER}:${PROXY_PASSWORD}@${PROXY_FQDN}:${PROXY_PORT}"

# set working directory
WAS: WORKDIR /app

# add `/app/node_modules/.bin` to $PATH
ENV PATH /app/node_modules/.bin:$PATH

# install app dependencies
COPY package.json ./
COPY package-lock.json ./
RUN npm install --silent

# add app
COPY . ./

# expose port
EXPOSE 8081

# start app
CMD ["npm", "start"]
```
containers/app/leaflet/Dockerfile.dev

***Note***: if you are ***not*** behind a proxy, comment out the following lines in Dockerfile.dev, like so:

```
# See https://stackoverflow.com/questions/29261811/use-docker-compose-env-variable-in-dockerbuild-file
# ARG PROXY_USER
# ARG PROXY_PASSWORD
# ARG PROXY_FQDN
# ARG PROXY_PORT

# ENV HTTP_PROXY="http://${PROXY_USER}:${PROXY_PASSWORD}@${PROXY_FQDN}:${PROXY_PORT}"
# ENV HTTPS_PROXY="http://${PROXY_USER}:${PROXY_PASSWORD}@${PROXY_FQDN}:${PROXY_PORT}"
```
containers/app/leaflet/Dockerfile.dev

Create a file called ```.dockerignore``` inside the ```leaflet``` directory.

```
$ cd containers/app/leaflet
$ touch .dockerignore 
```

Add the following content to ```.dockerignore```:

```
node_modules
.dockerignore
Dockerfile.dev
Dockerfile.prod
```
containers/app/leaflet/.dockerignore

Now let us add the ```leaflet``` service to ```sample.docker-compose.dev.yml``` by this entry:

```
...
service:
...
  leaflet:
    build:
      context: ./leaflet
      dockerfile: Dockerfile.dev
      args: # from env_file
        IMAGE_REPOSITORY: ${IMAGE_REPOSITORY}
        PROXY_USER: ${PROXY_USER}
        PROXY_PASSWORD: ${PROXY_PASSWORD}
        PROXY_FQDN: ${PROXY_FQDN}
        PROXY_PORT: ${PROXY_PORT}
    env_file:
      - .env
    container_name: leaflet-dev      
    ports:
      - "8081:3000"
    volumes:
      - ./leaflet:/app
      - /app/node_modules
...

```
containers/app/sample.docker-compose.dev.yml

Now it is time to build the development Docker Image, and run the container specifying its name as "leaflet-dev" to distinguish it from possible other stacks that are called "app" (the default name, based on the root directory), now including the ```leaflet``` service.

```
$ cd containers/app
$ docker-compose --file docker-compose.dev.yml up --project-name leaflet-dev --build -d
```

**Note**:   
```
-p, --project-name NAME     Specify an alternate project name
                              (default: directory name)
```

Fingers crossed ... !

If successful, you can browse to the start page of the new React App, which will look like below:

SCREENSHOT HERE

http://localhost:8080

Now check if we can also see the ```leaflet``` server at http://localhost:8081

SCREENSHOT HERE

http://localhost:8081

Bring down the container before moving on:

```
$ docker-compose --file docker-compose.dev.yml stop
```
