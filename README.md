# grunt-requiregen

## Contents

* [What is grunt-requiregen?](#what-is-grunt-requiregen)
* [Installation](#installation)
* [Usage](#usage)
* [Bug Tracker](#bug-tracker)
* [Author](#author)
* [License](#license)

## What is grunt-requiregen?

A grunt plugin which generates a RequireJS main.js file, which ensures js files are loaded in the right order

Require.js is a nice tool which combines plugin loading capabilities and main application minification for prod.
Maintaining dependencies between all of your modules is a bit cumbersome, especially if you are using AngularJS and its dependency injection system. This would require declaring your dependencies twice.

AngularJS still does not resolve everything as it needs to be loaded first, and usually, you need a bootstrap code to be loaded last. There might be other needs, like jQuery loaded first, app module before its services, etc.


## Installation

```bash
$ npm install grunt-requiregen
```

## Usage

Include the following line in your Grunt file. (Here is CoffeeScript version)

```CoffeeScript
    grunt.loadNpmTasks('grunt-requiregen')
```

```CoffeeScript
    # task that generates main.js (the RequireJS main file)
    # AngularJS is actually very nice with its dependency injection system
    # basically, we just need AngularJS to be loaded first, app second, and run last
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
                # if a module has been loaded in previous layers, it won't be loaded again
                # so that you can use a more generic minimatch expression in the end
                    'libs/jquery'
                    'libs/angular' # AngularJS needs jQuery before or will use internal jqLite
                    'libs/*'       # remaining libs before app
                    'app'          # app needs libs
                    '{routes,views,config,*/**}'  # remaining AngularJS components
                    'run'          # run has to be in the end, because it is triggering angular's own DI
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
