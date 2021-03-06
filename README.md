*DEPRECATION NOTICE*: This project is no longer maintained. Grunt has corrected the issues which made this plugin useful.

# nopt-grunt

> Allows additional command line options to be properly registered using [nopt](https://github.com/isaacs/nopt) along with those natively supported by [grunt](http://gruntjs.org).

[![Build Status](https://travis-ci.org/shannonmoeller/nopt-grunt.png?branch=master)](https://travis-ci.org/shannonmoeller/nopt-grunt)
[![NPM version](https://badge.fury.io/js/nopt-grunt.png)](http://badge.fury.io/js/nopt-grunt)
[![Dependency Status](https://gemnasium.com/shannonmoeller/nopt-grunt.png)](https://gemnasium.com/shannonmoeller/nopt-grunt)
[![Stories in Ready](http://badge.waffle.io/shannonmoeller/nopt-grunt.png)](http://waffle.io/shannonmoeller/nopt-grunt)
[![Bitdeli Badge](https://d2weczhvl823v0.cloudfront.net/shannonmoeller/nopt-grunt/trend.png)](https://bitdeli.com/free "Bitdeli Badge")


## Install

With [Node.js](http://nodejs.org)

    $ npm install nopt-grunt

## Example

Grunt is awesome. Grunt's support for using additional command line options is not awesome. The current [documentation](http://gruntjs.com/api/grunt.option) is misleading in that they give examples of using boolean flags and options with values, but they don't tell you that it only works that way with a single option. Try and use more than one option and things fall apart quickly.

```js
// Gruntfile.js -> grunt --foo
module.exports = function(grunt) {
    assert.strictEqual(grunt.option('foo'), true); // pass
    grunt.registerTask('default', []);
};

// Gruntfile.js -> grunt --bar=42
module.exports = function(grunt) {
    assert.strictEqual(grunt.option('bar'), 42);   // pass
    grunt.registerTask('default', []);
};

// Gruntfile.js -> grunt --foo --bar=42
module.exports = function(grunt) {
    assert.strictEqual(grunt.option('foo'), true); // fail -> foo === '--bar=42'
    assert.strictEqual(grunt.option('bar'), 42);   // fail -> bar === undefined
    grunt.registerTask('default', []);
};
```

Not helpful. Back to the [documentation](http://gruntjs.com/creating-tasks#cli-options-environment)! Still [not helpful](http://gruntjs.com/frequently-asked-questions#options).  Enter `nopt-grunt`.

```js
// Gruntfile.js -> grunt --foo --bar=42
module.exports = function(grunt) {
    // require it at the top and pass in the grunt instance
    require('nopt-grunt')(grunt);

    // new method now available
    grunt.initOptions({
        // longhand
        foo: {
            info: 'Some flag of interest.',
            type: Boolean
        },

        // shorthand
        bar: [Number, null]
    });

    assert.strictEqual(grunt.option('foo'), true); // pass!
    assert.strictEqual(grunt.option('bar'), 42);   // pass!

    // reference values as before
    grunt.option('no-foo');

    grunt.initConfig({});
    grunt.registerTask('default', []);
};
```

## Known issues

### `grunt --help` doesn't show my options

This plugin is a workaround for a [to-do](http://gruntjs.com/creating-tasks#cli-options-environment) on the Grunt team's list. While it allows you to use the api as expected, options will not be displayed when using `grunt --help`. If you'd like to see this sort of functionality natively supported, go add your vote to my related [Github issue](https://github.com/gruntjs/grunt/issues/1005).

## License

MIT
