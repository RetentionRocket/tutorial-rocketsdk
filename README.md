
# Tutorial: Using RocketSDK for Retention Marketing

The tutorial [Retention Marketing for Ecommerce](https://retentionrocket.github.io/tutorial-rocketsdk/) shows how to increase sales for an ecommerce site by adding the [Retention Rocket](https://www.retentionrocket.com/) RocketSDK JavaScript library. Merchants use the Retention Rocket service to reduce cart abandonment. The JavaScript library sends ecommerce event data to the Retention Rocket servers. Retention Rocket responds to events such as cart abandonment by sending SMS or Facebook messages to shoppers.

This repository contains the *source/index.html.md* file for the tutorial content as well as the [Slate](https://github.com/lord/slate) document generator.

## Prerequisites

To edit and publish the tutorial, you'll need:

 - Linux or OS X (Windows may work, but is unsupported)
 - Ruby (version 2.3.1 or newer)
 - Bundler

If Ruby is already installed, but the `bundle` command doesn't work, just run `gem install bundler` in a terminal.

## Download the Project

Copy the project files to your local environment using `git clone`.

```bash
$ git clone https://github.com/RetentionRocket/tutorial-rocketsdk
$ cd tutorial-rocketsdk
```

You're in the tutorial project folder.

## View

Run the [Middleman](https://middlemanapp.com/) web server to view the tutorial in a web browser at [http://localhost:4567](http://localhost:4567).

```bash
$ bundle install
$ bundle exec middleman server
```

## Edit

Edit the file *source/index.html.md* to revise the tutorial. Use [Markdown](https://en.wikipedia.org/wiki/Markdown) formatting.

## Publish

Save your file, commit with git and push the changes to Github, and deploy it as [GitHub pages](https://pages.github.com/). See [Deploying Slate](https://github.com/lord/slate/wiki/Deploying-Slate) for details.

```bash
$ ./deploy.sh
```

Deploying the tutorial makes it available publicly at [https://retentionrocket.github.io/tutorial-rocketsdk/](https://retentionrocket.github.io/tutorial-rocketsdk/).

## Questions? Need Help? Found a bug?

Suggestions for the tutorial? Found an error? [Submit an issue](https://github.com/RetentionRocket/tutorial-rocketsdk/issues).
