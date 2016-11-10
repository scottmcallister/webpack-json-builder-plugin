# webpack-json-builder-plugin
A webpack plugin that can be used to bundle JSON files containing locale information by name

To install:

```
$ npm install --save webpack-json-bundler-plugin
```

# Use Case

Imagine having a React application. Each component will need to define strings for its user interface. If these components should be multilingual, you will need locale files.

Using a directory layout like this:

```
src/
  components/
    component-1/
      locales/
        de-de.json
        en-us.json
      component-1.js
      component-1.scss
    component-2/
      locales/
        de-de.json
        fr-fr.json
      component-2.js
      component-2.scss
dist/
  locales/
    de-de.json
    en-us.json
    fr-fr.json
  app.js
```

In our build process, we want to merge all language files for each language together. E.g. component-1/locales/de-de.json and component-2/locales/de-de.json will be merged into a single file de-de.json, with their contents available as properties derivered from the file path.

The content of component-1/locales/de-de.json will be available as the deep property components.component-1 inside the bundle de-de.json file.

# Example 

In your webpack.config.js:

```javascript
var JsonBundlerPlugin = require('webpack-json-bundler-plugin');

module.exports = {
  // ...

  plugins: [
    new JSONBundlerPlugin({
      fileInput: '**/locales/*.json',
		  omit: /\/locales|\/?components|\/?services|\/?scenes|\/?features/g,
		  rootDirectory: 'src',
      localeDirectory: 'locales/'
    }),
  ]

}
```

# Options

You can pass the following options as parameters. 

* `omit` - a regular expression describing which directories to omit from the locale path in the bundled JSON

Example: 
```javascript
   dir1/dir2/components/dir3
   if 'omit' = /\/?components/g
   the resulting locale path in your bundled JSON will be dir1/dir2/dir3
```

* `fileInput` - a string that is used to identify all locale files that you wish to include in your bundled JSON.

* `localeDirectory` - the directory in which you want webpack to put the newly created locale files. 

Example: 
```javascript
   if localeDirectory: 'locales/'
   You will find your locale files in the locale folder within the directory where your webpack bundle is being output
   ex. 'dist/locales/fr-FR.json ...
```

* `rootDirectory` - the uppermost directory within your project where all locale files can be found. In the example project structure
above, this would be 'src'.


