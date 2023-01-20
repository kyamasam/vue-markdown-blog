## Create a Sample Electron App

```
mkdir my-electron-app && cd my-electron-app
npm init

```

This will guide you through the process where you will end up with an ```package.json```
Add the following to your ```package.json``` file

```
"scripts": {
    "start": "electron ."
  }
```

This tells your app that each time you run  ```npm start```, ```electron .``` should be executed.

### Install electron into your app's devDependencies

``` 
npm install --save-dev electron
```
Your ```package.json``` file should look as follows

```json
{
  "name": "sample-electron",
  "version": "1.0.0",
  "description": "Hello World!",
  "main": "main.js",
  "author": "Jane Doe",
  "license": "MIT",
  "scripts": {
    "start": "electron ."
  },
  "devDependencies": {
    "electron": "^22.0.2"
  },
  "dependencies": {}
}

```

### Create an index.html page with the following contents

```angular2html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <!-- https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP -->
    <meta http-equiv="Content-Security-Policy" content="default-src 'self'; script-src 'self'">
    <title>Hello World!</title>
  </head>
  <body>
    <h1>Hello World!</h1>
    We are using Node.js <span id="node-version"></span>,
    Chromium <span id="chrome-version"></span>,
    and Electron <span id="electron-version"></span>.
  </body>
</html>
```

### Create the entry point for your application, main.js with the following contents
```js
const { app, BrowserWindow } = require('electron')
const path = require('path')

function createWindow () {
  const win = new BrowserWindow({
    width: 800,
    height: 600,
    webPreferences: {
      preload: path.join(__dirname, 'preload.js')
    }
  })

  win.loadFile('./index.html') // points to your apllications index file
}

app.whenReady().then(() => {
  createWindow()

  app.on('activate', () => {
    if (BrowserWindow.getAllWindows().length === 0) {
      createWindow()
    }
  })
})

app.on('window-all-closed', () => {
  if (process.platform !== 'darwin') {
    app.quit()
  }
})
```
### Create a preload.js file

```js
window.addEventListener('DOMContentLoaded', () => {
  const replaceText = (selector, text) => {
    const element = document.getElementById(selector)
    if (element) element.innerText = text
  }

  for (const type of ['chrome', 'node', 'electron']) {
    replaceText(`${type}-version`, process.versions[type])
  }
})

```

Running ```npm start``` should now launch your electron app


## Create a sample vue Js App with vite

```

```

## add the following configurations to your vite.config.js
this prevents vite from building your application with absolute paths and prefers relative paths instead
```js
export default defineConfig({
  base: "",
  // more code here
});

```

## Use Environment variables
Electron throws errors with history mode enabled, view this post (here)[https://nklayman.github.io/vue-cli-plugin-electron-builder/guide/commonIssues.html]
For more information.
In order to history mode when building the electron app, we should have a way of telling the application hte difference between 
our normal build process and our electron build process.

### create .env file and an .env.electron file
Learn more about how vite works with environments (here)[https://vitejs.dev/guide/env-and-mode.html]
Add the following contents in the .env file

```dotenv
VITE_IS_ELECTRON=false
```
Add the following contents in the .env.electron file
```dotenv
VITE_IS_ELECTRON=true
```

Add the following code where your Vue Router has been instantiated

```js
const router = createRouter({
  history: import.meta.env.VITE_IS_ELECTRON
    ? createWebHashHistory()
    : createWebHistory(import.meta.env.BASE_URL),
  // more code here
});

```

## Building the vue application

```
vite build --mode electron
```

The output of the build process is placed in the dist folder
Copy the dist folder and paste it in the electron project as is.

```
 **dist**
 node_modules
 public
 scr
 ...
```

## Telling electron to use the files from the dist folder
Modify the `main.js` file in the electron project to point to the index file of the new vue js folder

Remove the following line from `main.js`  

```js
win.loadFile('./index.html') // points to your apllications index file
```

And replace it with.

```
win.loadFile('./dist/index.html') // points to your apllications index file
```

Running 
```
npm start should now launch your packaged the vue js app  
```