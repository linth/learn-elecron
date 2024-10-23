# Electron

## Quick start

## Prerequisites

check version of node and npm.
```
node -v
npm -v
```

## Create your application
```
mkdir my-electron-app && cd my-electron-app
```

## initial your application
```
npm init
```

## modify your package.json

please note "main" option.

```
{
  "name": "my-electron-app",
  "version": "1.0.0",
  "description": "Hello World!",
  "main": "main.js",
  "author": "Jane Doe",
  "license": "MIT"
}
```

## install electron by using cli
```
npm install --save-dev electron
```

## add script cli in package.json
```
{
  "scripts": {
    "start": "electron ."
  }
}
```

## add 3 files in your application
- main.js
- preload.js
- index.html

```javascript
// main.js
const { app, BrowserWindow } = require('electron/main')
const path = require('node:path')

function createWindow () {
  const win = new BrowserWindow({
    width: 800,
    height: 600,
    webPreferences: {
      preload: path.join(__dirname, 'preload.js')
    }
  })

  win.loadFile('index.html')
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

```javascript
// preload.js
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

```html
<!-- index.html -->
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Hello World!</title>
    <meta http-equiv="Content-Security-Policy" content="script-src 'self' 'unsafe-inline';" />
</head>
<body>
    <h1>Hello World!</h1>
    <p>
        We are using Node.js <span id="node-version"></span>,
        Chromium <span id="chrome-version"></span>,
        and Electron <span id="electron-version"></span>.
    </p>
</body>
</html>
```


## execute your application
```
npm start
```

![](https://www.electronjs.org/assets/images/simplest-electron-app-849f2d68df0c27475bfb850ed5d171a6.png)


## Reference
- [Electron quick start](https://www.electronjs.org/docs/latest/tutorial/quick-start/)