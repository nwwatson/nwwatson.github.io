---
layout: post
title:  "Setting Up the Play Framework"
date:   2014-10-30 11:20:00
categories: java play
---

In this post, I'm going to describe the setup of the Play famework on OS X Yosemitee. If you are using an older version of OS X, the steps will be similar. Not much has changed that would effect the overall use of the Play Framework.

I used (Homebrew)[http://brew.sh] to manage development software packages on OS X, and I'd recommend that you do the same. I'm going to assume you have the following items currently installed on your machine:

* Homebrew
* Java 8 SDK
* Eclipse IDE

Thankfully, installing Play OS X is easy when using homebew. If you already have Homebrew installed, run the following commands from the command line:

{% highlight shell %}
brew update
brew install play
{% endhighlight %}

Once installed its time to create our first application. We're going to create an application to track the locations we have been. Lets start by creating the Play application

{% highlight shell %}
activator new locations
{% endhighlight %}

The activator keyword is used to create structure of the Play appliation. The structure looks something like this:

|-- app
    |-- controllers
    |   - Appliaction.java
    |-- views
    |   - index.scala.html
    |   - main.scala.html
|-- conf
    - application.conf
    - routes
|-- projects
    - build.properties
    - plugins.sbt
|-- public
    |-- images
        - favicon.png
    |-- javascripts
        - hello.js
    |-- main.css
|-- test
    - ApplicationTest.java
    - IntegrationTest.java
- .gitignore
- activator
- activator-launch-1.2.3.jar
- build.sbt
- LICENSE
- README

## Application Structure

Lets take some time to familiarize ourselves with the Play folder structure. If you are familiar with Ruby on Rails, much of this will look familiar to you. We will walk through each directory so that you understand where various project files are found.

### App Folder

The "app" folder contains all the application code for your application. This includes java code, view templates, and assets that need compiled. By default, Play doesn't create a directory to store assets, such as JavaScript, CoffeeScript or Less that needs compiled, you have to create this on your own.

The "controllers" directory contains all the controllers that you application will use. Controllers are used by the play framework to respond to web requests from your appliation.

The "views" folder contains the views your appliation will return to users. The views are written using Scala template syntax. This syntax is similar to other templating syntaxes such as ERB, Handlebars, etc. If you are familiar with a templating language, you'll have little problem quickly picking up Scala templating syntax.

If you would like to use asset compilation, then you will need to create an app/assets directory. Play has the ability to preprocess assets before the are served to browsers. We will go more in depth on assets and asset compilation in a future article.

### Conf Directory

The "conf" directory contains all configuration for the application. By default, the directory contains two files, application.conf and routes. Application.conf file contains configuration information such as database connection information. The routes file contains mappings for HTTP URls to controllers.

### Public Directory

The "public" directory contains items that need to be served by the web server, but not compiled. For example, if your application uses images, you would place the images in the public folder.

### Test Directory

The "test" directory contains all unit tests for the application.

## Running the Base Application

Now that we have a good understanding how how the application is structured, lets start it up. Play comes with a development console that allows you to perform a variety of valuable tasks. To start the console ensure that you are in the "locations" directory and type the following command

{% highlight shell %}
activator
{% endhighlight %}

Once the conosle is started, you can view the commands available within the console by using the help command

{% highlight shell %}
help
{% endhighlight %}

Running our newly created application is simple. Simply use the run command in the console

{% highlight shell %}
run
{% endhighlight %}

The run command will launch a web server on port 9000. You can view the application in the browser by visiting the following address (http://localhost:9000)[http://localhost:9000]. Play has an auto-reload feature that will reload the environment each time a file is changed. This will simply the development process as you won't have to start and restart a web container.

## Seeting up Play to Use Your IDE

I'm using eclipse to develop this application. Play provides you a command to automatically setup the project files you need to develop in various IDEs such as Eclipse, IntelliJ and Netbeans. Since I'm using Eclipse, I'm going to use the eclipse command in the Play console.

{% highlight shell %}
eclipse
{% endhighlight %}

Once the eclipse command has finished, you need to import the application in to Eclipse. Click File > Import > General > Existing Project into Workspace. Select the root directory and browse to the directory which contains our Play appliaction. Click Finish.

To get debbuging to work within eclipse, you need to run the application with the jvm-debug option. Ensure you are not in the activator console.

{% highlight shell %}
activator -jvm-debug 9999 run
{% endhighlight %}

Once running, right click on the project in Eclipse and select Debug As > Debug Configurations. Select Remote Java Application on the left hand pane, and click the New button on the top left corner of the left hand pane. In the dialog that opens, change the port to 9999 then click Apply. From now on, you can click the Debug button to connect to the running application.

## Conclusion

In this tutorial we created a new application, discussed its structure, fired it up and setup our IDE for development. In a future installement, we'll start writing the code.
