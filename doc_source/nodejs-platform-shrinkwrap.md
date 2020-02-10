# Locking dependencies with npm shrinkwrap<a name="nodejs-platform-shrinkwrap"></a>

The Node\.js platform runs `npm install` each time you deploy\. When new versions of your dependencies are available, they will be installed when you deploy your application, potentially causing the deployment to take a long time\.

You can avoid upgrading dependencies by creating an `npm-shrinkwrap.json` file that locks down your application's dependencies to the current version\.

```
$ npm install
$ npm shrinkwrap
wrote npm-shrinkwrap.json
```

Include this file in your source bundle to ensure that dependencies are only installed once\.