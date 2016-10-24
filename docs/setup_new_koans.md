# Try Clojure Koans in Nightcode

## Clone our version of the Clojure Koans

````
clojurista@mylaptop $ mkdir -p ~/src/github/clojurebridge-minneapolis
clojurista@mylaptop $ cd ~/src/github/clojurebridge-minneapolis
clojurista@mylaptop $ git clone https://github.com/clojurebridge-minneapolis/clojure-koans.git
clojurista@mylaptop $ cd clojure-koans
````

## Make a new git branch for your work

Since you will be modifying the repo while working through the Koans
you can do that on a branch (so `git status` and `git diff` show you what you've accomplished). _NOTE: you can name your branch whatever you want!_

````
clojurista@mylaptop $ git branch clojurista
clojurista@mylaptop $ git checkout clojurista
````

## Open the Koans repo in Nightcode

Start Nightcode and then open the clojure-koans project.

Expand/open: src / koans / 01_equalities.clj

In the lower box click "Run with REPL", then type `(exec "run")`

## Solve a Koan

Replace the missing value `__` with the correct value (in this case `true`).

_NOTE: the "meditation" lists the file and line number of the current koan._

Press Control-s (or Command-s) to save the file.

You *should* see the next question pop up in the REPL area (if the answer is correct).

If the same Koan comes up again try a different answer!

_Hint: You can use the mini-REPL box on the lower left hand corner to try out expressions_
