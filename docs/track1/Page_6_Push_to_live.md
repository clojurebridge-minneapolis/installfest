
### Heroku

Up to this point, we've been running the server on our computer, `localhost`(*). This works great while writing or developing the program because it makes testing changes fast and easy. Eventually we'll want to put it on the internet for others to see and use.

To put it on the internet, we're going to use a company called Heroku for hosting(*). They're going to run our program on a machine visible to anybody on the Internet.  We will use use git to send them our program.

The advantage of using a company like Heroku is that they handle the work of actually maintaining a server. One of the  disadvantages is that Heroku can be expensive, but for a little program like ours it's free.

First, we need to make a couple of changes to our app so it can interact with Heroku.

In `handler.clj`, we will add a couple of functions to print log messages when Heroku starts and stops the program.


```clojure
(defn init []
  (println "chatter is starting"))


(defn destroy []
  (println "chatter is shutting down"))
```

Then we need to add a main function. This is what Heroku will actually invoke(*) to start our program.  Add this to the end of `handler.clj`.

```clojure
(defn -main [& [port]]
  (let [port (Integer. (or port (env :port) 5000))]
    (jetty/run-jetty #'app {:port port :join? false})))
```

The `ns` of `handler.clj` should look like:

```clojure
(ns chatter.handler
  (:require [compojure.core :refer :all]
            [compojure.route :as route]
            [ring.middleware.defaults :refer [wrap-defaults site-defaults]]
            [ring.middleware.params :refer [wrap-params]]
            [ring.adapter.jetty :as jetty]
            [hiccup.page :as page]
            [hiccup.form :as form]
            [ring.util.anti-forgery :as anti-forgery]
            [environ.core :refer [env]]))
```

We will still be able to start the app using `lein ring server` but now we can also start by entering `lein run -m chatter.handler` in the command line. When we start it this way, it defaults to port 5000, and you should see that in the browser. Port 3000 does not work anymore, but you see the app if you switch to 5000.

If we create a jar with `lein uberjar`, we can also start the app with `java $JVM_OPTS -cp target/chatter-standalone.jar clojure.main -m chatter.handler`

These new methods of starting the app are closer to what Heroku will use to start the app.

We also need a `Procfile` in the top directory containing the line `web: java $JVM_OPTS -cp target/chatter-standalone.jar clojure.main -m chatter.handler`

Stop the server using control-c, then restart with the command:

```lein run```

If the local web page still works; add and commit your changes, then push to github.

Now we'll deploy to heroku with the following commands:
+   `heroku create`
+   `git push heroku master`
+   `heroku ps:scale web=1`
+   `heroku open`

The final command will open the browser and point it at your app on Heroku.

Try the traceroute command again against the address Heroku assigned your app:

    $: traceroute obscure-brushlands-9918.herokuapp.com
    traceroute to obscure-brushlands-9918.herokuapp.com (50.16.239.160), 30 hops max, 60 byte packets
    1  192.168.1.1 (192.168.1.1)  15.820 ms  15.797 ms  15.773 ms
    2  96.120.49.33 (96.120.49.33)  23.895 ms  25.196 ms  25.192 ms
    3  te-0-2-0-6-sur01.webster.mn.minn.comcast.net (68.85.167.145)  25.179 ms  25.168 ms  25.362 ms
    4  te-0-7-0-13-ar01.crosstown.mn.minn.comcast.net (68.87.174.189)  30.743 ms  31.979 ms te-0-7-0-12-ar01.crosstown.mn.minn.comcast.net (69.139.219.129)  31.979 ms
    5  pos-0-0-0-0-ar01.roseville.mn.minn.comcast.net (68.87.174.194)  31.967 ms * *
    6  he-1-12-0-0-cr01.350ecermak.il.ibone.comcast.net (68.86.94.77)  39.269 ms  25.568 ms  26.422 ms
    7  be-10206-cr01.newyork.ny.ibone.comcast.net (68.86.86.225)  45.276 ms  63.727 ms  66.090 ms
    8  he-0-11-0-0-pe03.111eighthave.ny.ibone.comcast.net (68.86.83.98)  58.386 ms  58.374 ms  58.358 ms
    9  as16509-3-c.111eighthave.ny.ibone.comcast.net (50.242.148.118)  56.143 ms  58.280 ms  65.988 ms
    10  54.240.229.76 (54.240.229.76)  65.977 ms 54.240.229.82 (54.240.229.82)  65.982 ms *
    11  54.240.228.190 (54.240.228.190)  65.874 ms 54.240.228.202 (54.240.228.202)  65.888 ms 54.240.228.196 (54.240.228.196)  65.969 ms
    12  54.240.229.223 (54.240.229.223)  42.112 ms 54.240.229.200 (54.240.229.200)  45.620 ms 54.240.229.221 (54.240.229.221)  47.673 ms
    13  54.240.228.173 (54.240.228.173)  47.630 ms 54.240.228.177 (54.240.228.177)  52.382 ms 54.240.228.173 (54.240.228.173)  45.498 ms
    14  72.21.220.100 (72.21.220.100)  52.589 ms 72.21.220.116 (72.21.220.116)  52.527 ms 54.240.228.139 (54.240.228.139)  50.825 ms
    15  72.21.220.135 (72.21.220.135)  56.875 ms 72.21.220.167 (72.21.220.167)  59.011 ms 72.21.220.151 (72.21.220.151)  59.014 ms
    16  72.21.220.108 (72.21.220.108)  48.813 ms 205.251.245.242 (205.251.245.242)  49.425 ms  48.614 ms
    17  * * *
    18  * * *
    19  * * *
    20  216.182.224.85 (216.182.224.85)  55.704 ms 216.182.224.223 (216.182.224.223)  66.895 ms 216.182.224.227 (216.182.224.227)  54.629 ms
    21  * * *
    22  * * *
    23  * * *
    24  * * *
    25  * * *
    26  * * *
    27  * * *
    28  * * *
    29  * * *
    30  * * *

Try going to each other's app.

To delete the app from Heroku, select the app in the dash board, click settings, delete app is on the bottom.  Then,`git remote remove heroku` in the command line.
