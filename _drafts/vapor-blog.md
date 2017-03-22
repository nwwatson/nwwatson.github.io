---
layout: post
title:  "Beginning Vapor: Lets Write a Blog"
date:   2017-03-14 23:25:00
categories: swift vapor postgresql
comments: true
description: >
  Over 10 years ago, Ruby on Rails made a huge impact on me when I saw the
  "Blog in 15 Minutes" tutorial. I'd like to do the same for Vapor, a server
  side Swift framework.
---

Over 10 years ago, Ruby on Rails made a huge impact on me when I saw the
"Blog in 15 Minutes" tutorial. The simple tutorial, in which a simple blog
application was scaffolded show all that you needed to know in order to get
started using Rails. The "Blog in 15 Minutes" tutorial is the gold standard for
introducing a framework.

When writing a tutorial, many authors focus on showing would-be developers all the
features of the framework/language, but completely miss explain how to structure
a cohesive application. Many tutorials show an application thats built in one file,
or if they do create separate files, they create "buckets full of method" files
which do not show new users how to break applications in to smaller components.

My goal with this tutorial is to build a small blog built using Vapor, a server
side framework for Swift. I'd like to cover the following topics:

* Setup of Vapor on MacOS Sierra
* The creation of a Vapor projects
* Connecting the Vapor project to a database
* Building a controller that will handle the CRUD methods for creating posts
* Building Leaf templates to build the user interface.

<!--more-->

In the future, I may add additional posts which will add user authentication,
move the creation of blog posts in to an administrative area, and possibly add
tags to a post. With that, lets get started with our first Vapor project.

## Installation

