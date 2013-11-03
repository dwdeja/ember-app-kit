---
layout: default
title: "Getting Started"
permalink: getting-started.html
---

While Ember App Kit is powerful and easy once you get the hang of it, getting started with it isn't the most intuitive process for most developers. This guide will ease you into developing your application using EAK.

### Installing

The easiest way to create a new project with Ember App Kit is to simply [download it as a zip](https://github.com/stefanpenner/ember-app-kit/archive/master.zip). You can also `git clone` the repo, though you'll want to `rm -r .git` to remove its Git history.

Once you have the template, you'll need to install its dependencies. Ember App Kit's primary build tool is [Grunt](http://gruntjs.com), a build tool written in Node.js. If you don't already have Node installed, you can get it from [nodejs.org](http://nodejs.org/) or your package manager of choice (including [Homebrew](http://brew.sh/) on OSX).

Once you've installed Node, you'll need to install the Grunt command-line tool globally with:

{% highlight sh %}
npm install -g grunt-cli
{% endhighlight %}

This will give you access to the `grunt` command-line runner.

Next, in the folder for your new project, run:

{% highlight sh %}
npm install
{% endhighlight %}

This will install the dependencies Grunt relies on to build. These dependencies are primarily various Grunt tasks that do everything from module compilation to test running.

Before running your app for the first time, you'll want to install [Bower](http://bower.io), a package manager that keeps your front-end dependencies (including JQuery, Ember, and QUnit) up to date. This is as easy as running:

{% highlight sh %}
npm install -g bower
bower install
{% endhighlight %}

For more information on Bower, see the guide on [managing dependencies](dependencies.html)

Once your dependencies are installed, you should be able to simply run:

{% highlight sh %}
grunt server
{% endhighlight %}

and navigate to [http://0.0.0.0:8000](http://0.0.0.0:8000) to see your new app in action.

### Development using Ember App Kit

#### Folder Layout

Ember App Kit comes with lots of boilerplate, which can be daunting to navigate at first. Here's the layout of the application, which we'll explore in further detail throughout the guides.

<table markdown="1">
  <thead>
    <tr>
      <th>File/Folder</th>
      <th>Purpose</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>app/</td>
      <td>Contains your Ember application's code. Javascript files in this folder are *compiled* through the ES6 module transpiler and concatenated into a file called `app.js`. See the table below for more details.</td>
    </tr>
    <tr>
      <td>dist/</td>
      <td>Contains the *distributable* (that is, optimized and self-contained) output of your application. Deploy this to your server!</td>
    </tr>
    <tr>
      <td>public/</td>
      <td>This folder will be copied verbatim (excepting index.html) into the root of your built application. Use this for assets that don't have a build step, such as images or fonts.</td>
    </tr>
    <tr>
      <td>public/index.html</td>
      <td>The only actual page of your single-page app! Includes dependencies and kickstarts your Ember application.</td>
    </tr>
    <tr>
      <td>tasks/</td>
      <td>Contains custom Grunt tasks and helpers used in the build process.</td>
    </tr>
    <tr>
      <td>tasks/options/</td>
      <td>Contains configuration for the external Grunt tasks used for everything from module compilation to serving up your application.</td>
    </tr>
    <tr>
      <td>tests/</td>
      <td>Includes unit and integration tests for your app, as well as various helpers to load and run your tests.</td>
    </tr>
    <tr>
      <td>tmp/</td>
      <td>Various temporary output of build steps, as well as the debug output of your application (`tmp/public`).</td>
    </tr>
    <tr>
      <td>vendor/</td>
      <td>Your dependencies, both those included with EAK and those installed with Bower.</td>
    </tr>
    <tr>
      <td>.jshintrc</td>
      <td>[JSHint](http://jshint.com/) configuration.</td>
    </tr>
    <tr>
      <td>.travis.yml</td>
      <td>[Travis CI](https://travis-ci.org/) configuration. See [Testing with Karma](testing.html).</td>
    </tr>
    <tr>
      <td>Gruntfile.js</td>
      <td>Defines the various multitasks EAK uses to build. See [Asset Compilation](asset-compilation.html).</td>
    </tr>
    <tr>
      <td>bower.json</td>
      <td>Bower configuration and dependency list. See [Managing Dependencies](dependencies.html).</td>
    </tr>
    <tr>
      <td>karma.conf.js</td>
      <td>Karma configuration. See [Testing with Karma](testing.html).</td>
    </tr>
    <tr>
      <td>package.json</td>
      <td>NPM configuration. Mainly used to list the dependencies needed for asset compilation.</td>
    </tr>
  </tbody>
</table>

##### Layout within app/
<table markdown="1">
  <thead>
    <tr>
      <th>Folder/File</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>app/app.js</td>
      <td>Your application's entry point. This is the module that is first executed.</td>
    </tr>
    <tr>
      <td>app/router.js</td>
      <td>Your route configuration. The routes defined here correspond to routes in `app/routes/`.</td>
    </tr>
    <tr>
      <td>app/stylesheets/</td>
      <td>Contains your stylesheets, whether SASS, LESS, Stylus, Compass, or plain CSS (though only one type is allowed, see [Asset Compilation](asset-compilation.html). These are all compiled through the into `app.css`.</td>
    </tr>
    <tr>
      <td>app/templates/</td>
      <td>Your Handlebars templates. These are compiled to `templates.js`. The templates are named the same as their filename, minus the extension (i.e. `templates/foo/bar.hbs` -> `foo/bar`).</td>
    </tr>
    <tr>
      <td>app/controllers/, app/models/...</td>
      <td>Modules resolved by the Ember App Kit resolver. See [Using Modules &amp; the Resolver](using-modules.html).</td>
    </tr>
  </tbody>
</table>

#### Using Grunt

The development workflow for EAK is centered around Grunt, the build tool mentioned above. Grunt is simply a *task runner*, that is, it runs various tasks to handle your build pipeline. Unlike a build tool like Rake, which is usually used to write custom tasks configured for an application, Grunt primarily uses generic tasks that are configured through simple, generic JSON configuration.

If you'd like to peek into the innards of Ember App Kit's build pipeline, you can pop open the `Gruntfile.js` to see the exact order of execution in each task, along with the individual task configuration in the `tasks/options` folder. To get started, though, you only need to know a few easy commands:

* `grunt` - The default command builds your application (in *debug* mode) and runs its tests.

* `grunt server` - As you saw above, this command builds your application (in *debug* mode) and serves it. This task also will *watch* your application for changes, and will rebuild any time you change a file.

* `grunt test:server` - Same as above, but also runs all tests as files change.

* `grunt dist` - Builds your application once in *dist* mode. This means your assets will be minified and version-stamped. This task also builds to the `dist/` folder, which can be deployed to a static server in production.

* `grunt server:dist` - Same as above, but also launches a preview server for your optimized output.

#### Testing your application

#### Writing modules

### Next Steps

*Special thanks to the [Rails Getting Started guide](http://guides.rubyonrails.org/getting_started.html) for inspiration, as well as blog posts by [Matthew Beale](http://blog.safaribooksonline.com/2013/09/18/ember-app-kit/) and [Taras Mankovski](http://embersherpa.com/articles/introduction-to-ember-app-kit/).*