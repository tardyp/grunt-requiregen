[![build status](https://secure.travis-ci.org/tardyp/grunt-requiregen.png)](http://travis-ci.org/tardyp/grunt-requiregen)
# grunt-requiregen

## Contents

* [What is grunt-requiregen?](#what-is-grunt-requiregen)
* [Installation](#installation)
* [Usage](#usage)
* [Bug Tracker](#bug-tracker)
* [Author](#author)
* [License](#license)

## What is grunt-requiregen?

A grunt plugin which generates a requirejs main.js file, which ensures js files are loaded in the right order

Require.js is a nice tool which combines plugin loading capabilities, and main application minification for prod.
Maintaining dependancies between all your modules is a bit cumbersome, especially if you are using angular.js and its dependancy
injection system. This would require declaring your dependancies twice.

Angularjs still does not resolves everything as it needs to be loaded first, and usually, you need a bootstrap code to be loaded last. There might be other needs, like jquery very first, app module before its services, etc.


## Installation

```bash
$ npm install grunt-requiregen
```

## Usage

Include the following line in your Grunt file. (Here this is coffeescript version)

```coffeescript
    grunt.loadNpmTasks('grunt-requiregen')
```

```coffeecript
    # task that generates main.js (the require.js main file)
    # Angular is actually very nice with its dependancy injection system
    # basically, we just need angular is loaded first,  app second and run last
    # we should not be modifying this config when the app is growing
    requiregen:
        main:
            cwd: './.temp/scripts/'
            # require json2 and html5shiv are loaded conditionally in the html file
            src: ['**/*.js','!libs/require.js', '!libs/json2.js', '!libs/html5shiv-printshiv.js']
            options:
                order: [
                # order is a list of minimatch 'glob' like matching
                # modules, requiregen will generate the correct shim
                # to load modules in this order
                # if a module has been loaded in previous layers, it wont be loaded again
                # so that you can use a more generic minimatch expression in the end
                    'libs/jquery'
                    'libs/angular' # angular needs jquery before or will use internal jqlite
                    'libs/*'      # remaining libs before app
                    'app'          # app needs libs
                    '{routes,views,config,*/**}'  # remaining angularjs components
                    'run'     # run has to be in the end, because it is triggering angular's own DI
                ]
            dest: '.temp/scripts/main.js'
```


## Bug tracker

Have a bug?  Please create an issue here on GitHub!

https://github.com/tardyp/grunt-requiregen/issues

## Author

**Pierre Tardy**

+ http://github.com/tardyp


## License

Copyright 2013 Pierre Tardy

Licensed under the MIT license.
