# Install the Heroku Toolbelt

Go to the...
* prev section [Join the #clojure.mn channel](setup_new_irc.md)
* new [Setup](setup_new.md) instructions
* next section [Create an SSH key](setup_new_ssh.md)

--------------

## Sign up for a free Heroku account

Heroku is used to host web applications on the Internet.
You can use Heroku to make one (1) Clojure application
you make to everyone (for free)!

The first step is to create an acccount: https://signup.heroku.com/login

## Download and install the Heroku Toolbelt

The next step is to install the Heroku toolbelt: https://toolbelt.heroku.com/

The Heroku toolbelt has some utilities that help you
publish your application to their platform.

On Mac and Windows it includes the **git** and **ssh-keygen** programs
which you can use to manage source code repositories and
generate secure keys, respectively.

On Linux you can use for favorite package manager (e.g. apt, synaptic,
rpm) to install the relevant packages:

````
# apt-get install git openssh-client
````

_NOTE: in some distributions the git package may still be called_ `git-core`
