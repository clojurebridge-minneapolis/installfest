OS X Setup
==========

* [Start a terminal](#starting-a-terminal)
* [Make sure Java is installed](#making-sure-java-is-installed)
* [Get Leiningen installed](#installing-leiningen)
* [Get Heroku installed (includes Git)](#getting-setup-with-heroku)
* [Test installation](#testing-your-setup)

## Starting a terminal

For these instructions, and for much of the class, you will need to have a terminal, or command line, open. This is a text-based interface to talk to your computer, and you can open it by running Terminal.app, which is found under `/Applications/Utilities`. If you have never used the terminal before, you may want to spend some time [reading up on command-line basics](http://blog.teamtreehouse.com/command-line-basics).

Go ahead and open your terminal now. It should look something like this:

![blank terminal](img/os_x/blank_terminal.png)

The prompt (where you will type your commands) may look different: it usually shows the computer name and user name, as well as the folder or directory you are currently in.

For the rest of this setup, I will tell you to run commands in your terminal. When I say that, I mean "type the command into the terminal and press the Return key."

## Making sure Java is installed

Run `java -version` in your terminal.

If Java is installed, you will see something like this in your terminal:

![Java version](img/os_x/java_version.png)

If Java is not installed, or the version number is less than 1.8 you can download it here [Java Downloads](http://www.oracle.com/technetwork/java/javase/downloads/index.html)

![Java download 1](img/os_x/java-download1.png)

Click the Java download button.

![Java download 2](img/os_x/java-download2.png)

Accept the license agreement, and download the version for Mac OS X.

Double-click the jdk-8u111-macosx-x64.dmg file in your Downloads directory and walk through the installer.

![Java download 1](img/os_x/java-installer1.png)

## Installing Leiningen

Leiningen is a tool used on the command line to manage Clojure projects.

To install `lein`, execute the following commands in your terminal. You will be prompted to enter your password.

```bash
curl https://raw.githubusercontent.com/technomancy/leiningen/stable/bin/lein > lein
sudo mkdir -p /usr/local/bin/
sudo mv lein /usr/local/bin/lein
sudo chmod a+x /usr/local/bin/lein
cd $HOME
echo 'PATH=$PATH:/usr/local/bin' >> .bashrc
source .bashrc
```

After you run the above commands, run the `lein version` command. It should take a while to run, as it will download some resources it needs the first time. If it completes successfully, you are golden! If not, ask an instructor for help.

## Getting setup with Heroku

Heroku is the tool we will use in order to put your application online where others can see it.

First, we need to create an account. Go to [Heroku](http://heroku.com) and click the "Sign up" link.

![Heroku step 1](img/heroku-step1.png)

You will be taken to a form where you can create an account. Fill out that form, and you will be sent an email with a link to click to continue the signup process.

![Heroku step 2](img/heroku-step2.png)

After clicking on the link, you will be taken to another form where you will need to choose a password. Choose one and enter it twice.

![Heroku step 3](img/heroku-step3.png)

After all that, go [here](https://devcenter.heroku.com/articles/getting-started-with-clojure#set-up) and click "Download Heroku Toolbelt for Mac OS X".

![Heroku dashboard](img/heroku-getting-started.png)

If you do not see this link, you can download the toolbelt from [toolbelt.heroku.com](https://toolbelt.heroku.com/).

This will download a .pkg file. Double-click it to install the Heroku Toolbelt and follow all prompts from the installation wizard. You will need your OS X account password to complete installation. If you are prompted to create an SSH key, say yes. It may also prompt you to install git. Once the Heroku Toolbelt is installed, go to your terminal and run the command `heroku login`. You will be prompted for your email and password on Heroku. If you enter them and the command ends successfully, congratulations!

![Heroku login](img/os_x/heroku_login.png)

To make your computer automatically communicate securely with Heroku, you will need to set up SSH. First, go to your terminal and run the command `ssh-keygen`. When prompted for inputs, simply press Return until the process has finished.

![Heroku SSH setup](img/os_x/heroku_ssh_setup.png)

Finally, complete your SSH setup with Heroku by running the command `heroku keys:add`.

![Heroku key-add](img/os_x/heroku_key_add.png)

## Testing your setup

You have set up Java, Leiningen, Atom, Git, and Heroku on your computer--all the tools you will need for this course. Before starting, we need to test them out.

Go to your terminal and run the following command:

```
git clone https://github.com/heroku/clojure-sample.git
```

This will check out a sample Clojure application from GitHub, a central repository for lots of source code. Your terminal should look similar to this picture:

![Testing git clone](img/os_x/testing-step1.png)

Then run the command:

```
cd clojure-sample
```

This will put you in the directory with the source code for this sample bit of Clojure code. After that completes, run:

```
lein repl
```

This could take a long time, and will download many other pieces of code it relies on. You should see lines that start with `Retrieving ...` on your screen. When it finishes, your terminal should look like the following:

![Testing lein repl](img/os_x/testing-step2.png)

This is starting a REPL, which we will learn about soon. It's a special terminal for Clojure. At the REPL prompt, type `(+ 1 1)` and press Return. Did you get the answer `2` back? You will learn more about that in the course. For now, press the Control button and D button on your keyboard together (abbreviated as Ctrl+D). This should take you out of the Clojure REPL and back to your normal terminal prompt.

You should still be in the `clojure-sample` directory.

Run this command:

`heroku create`

There should be output about something being created. A URL will be displayed. Look at the following example:

![Testing heroku create](img/os_x/testing-step5.png)

Next, run the following commands:

```
git push heroku master
heroku open
```

Enter "yes" if you are asked if you are sure you want to connect, like in the following image:

![Connecting via SSH](img/os_x/testing-step6.png)

Your browser should open (and take a long time to load), and you should see a website like the following:

![Testing heroku working](img/os_x/testing-step7.png)

If your browser does not open after running `heroku open`, start a browser and go to the URL displayed after you ran `heroku create`.

Congratulations! That website is running code you have on your computer that you have uploaded. You have actually made a very simple Clojure app, and your computer is all set up to make more.

## Try the koans

If you're a track 2 student, try to tackle running the [koans](koans.md).
