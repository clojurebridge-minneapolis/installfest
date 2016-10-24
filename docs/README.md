# ClojureBridgeMN Documentation

## View the documentation online

* [http://clojurebridge-minneapolis.github.io](http://clojurebridge-minneapolis.github.io)

## Editing the documention

### Install dependencies

* For Mac and WIndows
  1. Install python, pip, and virtualenvwrapper
  2. Set up a virtualenv for the project with virtualenvwrapper. I chose to call mine "installfest"
  3. `pip install -r requirements.txt`
* For Linux (e.g. debian unstable)
  1. `apt-get install python-alabaster python-argh python-babel python-recommonmark python-docutils python-imagesize python-livereload python-markupsafe python-pathtools python-pygments python-yaml python-recommonmark python-six python-snowballstemmer python-sphinx python-sphinx-rtd-theme python-tornado python-watchdog python-pip`
  2. `pip install port-for`
  3. `pip install sphinx-autobuild`

### Develop with the interactive autobuilder

1. `make html`
2. Open [http://localhost:8000/](http://localhost:8000/) in your favorite browser
  NOTE: the built in server is serving static files from `_build/html/index.html`
3. When you are done auto building simply ^C the make process

This will start an autobuild process which will:
* detect file changes in *.rst and *.md files and will regenerate the docs
* cause any connected browsers to auto-reload

### Create the docs once

This is useful if you do NOT want the autobuilder or built-in web server.

Simply run `make justhtml`

### Auto-publication

Once changes in this repo are pushed to master a bot will
publish the updated documentation on [http://clojurebridge-minneapolis.github.io](http://clojurebridge-minneapolis.github.io)
