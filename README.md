# React DevTools bugs

This example project demonstrates two bugs, X and Y using React DevTools in standalone mode. This was run on:
* macOS 11.0.1 Big Sur, on an Apple Silicon (M1) chip
* Safari 14.0.1
* React 17.0.1
* React DevTools 4.1.1,  running in standalone mode via Electron 11.1.0 (darwin-arm64 build)
* New project using `create-react-app .` with the [useScript hook](https://usehooks.com/useScript/) to add the `<script>` tag required by React DevTools.

There are two problems:

## Electron 11.1.0 required for M1
react-devtools expects Electron 9.x.x; however builds for darwin-arm64 are only available for 11.1.0. Installation fails with:

```
npm ERR! code 1
npm ERR! path /Users/michael/git/components-blank/node_modules/electron
npm ERR! command failed
npm ERR! command sh -c node install.js
 HTTPError: Response code 404 (Not Found) for https://github.com/electron/electron/releases/download/v9.4.0/electron-v9.4.0-darwin-arm64.zip
 ...
 ```

**Resolution:** The `electron` version  for `react-devtools` should be bumped to 11.1.0.

If using Yarn, you can add a `resolutions` entry to `package.json` specifying which version to install:
```
"resolutions": {
  "electron": "^11.0.1"
}
```
...then `yarn install`.

Otherwise, install the Electron package at 11.1.0 (`npm install --save-dev electron@11.1.0) to download the arm64 distribution, and symlink two paths:
```
ln -s node_modules/electron/dist node_modules/react-devtools/node_modules/electron/dist
ln -s node_modules/electron/path.txt node_modules/react-devtools/node_modules/electron/path.txt
```

## Components view is blank
A `<script>` for the React DevTools is added in `App.js`. Start the DevTools in standalone mode

```
./node_modules/react-devtools/bin.js
```

Start the React app's development server:

```
yarn start
```

Refresh the React app if needed to establish a connection with the DevTools. The _Components_ view is blank (no matter what filters are applied):

![](blank-components-react-devtools.gif)