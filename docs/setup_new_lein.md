# Install Leiningen

Go to the...
* prev section [Install Java](setup_new_java.md)
* new [Setup](setup_new.md) instructions
* next section [Getting Started with Heroku](setup_new_heroku2.md)

------------

## Download Leiningen for Windows

For Windows there is a special installer:

http://leiningen-win-installer.djpowell.net/

We want to make sure that the `lein` command is accessible
anywhere so we'll add it to the **PATH** environment variable.

On Windows you can go to **System Properties | Advanced | Environment
Variables**

_NOTE: see these alternate notes on setting the Windows PATH:_
[note 1](https://java.com/en/download/help/path.xml),
[note 2](http://stackoverflow.com/questions/23400030/windows-7-add-path))

![System Properties](img/new/lein2.png)

Next find the **Path** variable:

![Path Variable](img/new/lein4.png)

Now add the directory for lein to the **Path**

Note the previous entry should end with a semicolon (';').
If there isn't already a semicolon add one now.

Then add the path to the Leiningen program `%USERPROFILE%\.lein\bin`

![Add lein to Path](img/new/lein5.png)

## Download Leiningen for Mac or Linux

For Mac and Linux you can download the `lein` script:

````
clojurista@mylaptop $ mkdir ~/bin
clojurista@mylaptop $ cd ~/bin
clojurista@mylaptop $ curl https://raw.githubusercontent.com/technomancy/leiningen/stable/bin/lein
clojurista@mylaptop $ chmod +x lein
clojurista@mylaptop $ cd ..
clojurista@mylaptop $ echo 'export PATH=$PATH:$HOME/bin' > .profile
clojurista@mylaptop $ . .profile
````
## Test Leiningen (all platforms)

Now you can change directories back to your HOME directory
and verify that `lein` is in your path by typing:

    lein version

You should see the version of Leiningen (and Java) print out.

Now you can type `lein repl` to start a simple Clojure REPL.

You can enter a simple addition `(+ 1 2)`.

Finally type `(exit)` to return to the command line prompt:

![Test Leiningen ](img/new/lein6.png)
