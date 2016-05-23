{{#template name="tutorials.whatsapp.ionic.step_02.md"}}

Now that we've finished with our initial setup, let's dive into the code for our app.

First, we will need some helpers which will assist us in writing `AngularJS` using es6's class system. We will use the [angular-ecmascript](https://github.com/DAB0mB/angular-ecmascript) npm package for this purpose. Let's install it:

    $ npm install angular-ecmascript --save

`angular-ecmascript` is a utility library which will help us write an `AngularJS` app using the es6 class system. In addition, `angular-ecmascript` provides us with some very handy features, like auto-injection (without the need for any pre-processors like [ng-annotate](https://github.com/olov/ng-annotate)), and setting our controller as the view model any time it is created (See [referene](/api/1.3.11/reactive)). The API shouldn't be too complicated to understand, and we will get more familiar with it as we progress through the tutorial.

> *NOTE*: As of now there is no best pratice for writing `AngularJS` es6 code; this is one method we recommend out of many possible other options.

Now that everything is set, let's create the `RoutesConfig` using the `Config` module-helper:

{{> DiffBox tutorialName="ionic-tutorial" step="2.2"}}

This will be our main app router and is implemented using [angular-ui-router](https://atmospherejs.com/angularui/angular-ui-router). Any time we would like to add some new routes and configure them, this is where we do so.

After defining a helper, we shall always load it in the main app file like this:

{{> DiffBox tutorialName="ionic-tutorial" step="2.3"}}

As you can see, there is currently only one route state defined called `tabs`, which is connected to the `tabs` view. Let's add it:

{{> DiffBox tutorialName="ionic-tutorial" step="2.4"}}

In our app we will have 5 tabs: `Favorites`, `Recents`, `Contacts`, `Chats`, and `Settings`. In this tutorial we will only focus on implementing the `Chats` and the `Settings` tabs, but your'e more than free to continue on and implement the rest of them.

Let's create the `Chats` view which will appear once we click on the `Chats` tab. But first, let's install `Moment`, a utility library for manipulating date objects. It will soon come in handy:

    $ npm install moment --save

Our `package.json` should now look like this:

{{> DiffBox tutorialName="ionic-tutorial" step="2.5"}}

After installing `Moment`, we need to expose it to our environment, since some of the libraries we depend on are not using es6's module system and expect it to be defined as a global variable. For this, we use the `expose-loader`. Simply add to `index.js`:

{{> DiffBox tutorialName="ionic-tutorial" step="2.6"}}

After the `?` comes the variable name which should be defined on the global scope, and after the `!` comes the library we would like to load. In this case we load the `Moment` library and would like to expose it via `window.global`.

> *NOTE*: Although `Moment` is defined on the global scope, we will continue to make things clearer and more declarative by importing it in every module where it is used.

Now that `Moment` is locked and loaded, we will create our `Chats` controller and use it to create some data stubs:

{{> DiffBox tutorialName="ionic-tutorial" step="2.7"}}

And we will load it:

{{> DiffBox tutorialName="ionic-tutorial" step="2.8"}}

> *NOTE*: From now on, we will load every component immediately after creating it, without any further explanations.

The data stubs are just temporary, fabricated samples which will be used to test our application and how it interacts with data. You can also look at our schema to figure out how the application will look.

Now that we have the controller with the data, we need a view to present it. We will:
- use `ion-list` and `ion-item` directives, which provides us a list layout;
- iterate over our static data using `ng-repeat`; and,
- display the chat's name, image and timestamp.

Let's create it:

{{> DiffBox tutorialName="ionic-tutorial" step="2.9"}}

We also need to define the appropriate route state which will be navigated any time we press the `Chats` tab. Let's do so:

{{> DiffBox tutorialName="ionic-tutorial" step="2.10"}}

If you look closely you will see that we used the `controllerAs` syntax, which means that we want our data models stored on the controller itself and not attatched to the $scope.

We also used the `$urlRouterProvider.otherwise()` to define our `Chats` state as the default one, and so that any unrecognized route will automatically redirect us to this state.

As of now, our chats' dates are presented in a very messy format which is not very informative for every-day users. We would rather use a calendar format, and in order to do that, we need to define a `Filter`. Filters are provided by `Angular` and handle the way our data is presented in the view. Let's add the `CalendarFilter`:

{{> DiffBox tutorialName="ionic-tutorial" step="2.11"}}

{{> DiffBox tutorialName="ionic-tutorial" step="2.12"}}

And now let's apply it to the view:

{{> DiffBox tutorialName="ionic-tutorial" step="2.13"}}

As you can see, in order to apply a filter to the view, we simply pipe it next to our data model.

We would also like to be able to remove a chat, so let's add a delete button to each one:

{{> DiffBox tutorialName="ionic-tutorial" step="2.14"}}

And implement its logic in the controller:

{{> DiffBox tutorialName="ionic-tutorial" step="2.15"}}

Now everything is ready, but it looks a bit dull. Let's add some style:

{{> DiffBox tutorialName="ionic-tutorial" step="2.16"}}

Since the stylesheet was written in `SASS`, we need to import it into our main `scss` file:

{{> DiffBox tutorialName="ionic-tutorial" step="2.17"}}

> *NOTE*: From now, we will import every `scss` file immediately after creating it, without any further explanations.

Our `Chats` tab is now ready. You can run it inside a browser, or if you prefer to see it in a mobile layout, you should use `Ionic`'s simulator. Just follow the instructions:

    $ npm install -g ios-sim
    $ cordova platform add i

The API shouldn't be too complicated to understand, and we will get familiar with it as we progress through the tutorial.

{{tutorialImage 'ionic' '1.png' 500}}

And if you swipe a menu item to the left:

{{tutorialImage 'ionic' '2.png' 400}}

{{/template}}