I'm going to be doing this tutorial on MacOS Sierra. To follow along, install
XCode from the Apple App Store. If you wish to do development on an Ubuntu box,
[follow the installation instructions provided on Vapor's site](https://vapor.github.io/documentation/getting-started/install-swift-3-ubuntu.html).

The Vapor project team provides the Vapor Toolbox which helps you to quickly
scaffold a Vapor project. You can install it by opening up terminal and running
the following command:

```shell
curl -sL check.vapor.sh | bash
```

After installation, test that everything works.

```shell
vapor --help
```

# Creating Our First Project

We're going to use the Vapor Toolbox to create our first project. By default, Vapor
scaffolds an application with an example model and controller created. For the
purpose of this tutorial, I'd like to create these manually, so we're going to
specify a custom template I created.

```shell
vapor new VaporBlog --template=nwwatson/vapor-starter
```

This will scaffold a default project structure. Don't worry much about it right
now. We'll dive more in to that shortly. The first thing we want to do is build
and run the project to ensure everything works. To build the project, run the
following command in terminal:

```shell
vapor build
```

Once the project is built, lets run it to ensure everything works correctly.

```shell
vapor run serve
```

At this point, you should be able to open the web address [http://localhost:8080](http://localhost:8080) in your browser and
see our application running.

## Creating an XCode Project

We are going to use XCode for this tutorial. While you can develop Swift
applications using other tools, such as Atom, XCode provides an integrated
environment that will greatly help for development and trouble shooting. To
generate the XCode project, use the following shell command.

```shell
vapor xcode
```
The console will ask if you would like to open the XCode project. Type a 'Y'
and hit return, the XCode project should open for you.


## Setting Up A Database

For this tutorial, we're going to use SQLite for data storage This provides a
simple way to develop an application without having setup MySQL or PostgeSQL
connection strings. SQLite should be installed by default if you're using MacOS,
if not, you can install it using Homebrew.

```shell
brew install sqlite3
```

Once installed, we'll need to setup our vapor project to use SQLite. The first
thing we have to do is modify our Package.swift file to include the SQLite
provider for Vapor.

```swift
import PackageDescription

let package = Package(
  name: "VaporBlog",
  dependencies: [
    .Package(url: "https://github.com/vapor/vapor.git", majorVersion: 1, minor: 5),
    .Package(url: "https://github.com/vapor/sqlite-provider.git", majorVersion: 1, minor: 1)
  ],
  exclude: [
    "Config",
    "Database",
    "Localization",
    "Public",
    "Resources",
  ]
)
```

Once you have made the change to Package.swift, you'll need to build and regenerate
the XCode project in order to get the SQLite 3 dependencies as a part of your project.

```shell
vapor clean && vapor xcode
```

Once XCode is reopened, we will need to add configuration to tell Vapor where to
place our SQLite database. Lets create a sqlite.json file in the Config directory
of the project.

```json
{
  "path": "/db/database.sqlite3"
}
```

The path specified in this file is set relatively from the Vapor project's
working directory. We want to ensure that the sqlite file does not get checked
in to source control (if you're using one), so we're going to add it to our
.gitignore file.

```
Packages
.build
xcuserdata
*.xcodeproj
Config/secrets
/db/*.sqlite3
```

Finally, we will write some code to connect to the SQLite3 database and display
the version to ensure we have everything setup correctly. First, we'll import
the SQLite3 provider in to Main.swift

```swift
import VaporSQLite
```

Next, we're going to add provider to the Vapor Droplet for SQLite.

```swift
try drop.addProvider(VaporSQLite.Provider.self)
```

Finally, we're going to modify our base route to query for the version of
SQLite and pass along data to our view

```swift
drop.get { req in
  let result = try drop.database?.driver.raw("SELECT sqlite_version()")
  let row = result?.array?.first?.object
  let version = row?["sqlite_version()"]?.string

  return try drop.view.make("welcome", [
    "sqlite-version": (version?.makeNode())!,
    "app-name": drop.localization[req.lang, "application", "name"],
    "app-description": drop.localization[req.lang, "application", "description"],
    "app-author": drop.localization[req.lang, "application", "author"]
  ])
}
```

When all is said and done, your Main.swift file should look like this.

```swift
import Vapor
import VaporSQLite

let drop = Droplet()
try drop.addProvider(VaporSQLite.Provider.self)

drop.get { req in
  let result = try drop.database?.driver.raw("SELECT sqlite_version()")
  let row = result?.array?.first?.object
  let version = row?["sqlite_version()"]?.string

  return try drop.view.make("welcome", [
    "sqlite-version": (version?.makeNode())!,
    "app-name": drop.localization[req.lang, "application", "name"],
    "app-description": drop.localization[req.lang, "application", "description"],
    "app-author": drop.localization[req.lang, "application", "author"]
  ])
}

drop.run()
```

Now to test things out. Run the application using XCode and visit (http://localhost:8080](http://localhost:8080) in your browser.


## Creating Our Post Model

Now that SQLite is setup, lets build a model to represent our blog post. The
Model will use Vapor's object relational mapping library Fluent. Fluent allows
developers to interact with the database without having to write SQL code.

```shell
touch Sources/App/Models/Post.swift
vapor xcode
```

```swift
import Vapor

final class Post: Model {

  var id: Node?
  var exists: Bool = false

  var title: String
  var body: String
  //var postedOn: Double

  init(title: String, body: String) {
    self.id = nil
    self.title = title
    self.body = body
  }

  init(node: Node, in context: Context) throws {
    id = try node.extract("id")
    title = try node.extract("title")
    body = try node.extract("body")
  }

  func makeNode(context: Context) throws -> Node {
    return try Node(node:[
      "id": id,
      "title": title,
      "body": body
    ])
  }

  static func prepare(_ database: Database) throws {
    try database.create("posts") { posts in
      posts.id()
      posts.string("title")
      posts.string("body")
    }
  }

  static func revert(_ database: Database) throws {
    try database.delete("posts")
  }

}
```

## Creating a Controller

```shell
touch Sources/App/Controllers/PostsController.swift
vapor xcode
```
