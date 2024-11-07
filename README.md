# Ruby Gems Guide

[![Build Status](TODO)
[![Documentation](docs/img/badge_documentation_latest.svg)](https://jcouball.github.io/ruby-gems-guide)

This repository contains the source code for the [Ruby Gems
Guide](https://jcouball.github.io/ruby-gems-guide) that describes how to build Ruby
Gems.

* [Contributing to this Guide](#contributing-to-this-guide)
* [Running in VS Code in a Container Volume](#running-in-vs-code-in-a-container-volume)
* [Setup the VS Code Project](#setup-the-vs-code-project)
  * [View the Documentation](#view-the-documentation)
  * [Run the Markdown Linter](#run-the-markdown-linter)
* [Viewing Documentation Without Docker](#viewing-documentation-without-docker)

## Contributing to this Guide

Existing pages maybe be edited in place.

To add a new page, add a new markdown file in [docs](/docs) and then create a
new navigation entry to the `nav:` tree in [mkdocs.yml](mkdocs.yml) which
references the new markdown file.

In either case, make sure the changes do not break the document build and do
not contain linter offenses before creating a pull request.

## Running in VS Code in a Container Volume

This project is configuerd to use the VS Code Remote Containers extension so you
can view documentation changes on your laptop before they are published without
having to install Python, mkdocs, Ruby and other dependencies on your laptop.

## Setup the VS Code Project

1. Install the following products on your laptop:
    * [Docker](https://docs.docker.com/docker-for-mac/install/)
    * [VS Code](https://code.visualstudio.com/download)
    * [The VS Code "Remote Development" extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.vscode-remote-extensionpack)
2. In VS Code, from the command palette run the command
   `Clone Repository in Container Volume`
    * When prompted for a repository URL enter this repository's URL: `https://github.com/jcouball//ruby-gems-guide.git`
    * You will see an error that says "The repository URL
      'https://github.com/jcouball//ruby-gems-guide.git' is not recognized, using
      a different repository name." -- this is fine, press `OK`
    * When prompted to select the volume for the cloned repository, select
      `Create a unique volume`
    * VS Code will build an development image and clone the code into a container
      running that image. This will take a few minutes. This is only done the first
      time you open the project.

When you close this project, the docker container will be reused the next time the
project is opened.

The source code for in this document will be hosted in a docker volume so it won't be
accessible via your computer's file system. Commit and push your changes
often so you don't lose work if your Docker volume is deleted.

### View the Documentation

The `Guide: Document Server` task will run a document server so you can
preview the changes as you make them to the documentation.

1. In the VS Code command pallet, run the command `Tasks: Run Task`
2. Select the task `Guide: Document Server`
    * The task will open a terminal session in VS Code to run "mkdocs serve"
    * If there is some error that prevents mkdocs from running, fix the error
      before continuing
3. When prompted, select `Open Browser` to see the document output. Alternately,
   open [http://127.0.0.1:8000](http://127.0.0.1:8000) to view the documentation.

The page in the browser will update as you make changes to the source code for
that page. Press `Control-C` in the terminal window to exit the document server.

### Run the Markdown Linter

The `Guide: Markdown Linter` task will open a terminal session in VS Code to
run the markdown linter every time there is a change to a markdown files in the
project.

1. In the VS Code command pallet, run the command `Tasks: Run Task`
2. Select the task `Guide: Markdown Linter`

Type `exit` in the terminal window to exit the linter.

## Viewing Documentation Without Docker

* Make sure Python is installed
* Make sure [Poetry](https://python-poetry.org/docs/) is installed
* Install mkdocs and any plugins or theese defined in the
  [pyproject.toml](pyproject.toml) file running:

    ```console
    poetry install
    ```

* Start the document webserver, by running the `mkdocs serve` command in the
  root directory of this repository

    ```console
    poetry run mkdocs serve
    ```

* Connect to the documentation webserver using you're browser at: [http://127.0.0.1:8000](http://127.0.0.1:8000)
* Press CTRL-C to exit the documentation server when done.
