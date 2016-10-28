
# Chapter 1: ClojureBridge Track 1 

## Overview
This curriculum will introduce you to Clojure by creating a web app called Chatter. We will teach you to use the command line, make code changes in LightTable, and push those changes to GitHub so you can share your app with the world. 

Each chapter below will work through different concepts of coding and provide Clojure-specific instructions to help you create your app. Review this page, then work your way through each chapter in the table of contents. 

## The Chatter App Goals & Tools

### Specific Goal of Workshop

Create a web app using Clojure code.

We will introduce you to Clojure by creating and deploying a web app where you and your friends can post messages to each other.

![](https://github.com/annieengmark/track1-chatter/blob/master/images/finished%20app.png "finished app")

### Additional Benefits & Learning Opportunities

* Understand HTTP and web basics
* Basic Clojure syntax
* REPL (Read Eval Print Loop)
* Leiningen for project automation
* Git for version control (branches, merges, commits)
* Heroku deployment

### <a name="required_tools">Required Tools</a>

* Java 1.7+
* Leiningen 2.5+
* Git 2.1+
* Firefox with Firebug
* The Heroku Toolbelt

_These should have all been installed at the Friday night Install Fest_


## Readme
This is the standard structure of most coding projects. You will have a Readme file that will provide a basic overview of the project and the programs you need to open the project. 

### App

In this curriculum we will create a web app, the Chatter app, that displays posted messages. 

### Prerequisites

You will need [Leiningen][] 2.0.0 or above installed. 


[leiningen]: https://github.com/technomancy/leiningen

### Running The Server & Entering Commands in the terminal

#### Server
Confirm your server is running properly. To start a web server for the application, enter the following command in the terminal:

    lein --version

This will confirm what version of lein and what version of java you have on your machine. 

#### Entering Commands

Confirm you have all the required software/apps installed prior to starting your project. The [required tools](#required_tools) were installed during _Install Fest_ on Friday night.

We will show examples of commands in terminal (also called the command line). Our prompt is the dollar sign; everything _after_ the dollar sign is the command you enter. You do not need to type `$:` in your terminal. We may we enter multiple commands in the same snippet &mdash; the dollar sign indicates a new command to enter. You should enter each command individually.

The terminal, or command line, you use may have different prompts (`$:`), only enter the text after the prompt.

Open your terminal and enter the commands below to confirm you have the correct tools installed. 

	$: lein -version
    Leiningen 2.5.1 on Java 1.8.0_40-internal OpenJDK 64-Bit Server VM

    $: git --version
    git version 2.1.4

    $: heroku --version
    heroku-toolbelt/3.25.0 (x86_64-linux-gnu) ruby/2.1.5
    You have no installed plugins.

Only enter the first line in the command line; the following lines are the results of your command and indicate the versions of software you have installed on your computer.

If your commands and results look like the example above, you are ready to move on and start working on your Chatter app.



## Moving Forward
Once you confirm your server is working properly, move to the next page where were will briefly go over what Clojure is, what the Web is and how we interact with it, and how HTML works.

Next page, [Chapter 2: Big Picture](Page%202_Big%20Picture.md)


### Copyright and License

Copyright © 2015 Chris Koehnen

Licensed under the [MIT LICENSE](http://opensource.org/licenses/MIT)
