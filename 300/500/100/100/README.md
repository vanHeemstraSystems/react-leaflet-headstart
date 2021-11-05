# 100 - Service Setup

## 100 - Install Create React App globally

```
$ npm install -g create-react-app@3.4.1
```

## 200 - Setup the directory structure

Not Applicable

## 300 - Generate a new app

Inside the subdirectory /containers/app/ created a new app called '***leaflet***':

```
$ cd containers/app
$ npm init react-app leaflet --use-npm
$ cd leaflet
```

You will be prompted as follows:

```
found 27 vulnerabilities (8 moderate, 18 high, 1 critical)
  run `npm audit fix` to fix them, or `npm audit` for details

Success! Created webui at /Users/willemvanheemstra/git/react-leaflet-headstart/containers/app/leaflet
Inside that directory, you can run several commands:

  npm start
    Starts the development server.

  npm run build
    Bundles the app into static files for production.

  npm test
    Starts the test runner.

  npm run eject
    Removes this tool and copies build dependencies, configuration files
    and scripts into the app directory. If you do this, you can’t go back!

We suggest that you begin by typing:

  cd webui
  npm start

Happy hacking!
```

Take care of the reported vulnerabilities by running the following command from within the webui directory.

```
$ cd leaflet
$ npm audit fix
```

Add a .gitignore file to the ```leaflet``` folder.

```
$ cd containers/app/leaflet
$ touch .gitignore
```

Add the following to the .gitignore file.

```
# See https://help.github.com/articles/ignoring-files/ for more about ignoring files.

# dependencies
/node_modules
/.pnp
.pnp.js

# testing
/coverage

# production
/build

# misc
.DS_Store
.env.local
.env.development.local
.env.test.local
.env.production.local

npm-debug.log*
yarn-debug.log*
yarn-error.log*
```
containers/app/leaflet/.gitignore
