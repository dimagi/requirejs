# How to use RunJS with jQuery

When a project reaches a certain size, managing the script modules for a project starts to get tricky. You need to be sure to sequence the scripts in the right order, and you need to start seriously thinking about combining scripts together into a bundle for deployment, so that only one or a very small number of requests are made to load the scripts.

You may also want to load code on the fly, after page load.

RunJS can help you manage the script modules, load them in the right order, and make it easy to combine the scripts later via the RunJS build tools without needing to change your markup. It also gives you an easy way to load scripts after the page has loaded, allowing you to spread out the download size of your logic over time.

RunJS has a module system that lets you define well-scoped modules, but you do not have to follow that system to get the benefits of dependency management and build tool optimizations. Over time, if you start to create more modular code that needs to be reused in a few places, the module format for RunJS makes it easy to write encapsulated code that can be loaded on the fly. It can grow with you, particularly if you want to incorporate internationalization (i18n) string bundles, to localize your project for different languages, or load some HTML strings and make sure those strings are available before executing code.

## Get run.js

First step is integrating run.js with your code. There are two options:

1. Grab run.js from the RunJS project and drop it in directory with jquery.js
2. Build jQuery integrated with run.js See this patch to the jQuery source:
xxx

**NOTE**: Following the above approaches will get RunJS set up with its core feature set. Later, if you want to use the i18n or text plugins with RunJS or more sophisticated things like loading multiple versions of modules or creating module modifiers, you will need to grab the complete source version of run.js along with the run directory (which contains the i18n.js and text.js files).

## Set up your HTML page.

A sample HTML page would look like this (assuming you put all your .js files in a "scripts" subdirectory):

    <!DOCTYPE html>
    <html>
        <head>
            <title>jQuery+RunJS Sample Page</title>
            <!-- If you built run.js into jquery
                 then you do not need the script tag for run.js here -->
            <script src="scripts/run.js"></script>
            <script src="scripts/jquery.min.js"></script>
            <script>run(["app"]);</script>
        </head>
        <body>
            <h1>jQuery+RunJS Sample Page</h1>
        </body>
    </html>

The call to run(["app"]); tells RunJS to load the scripts/app.js file. RunJS will load any dependency that is passed to run() without a ".js" file from the same directory as run.js. If you feel more comfortable specifying the whole path, you can also do the following:

    <script>run(["scripts/app.js"]);</script>

What is in app.js? Another call to run.js to load all the scripts you need and any init work you want to do for the page that is not in another helper script. This example app.js script loads two plugins, jquery.alpha.js and jquery.beta.js (not the names of real plugins, just an example). The plugins should be in the same directory as jquery.js:

app.js:

    run(["jquery.alpha", "jquery.beta"], function() {
        //the jquery.alpha.js and jquery.beta.js plugins have been loaded.
        $(function() {
            $('body').alpha().beta();
        });
    });

## Feel the need for speed

Now your page is set up to be optimized very easily. Download the RunJS source and place it anywhere you like, preferrably somewhere outside your web development area. Then, in the scripts directory that has jquery.js and app.js, create a file called app.build.js:
