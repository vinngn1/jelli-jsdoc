
# Jelli's JSDoc Guide


## Installation

JSDoc 3 is an API documentation generator for JavaScript, similar to JavaDoc or PHPDoc. You add documentation comments directly to your source code, right along side the code itself. The JSDoc Tool will scan your source code, and generate a complete HTML documentation website for you.

JSDoc's purpose is to document the API of your JavaScript application or library. It is assumed that you will want to document things like: namespaces, classes, methods, method parameters, etc.

JSDoc comments should generally be placed immediately before the code being documented. It must start with a `/** `sequence in order to be recognized by the JSDoc parser. Comments beginning with `/*`, `/***`, or more than 3 stars will be ignored. This is a feature to allow you to suppress parsing of comment blocks.

  - To learn more about JSDoc, go to [UseJSDoc.org](http://usejsdoc.org/)
  - For an example of JSDoc documentation, visit [Jaguar.js](http://davidshimjs.github.io/jaguarjs/doc/index.html)

### For new project or existing project without JSDoc set up

1. Make sure your git submodule is up to date

        cd scripts
        git checkout master
        git pull origin master
        cd ..


2. Copy the example configuration file from the `scripts/jsdoc` directory to configuration directory

        cp scripts/jsdoc/conf.example.json configuration/jsdoc.configuration.json

3. Modify application name for your project in `configuration/jsdoc.configuration.json`
    
    `applicationName: "App Name"`: specify your app name

4. Copy these two below lines to the dependencies section in `package.json` of your project

        "jelli-jsdoc-theme": "^0.0.2",
        "jsdoc": "^3.3.3"

    Then run

        npm install

5. Start generate documentation as for existing project.

        node_modules/.bin/jsdoc -c configuration/jsdoc.configuration.json

    Your docs can be seen at `www.appname.localhost.jelli.com:1234/docs`


### For existing project with JSDoc already set up

After pulling out a Jelli project from github, you can run the following command to generate the documentation. The generated docs will be in `docs` directory of your project directory.

    npm install
    node_modules/.bin/jsdoc -c configuration/jsdoc.configuration.json


## Jelli Documenting Convention

In our code base we already used `/**` with `**/` as closing comment without purposely to generate docs. To avoid junk docs to be generated, those comments closed with `**/` will be ignore.

    /**
     *  These comments in here will be used to generate docs
     */


    /**
     *  These comments like this will be ignored
     **/

### Document a module

It's required by [Jelli's] convention that each module need to be documented. Description, module name are required.

    /**
     * An UI Module represent a play Log as a row in a table 
     * @module timelineEventView
     * @copyright  Copyright ©2015 Jelli, Inc.
     * @author Vinh Nguyen
     */
    define([
        'ember', 
        'compiledTemplates/timelineEventViewTemplate'
    ], function(
        Ember,
        timelineEventViewTemplate
    ){
        // module content
    });


### Document a method/fucntion

By convention, each method is required to be documented with at least description. `@param` and `@return` can be used as well as `@private`.

Inside a AMD module, for a method which is not part of the module returning object, it's required to specify `@method methodName`. Otherwise, JSDoc is able to pick up the method name.

    /**
     * An inner function to format duration used in the controller
     * @method durationFormat
     * @param  {string} time timestamps string
     * @param  {number} decimal number of floating number
     * @return {string} formated duration
     * @private
     */
    function durationFormat (time, decimal) {
        return "some value";
    }

    var timelineEventView = Ember.View.extend({
        /**
         * A small description is sufficient
         */
        didInsertElement: function() {
        },

        /**
         * When having `property` or `observe` attached at the end, 
         * without flagging using `@method`, JSDoc will understand 
         * this is a member instead of a method
         * @method
         */
        tooltipContent: function () {
        }.property('play.hasWarning')
    });

### Document a property/member

It's good to specify a description and type for members/properties
    
    var timelineEventView = Ember.View.extend({
        /**
         * This is a member/property
         * @type {String}
         */
        classNameBindings: ":cool-class",

        /**
         * This is another member/property
         * @type {Boolean}
         */
        isNegated: false
    });

### Document enum inside AMD module

Some extra effords is required when it comes to document an enum inside an AMD module. Without using `@alias: module:muduleName.enumName`, JSDoc wont understand and be able to generate docs as desired.

    /**
     * Enum for Completion Status values.
     * @enum {String}
     * @alias module:timelineEventView.COMPLETION_STATUS
     */
    var COMPLETION_STATUS = {
        /** Completed without error */
        "COMPLETED": 'completionErrorCompleted',
        /** Stopped */
        "STOPPED": 'completionErrorIsStopped',
        /** Interrupted */
        "INTERRUPTED": 'completionErrorIsInterrupted',
        /** Appliance Error */
        "UNKNOWN": 'completionErrorIsUnknown'
    };

### Full Example

[![See Screenshot](output.png)](output.png)

    /**
     * An UI Module represent a play log as a row in a table 
     * @module timelineEventView
     * @author Vinh Nguyen
     * @copyright  Copyright ©2015 Jelli, Inc.
     */

    define([
      'ember', 
      'compiledTemplates/timelineEventViewTemplate'
    ], function(
      Ember,
      timelineEventViewTemplateTemplate
    ){
        /**
         * Enum for Completion Status values.
         * @enum {String}
         * @alias module:timelineEventView.COMPLETION_STATUS
         */
        var COMPLETION_STATUS = {
            /** Completed without error */
            "COMPLETED": 'completionErrorCompleted',
            /** Stopped */
            "STOPPED": 'completionErrorIsStopped',
            /** Interrupted */
            "INTERRUPTED": 'completionErrorIsInterrupted',
            /** Appliance Error */
            "UNKNOWN": 'completionErrorIsUnknown'
        };

        /**
         * An inner function to format duration used in the view
         * @method durationFormat
         * @param  {string} time timestamps string
         * @param  {number} decimal number of floating number
         * @return {string} formated duration
         * @private
         */
        function durationFormat (time, decimal) {
            return "some value";
        }

        var timelineEventView = Ember.View.extend({
            /**
             * If it's been negated
             * @type {Boolean}
             */
            isNagated: false,

            /**
             * Bottom Selectors
             * @type {String}
             */
            bottomSelectors: ".footer",

            /**
             * Bootstrap tooltips need to be initalized after inserted into the DOM
             */
            didInsertElement: function() {
                // function body
            },

            /**
             * A function to build tooltip content in case there is an error happened with the play
             * @method
             * @return {string} Tooltip content
             */
            tooltipContent: function () {
                if (this.get('play.completionErrorIsUnknown')) {
                    return this._durationFormat(this.get('play.adObject.duration')) + "s ad interrupted at " + this._durationFormat(this.get('play.duration'), 1) + "s by appliance error";
                }

                if (this.get('play.completionErrorIsInterrupted')) {
                    return this._durationFormat(this.get('play.adObject.duration')) + "s ad interrupted at " + this._durationFormat(this.get('play.duration'), 1) + "s";
                }

                if (this.get('play.completionErrorIsStopped')) {
                    return this._durationFormat(this.get('play.adObject.duration')) + "s ad stopped at " + this._durationFormat(this.get('play.duration'), 1) + "s";
                }
            }.property('play.hasError')
      });

        return timelineEventView;
    });

