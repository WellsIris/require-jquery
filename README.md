# RequireJS + jQuery

This project shows how [jQuery](http://jquery.com) can be used with [RequireJS](http://requirejs.org). It includes a sample project that you can use as a template to get started.

See the [Use with jQuery](http://requirejs.org/docs/jquery.html) page on the RequireJS site for more background on the sample project. The RequireJS site also has a link to [download this sample project](http://requirejs.org/docs/download.html#samplejquery).

The sample project uses a require-jquery.js file which is a combination of three files:

* RequireJS, version 0.24.0
* jQuery, version 1.5.2
* [post.js](require-jquery/blob/master/parts/post.js), which just registers jQuery as a module.

This project will be kept relatively up to date with the latest jQuery and RequireJS files as they are released.

## Alternate Integration

If you do not want to bundle RequireJS with jQuery, you can load jQuery separately, not as part of the same file as RequireJS, but it has some implications when using the [RequireJS optimizer](http://requirejs.org/docs/optimization.html). See **Optimization Considerations** below.

First, an explanation on what to change in the sample project:

### app.html

* Change the script tag to load just **require.js** ([download it](http://requirejs.org/docs/download.html#requirejs) from the RequireJS site) instead of require-jquery.js
* Remove the data-main attribute
* Load the main.js file with a require() call in the HTML file.
* Specify a [priority configuration option](http://requirejs.org/docs/api.html#config). This tells RequireJS to download jQuery before tracing any other script dependencies:

    <!DOCTYPE html>
    <html>
        <head>
            <title>jQuery+RequireJS Sample Page</title>
            <script src="scripts/require.js"></script>
            <script>
            require({
                baseUrl: 'scripts',
                priority: ['jquery']
            }, ['main']);
            </script>
        </head>
        <body>
            <h1>jQuery+RequireJS Sample Page</h1>
            <p>Look at source or inspect the DOM to see how it works.</p>
        </body>
    </html>

The above example assumes that you downloaded jQuery and placed it in the project
as **webapp/scripts/jquery.js**. If you wanted to load jQuery from a CDN, like Google's you
could do this:

    <!DOCTYPE html>
    <html>
        <head>
            <title>jQuery+RequireJS Sample Page</title>
            <script src="scripts/require.js"></script>
            <script>
            require({
                baseUrl: 'scripts',
                paths: {
                    jquery: 'https://ajax.googleapis.com/ajax/libs/jquery/1.6.0/jquery.min'
                },
                priority: ['jquery']
            }, ['main']);
            </script>
        </head>
        <body>
            <h1>jQuery+RequireJS Sample Page</h1>
            <p>Look at source or inspect the DOM to see how it works.</p>
        </body>
    </html>

However, you will want to download a local copy of jQuery and place it in the
project at **webapp/scripts/jquery.js** so it can be used with the optimizer.

### Optimization Considerations

With jQuery loaded externally, before main.js is loaded, jQuery now needs to be
excluded from the built file the optimizer generates.

Make sure you have a jquery.js in the **webapp/scripts** directory. Then change
the app.build.js file to the following:

    ({
        appDir: "../",
        baseUrl: "scripts",
        dir: "../../webapp-build",
        //Comment out the optimize line if you want
        //the code minified by UglifyJS
        optimize: "none",

        modules: [
            {
                name: "main",
                exclude: ["jquery"]
            }
        ]
    })

This will bundle all the scripts into the built main.js file, except for jQuery.
