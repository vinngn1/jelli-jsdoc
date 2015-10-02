
# JSDoc Theme for Jelli Doccumentation


## Introduction

JSDoc 3 is an API documentation generator for JavaScript, similar to JavaDoc or PHPDoc. You add documentation comments directly to your source code, right along side the code itself. The JSDoc Tool will scan your source code, and generate a complete HTML documentation website for you.

JSDoc's purpose is to document the API of your JavaScript application or library. It is assumed that you will want to document things like: namespaces, classes, methods, method parameters, etc.

JSDoc comments should generally be placed immediately before the code being documented. It must start with a `/** `sequence in order to be recognized by the JSDoc parser. Comments beginning with `/*`, `/***`, or more than 3 stars will be ignored. This is a feature to allow you to suppress parsing of comment blocks.

  - To learn more about JSDoc, go to [UseJSDoc.org](http://usejsdoc.org/)
  - For an example of JSDoc documentation, visit [Jaguar.js](http://davidshimjs.github.io/jaguarjs/doc/index.html)


## Jelli Commenting Convention

In our code base we already used `/**` with `**/` as closing comment without purposely to generate docs. To avoid junk docs to be generated, those comments closed with `**/` will be ignore.

    /**
     *  These comments in here will be used to generate docs
     */


    /**
     *  These comments like this will be ignored
     **/


## For new project or existing project without JSDoc set up

1. Make sure your git submodule is up to date

        cd scripts
        git submodule update --remote
        cd ..

2. Copy the example configuration file from the `scripts/jsdoc` directory to configuration directory

        cp scripts/jsdoc/conf.example.json configuration/jsdoc.configuration.json

3. Modify application name for your project in `configuration/jsdoc.configuration.json`
    
    `applicationName: "App Name"`: specify your app name

4. Copy these two below lines to the dependencies section in `package.json` of your project

        "jaguarjs-jsdoc": "git+https://github.com/vinngn1/jelli-jsdoc.git",
        "jsdoc": "^3.3.3"

    Then run

        npm install

5. Start generate documentation as for existing project.

        node_modules/.bin/jsdoc -c configuration/jsdoc.configuration.json

    Your docs can be seen at `www.appname.localhost.jelli.com:1234/docs`


## For existing project with JSDoc already set up

After pulling out a Jelli project from github, you can run the following command to generate the documentation. The generated docs will be in `docs` directory of your project directory.

    npm install
    node_modules/.bin/jsdoc -c configuration/jsdoc.configuration.json
