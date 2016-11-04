# Chapter 4: ClojureScript

This exercise is to simply give you:

- a taste of interactive development using ClojureScript (you get a CLJS REPL _in the browser_)
- examples of JavaScript interop
- pointers to go further

## Watch the flappy bird demo video

The [video](https://www.youtube.com/watch?v=KZjFVdU8VLI) only 6 minutes, but gives you the context for flappy-bird.

Next read (but don't follow the instructions yet) the [blog post](http://rigsomelight.com/2014/05/01/interactive-programming-flappy-bird-clojurescript.html) about this flappy bird demo.

## Clone flappy-bird-demo

We've updated the flappy-bird-demo in our ClojureBridgeMN organization so
you can benefit from the latest versions of libraries. After you
clone it check out our branch with the modifications (because we're
hoping Bruce will accept our changes in a Pull Request).

```
clojurista@mylaptop$ git clone git clone https://github.com/clojurebridge-minneapolis/flappy-bird-demo.git
clojurista@mylaptop$ git checkout tmarble/ClojureBridgeMN-track2
```

## Try ClojureScript with flappy bird

Use **[lein figwheel](https://github.com/bhauman/lein-figwheel/)** to
create an interactive browser REPL:

```
clojurista@mylaptop$ lein figwheel
Figwheel: Cleaning because dependencies changed
Figwheel: Cutting some fruit, just a sec ...
Retrieving figwheel/figwheel/0.5.4-7/figwheel-0.5.4-7.pom from clojars
Retrieving ...
Figwheel: Validating the configuration found in project.clj
Figwheel: Configuration Valid :)
Figwheel: Starting server at http://0.0.0.0:3449
Figwheel: Watching build - flappy-bird-demo
Figwheel: Cleaning build - flappy-bird-demo
Compiling "resources/public/js/flappy_bird_demo.js" from ["src"]...
Successfully compiled "resources/public/js/flappy_bird_demo.js" in 13.933 seconds.
Figwheel: Starting CSS Watcher for paths  ["resources/public/css"]
Launching ClojureScript REPL for build: flappy-bird-demo
Figwheel Controls:
          (stop-autobuild)                ;; stops Figwheel autobuilder
          (start-autobuild [id ...])      ;; starts autobuilder focused on optional ids
          (switch-to-build id ...)        ;; switches autobuilder to different build
          (reset-autobuild)               ;; stops, cleans, and starts autobuilder
          (reload-config)                 ;; reloads build config and resets autobuild
          (build-once [id ...])           ;; builds source one time
          (clean-builds [id ..])          ;; deletes compiled cljs target files
          (print-config [id ...])         ;; prints out build configurations
          (fig-status)                    ;; displays current state of system
  Switch REPL build focus:
          :cljs/quit                      ;; allows you to switch REPL to another build
    Docs: (doc function-name-here)
    Exit: Control+C or :cljs/quit
 Results: Stored in vars *1, *2, *3, *e holds last exception object
Prompt will show when Figwheel connects to your application
To quit, type: :cljs/quit
cljs.user=> (println 123)
123
nil
cljs.user=> (.-location (.-document js/window))
#object[Location http://0.0.0.0:3449/]
cljs.user=> :cljs/quit
clojurista@mylaptop$
```
Open your browser to [http://localhost:3449](http://localhost:3449)

Turn on your developer console.

When you type `(println 123)` you are interacting with the browser _live_!

You can try some JavaScript interop with `(.-location (.-document js/window))`

Now open the ClojureScript source file in your editor: `src/flappy_bird_demo/core.cljs`

See the string for the button label `"START"` on line 187. Change it to `"START now!"` and see the browser _update automatically_ when you save `core.cljs`.

Press the **START now!** button to start playing! Use your mouse button to **JUMP**!

To quit a CLJS REPL type `:cljs/quit`


## Going further

 * [ClojureScript.org](http://clojurescript.org)
 * [lein-figwheel](https://github.com/bhauman/lein-figwheel/)
 * [tenzing](https://github.com/martinklepsch/tenzing)
 * [klipse](https://github.com/viebel/klipse) and the [klipse/clojure blog](http://blog.klipse.tech/clojure/2016/06/07/klipse-plugin-tuto.html)
