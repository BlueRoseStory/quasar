---
title: App Extension Uninstall API
---

This page refers to `src/uninstall.js` file which is executed when the App Extension is uninstalled. Not all App Extensions will need an uninstall -- this is an optional step.

Example of basic structure of the file:

```js
module.exports = function (api) {
  // props and methods for "api" Object
  // are described below
}
```

## api.extId
Contains the `ext-id` (String) of this App Extension.

## api.prompts
Is an Object which has the answers to the prompts when this App Extension gets installed. For more info on prompts, check out [Prompts API](/app-extensions/development-guide/prompts-api).

## api.resolve
Resolves paths within the app on which this App Extension is running. Eliminates the need to import `path` and resolve the paths yourself.

```js
// resolves to root of app
api.resolve.app('src/my-file.js')

// resolves to root/src of app
api.resolve.src('my-file.js')

// resolves to root/src-pwa of app
api.resolve.pwa('some-file.js')

// resolves to root/src-ssr of app
api.resolve.ssr('some-file.js')

// resolves to root/src-cordova of app
api.resolve.cordova('config.xml')

// resolves to root/src-electron of app
api.resolve.electron('some-file.js')
```

## api.appDir
Contains the full path (String) to the root of the app on which this App Extension is running.

## api.hasExtension
Check if another app extension is installed.

```js
/**
 * Check if another app extension is installed
 *
 * @param {string} extId
 * @return {boolean} has the extension installed.
 */
if (api.hasExtension(extId)) {
  // hey, we have it
}
```

## api.removePath
Removes a file or folder from the app project folder (which the App Extension has installed and is no longer needed).

Be mindful about it and do not delete the files that would break developer's app.

The path to file or folder needs to be relative to project's root folder.

```js
/**
  * @param {string} __path
  */
api.removePath('my-folder')
```

The above example deletes "my-folder" from the root of the app.

## api.getPersistentConf

<q-badge label="@quasar/app v1.0.0-beta.25+" />

Get the internal persistent config of this extension. Returns empty object if it has none.

```js
/**
 * @return {object} cfg
 */
api.getPersistentConf()
```

## api.onExitLog
Adds a message to be printed after App CLI finishes up uninstalling the App Extension and is about to exit. Can be called multiple times to register multiple exit logs.

```js
/**
 * @param {string} msg
 */
api.onExitLog('Thanks for having used my extension')
```
