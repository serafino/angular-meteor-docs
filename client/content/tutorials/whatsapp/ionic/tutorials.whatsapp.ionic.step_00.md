{{#template name="tutorials.whatsapp.ionic.step_00.md"}}

Both `Meteor` and `Ionic` took their platform to the next level in tooling.
Both provide CLI interface instead of bringing together a bunch of dependencies and build configuration tools.
There are a few differences between Meteor and Ionic; this post will focus on the `Ionic` CLI.

To start, let’s install `Ionic` with Npm. On your command line:

    $ npm install -g ionic

Now let’s create a new `Ionic` app with the tabs template:

    $ ionic start whatsapp tabs

Now inside the app’s folder, run:

    $ npm install
    $ bower install

Let’s run this default app on the command line:

    $ ionic serve

to run it inside a browser, or:

    $ ionic emulate

to run it inside a simulator.

It is also recommended to exclude the `libs` dir from git so irrelevant content won't be uploaded to github as we make progress with this tutorial. Just edit the `.gitignore` file like so:

{{> DiffBox tutorialName="ionic-tutorial" step="0.2"}}

{{/template}}
