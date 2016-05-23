{{#template name="tutorials.whatsapp.ionic.step_01.md"}}

In this tutorial we will write our app using `ecmascript6`, which is the latest version of javascript updated with the new ECMA standards. (From now on we will refer it as 'es6'). So before we dive into building our app, we need to do some preliminary setup inorder to achieve that.

In order to write some es6 code we will need a pre-processor. [babel](https://babeljs.io/) plays that role perfectly, but that's not all we need. One of the most powerful tools in es6 is the module system, which uses relative paths to load the different modules we implement. This is something `babel` can't do, because as a syntax compiler, it can't load relative modules. Therefore, we will need some sort of a module bundler.

That's where [Webpack](https://webpack.github.io/) kicks in. `Webpack` is a very powerful, commonly-used module bundler; but, it can also use pre-processors and is easily configured using whatever rules we specify.

`Meteor` also uses the same techniques to implement es6 and load `npm` modules into client side code, but since we're using the `Ionic` cli and not `Meteor`, we will implement our own `Webpack` configuration, using our own rules.

Great, now that we have an idea what `Webpack` is all about, let's setup our initial config:

{{> DiffBox tutorialName="ionic-tutorial" step="1.1"}}

> *NOTE*: Since we don't want to digress from this tutorial's subject, we won't go into details about `Webpack`'s config. For more information, see [reference](https://webpack.github.io/docs/configuration.html).

We would also like to initiate `Webpack` when building our app. All our tasks will be defined in a single file called `gulpfile.js`, which uses [gulp](http://gulpjs.com/)'s API to perform and chain them.

Let's edit our `gulpfile.js` as shown:

{{> DiffBox tutorialName="ionic-tutorial" step="1.2"}}

From now on all our client code will be written in the `./src` folder. `Gulp` should automatically detect changes in our files and re-build them once the app is running.

> *NOTE*: Again, we would like to focus on building our app rather than explaining 3rd party libraries. For more information about tasks in `Gulp` see [reference](https://github.com/gulpjs/gulp/blob/master/docs/API.md).

Last but not least, let's install the necessary dependencies in order to make our setup work. Run:

    $ npm install babel --save
    $ npm install babel-core --save
    $ npm install babel-loader --save
    $ npm install babel-plugin-add-module-exports --save
    $ npm install babel-preset-es2015 --save
    $ npm install babel-preset-stage-0 --save
    $ npm install expose-loader --save
    $ npm install lodash.camelcase --save
    $ npm install lodash.upperfirst --save
    $ npm install script-loader --save
    $ npm install webpack --save

> *TIP*: You can also do this on a single line using `npm i <package1> <package2> ... --save`.

Our `package.json` should look like this:

{{> DiffBox tutorialName="ionic-tutorial" step="1.3"}}

`Ionic` provides us with a very nice skelton for building our app; but, we would like to use a different method which is a little more advanced and which will help us write es6 code properly.

Thus, we shall clean up some of our profect files. Just run:

    $ cd ./www
    $ rm -rf ./css
    $ rm -rf ./img
    $ rm -rf ./js
    $ rm -rf ./templates

Next, we will setup our `index.html`:

{{> DiffBox tutorialName="ionic-tutorial" step="1.5"}}

- We named our app `Whatsapp`, since that's what it represents.
- We removed all css files except one, since they are all pre-processed and imported into one file called `ionic.app.css` using a library called [SASS](http://sass-lang.com/). All our scss files should be defined in `scss` folder.
- Same goes for javascript files; they will all be bundled into one file called `app.bundle.js` using our previously-defined `Webpack` config.
- We removed the `ng-app` attribute, which will then take place in our javascript code.

Now that we have an initial setup, let's define the entry point for our code. Create a file called `index.js` in `src` with the following contents:

{{> DiffBox tutorialName="ionic-tutorial" step="1.6"}}

This is simply a file where all our desired scripts are loaded. Note that libraries are being loaded with the `script!` pre-fix, which is braught to us by the `script-loader` npm package. This pre-fix is called a loader, and we actually have many types of loaders. This one tells `Webpack` that the files specified afterwards should be loaded as-is, without applying any module requirements or pre-processors.

> *NOTE*: We could also have specified the script loader as a general rule for all our libraries, but then it wouldn't be clear that the imported files were being treated as scripts. Either approach is fine, but in the interested of being more declarative, we will stick with the direct and simple approach of specifying the script loader for every library module.

As you can see there is also an `app.js` file being imported at the bottom. This is our main app file. Let's write it now:

{{> DiffBox tutorialName="ionic-tutorial" step="1.7"}}

As you can see, we define our app's module and bootstrap it. The bootstrapping code is where we initialize our app's primary logic, and is called automatically by `Angular`. Of course, there is some additional code related to the `cordova` enviroment, like hiding the keyboard on startup.

We'de now like to build our app and watch for file changes as it runs. To do so, just edit the `ionic.project` file and add `Gulp` files to run on startup.

{{> DiffBox tutorialName="ionic-tutorial" step="1.8"}}

Also, since we use pre-processors for both our `.js` and `.css` files, they are no longer relevant. Let's make sure they aren't included in future commits by adding them to the `.gitignore` file:

{{> DiffBox tutorialName="ionic-tutorial" step="1.9"}}

{{/template}}
