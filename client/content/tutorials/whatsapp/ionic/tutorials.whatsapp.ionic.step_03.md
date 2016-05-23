{{#template name="tutorials.whatsapp.ionic.step_03.md"}}

Now that we have the layout and some dummy data, let’s make our app real by creating a Meteor server and connect to it.

First download Meteor from the Meteor site: [https://www.meteor.com/](https://www.meteor.com/)

Next, create a new Meteor server inside our project.

Open the command line in our app’s root folder and type:

    $ meteor create api

We just created a live and ready example Meteor app inside the `api` folder.

As you can see, `Meteor` provides us with an exmaple app. Since none of the files are relevant to our purposes, you can go ahead and delete them:

    $ cd api
    $ rm .gitignore
    $ rm package.json
    $ rm -rf node_modules
    $ rm -rf client
    $ rm -rf server

By now you probably noticed that `Meteor` uses `npm`, just like the our `Ionic` project. Since we don't want our client and server to be seperated and require a duplicate installation for each package we decide to add, we'll need to find a way to make them share resources.

We will acheive that by symbolic linking the `node_modules` dir:

    $ cd api
    $ ln -s ../node_modules

> *NOTE*: Our symbolic link needs to be relative, otherwise it won't work on other machines cloning the project.

Don't forget to reinstall `Meteor`'s node dependencies deleting the `node_modules` dir:

    $ npm install meteor-node-stubs --save

Our `package.json` should look like this:

{{> DiffBox tutorialName="ionic-tutorial" step="3.3"}}

Now we are ready to write some server code.

Let’s define two data collections, one for our `Chats` and one for their `Messages`.

We will define them inside a subdirectory in `api` called `server`, since only code written inside this dir will be bundled for the server by `Meteor`'s build system. We have no control over this behavior, and therefore we can't change the layout. One of `Meteor`'s disadvantages is that it's not configurable, so we will have to fit ourselves into this build strategy.

Let's go ahead and create the `collections.js` file:

{{> DiffBox tutorialName="ionic-tutorial" step="3.4"}}

Now we will update our `webpack.config.js` to handle some server logic:

{{> DiffBox tutorialName="ionic-tutorial" step="3.5"}}

In the file above, we simply added an alias for the `api/server` folder and a custom handler for resolving `Meteor` packages. This gives us the effect of combining client- and server-side code, something that is already a built-in feature of `Meteor`'s CLI. Since we're not using the CLI, we created it ourselves this time.

Now that the server side is connected to the client side, we will also need to watch for server-side code changes and re-build our client code accordingly.

To do so, we will have to update the watched paths in `gulpfile.js`:

{{> DiffBox tutorialName="ionic-tutorial" step="3.6"}}

Let’s bring to bear `Meteor`'s powerful client side tools that will help us easily sync to the `Meteor` server in real time.

Navigate to your project’s root folder using the command line and type:

    $ bower install meteor-client-side --save
    $ bower install angular-meteor --save

Notice that we also installed the `angular-meteor` package. `angular-meteor` will help us bring `Meteor`'s benefits into an `Angular` project.

Our `bower.json` should now look like this:

{{> DiffBox tutorialName="ionic-tutorial" step="3.7"}}

Don't forget to import the packages we've just installed in `index.js`:

{{> DiffBox tutorialName="ionic-tutorial" step="3.8"}}

We will also need to load `anular-meteor` into our app as a module dependency, since that's how `Angular`'s module system works:

{{> DiffBox tutorialName="ionic-tutorial" step="3.9"}}

Now instead of mocking our static data in the controller, we can mock it on the server.

Create a file named `bootstrap.js` inside the `api/server` dir and place the following initialization code inside:

{{> DiffBox tutorialName="ionic-tutorial" step="3.10"}}

The code is pretty easy and self explanatory.

Let’s bind the collections to our `ChatsCtrl`.

We will use `Scope.helpers()`. Each key will be available on the template and will be updated when it changes. Read more about helpers in our [API](http://www.angular-meteor.com/api/helpers).

{{> DiffBox tutorialName="ionic-tutorial" step="3.11"}}

> *NOTE*: These are exactly the same collections as the server's. Adding `meteor-client-side` to our project has created a `Minimongo` on our client side. `Minimongo` is a client side cache with exactly the same API as [Mongo](https://www.mongodb.org/)'s API. `Minimongo` will take care of automatically syncing the data with the server.

> *NOTE*: `meteor-client-side` will try to connect to `localhost:3000` by default. To change it, simply set a global object named `__meteor_runtime_config__` with a property called `DDP_DEFAULT_CONNECTION_URL`, and set whatever server url you'd like to connect to.

> *TIP*: You can have a static, front end app that works with a `Meteor` server. In other words, you can use `Meteor` as the back end to any front end web or mobile app without making any changes to your app structure or build process.

Now our app all all its clients are synced up with the server in real time!

To test it, you can open another browser, or another window in incognito mode, open another client side by side and delete a chat (by swiping the chat to the left or clicking `delete`).

Watch as the chat gets deleted and updated in all the connected client in real time!

{{tutorialImage 'ionic' '3.png' 500}}

{{/template}}
