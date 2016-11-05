# Chapter 3: Concurrency

**The Chat Returns**

This is based on the [track2-functional](https://github.com/clojurebridge-minneapolis/track2-functional/) repository.

## Goals
* To demonstrate some of Clojure's tools for managing concurrency.
* To illustrate higher order functions and functional composition.

## Our story so far...
The track1 curricula presents a guide for building a simple web-based
chat application.  The application allows it's users to read a single,
shared conversation and post new messages with using the name and
message content of their choosing.

### Limitations
The chat server keeps all of the messages it receives in application
memory.  Because there are limits to the amount of memory available
it will eventually fail in strange ways.  It may crash, it may return
errors when someone tries to use, or it may be able to display the
messages but fail when new messages are sent.  It can be difficult
to predict exactly what will happen when an application runs out of
memory.

### Goals
To prevent the application from running out of memory there are
a handful of changes that can be made:

* Limit the number of messages kept in the chat history.
* Keep a running total of all the messages ever posted.
* Retain a count of the number of messages posted by each user.

By imposing a limit on the number of messages the program keeps, we can
ensure it doesn't run out of memory.  Keeping a count of messages, both
an overall total and per-user counts, keeps users informed about how
active the conversation and it's participants have been.

In this section you will find a **specification** for how these features
should behave, followed by a series of **implementation** that provide
varying degrees  **correctness**.

## Some Definitions
* Mutable:  Something that can be changed (mutated).
* Immutable:  Something that is unchangeable.
* Imperative:  A command, sometimes a request.
* Expression:  A symbol or combination of symbols which can be
evaluated to produce a result.

## The specification
### The Conversation Database

1. The chat conversation will be stored in a `Map`, a collection of
key/value pairs, also known as an _associative array_, _dictionary_,
or _hash_.
1. This `Map` will contain four (4) fields:
  * `limit`:  A number indicating the maximum number of messages to retain.
  * `messages`:  A list holding the message history.
  * `counts`:  Another `Map` where each key is the name of a user and each
value is the number of messages sent by that user.
  * `total`:  A counter of all messages sent over the lifetime of this
conversation.

Or, to put it more simply, a JSON representation of the conversation database
would look like this:

```
{ "limit"    : 20,
  "messages" : [
                { "name": "Alice",
                  "message": "Has anyone seen the white rabbit?"
                },
                { "name": "White Rabbit",
                  "message": "I'm Late!"
                }
              ],
  "total"    : 2,
  "counts"   : { "Alice": 1,
                 "White Rabbit": 1
               }
}
```

### The Algorithm
When a new message is received:

1. Create a new message record using the `name` and `message` provided by
the user.
1. Add this new message record to the beginning of the chat history.
1. If the conversation contains more messages than what the `limit` specifies,
then remove the excess entries from the end of the list.
1. Increment the `total` counter for the conversation.
1. Find the message count for the user, and increment that too.

### Correctness and Consistency
Before we attempt to solve this problem, we should describe the behavior and
the results of a *correct* solution.  For example:
* When I post a message to the conversation, my message should be the first
  in the list.
* When users see the conversation the information must be *consistent*.
 * The conversation must never contain more messages than it's limit.
 * The `total` should be equal to the sum of the per-user counters in `counts`.
 * The `total` should be equal to or greater than number of messages present.

What does consistency mean?  The algorithm described above lists several steps
each of which updates a different attribute of the conversation.  Messages
are added and possibly removed, counters are incremented, etc.  If you were
to peek at the conversation while the changes were in progress, or if the
changes failed part way through, you might see numbers that didn't add up or
not see the message that was just received.  We need to ensure the program
doesn't show inconsistent data to the users.  For a non-critical application,
like a chat system, this may seem overblown.  However in software handling
monetary exchanges, or the transfer of in-game assets, ensuring that data
remains consistent is critically important.


## Attempt #1: Basic Mutable Data
### Constructing the database
In order to illustrate some of the differences in how mutable and immutable
data types are used, this example will build a conversation database using
Java's basic, mutable `HashMap` and `LinkedList` classes rather than Clojure's
standard immutable maps, lists, and vectors.  Because this example
uses specific Java classes, Clojure's _Java interop_ support is used to
create and interact with these native Java classes and objects.

```
(defn new-mutable-conversation-db
  [message-limit]
  (let [conversation (new HashMap)]
    (.put conversation :limit    message-limit)
    (.put conversation :total    0)
    (.put conversation :counts   (new HashMap))
    (.put conversation :messages (new LinkedList))))
    conversation
    ))
```

The example above is written in an imperative style, as a series of commands.

1. Construct a new `HashMap` and give it then name `conversation`.
1. `put` four new entries into the `HashMap`, defining a fresh conversation.
1. Return the configured `conversation`.

The `let` expression may contain multiple statements, but will only return
the value of the last expression inside of it's parenthesis.  If `conversation`
wasn't at the end of the statements, and the function ended with one of
the calls to `(.put conversation ...)` instead, then the function would return
the result from that `(.put ...)`, which would be nothing, or `nil`.  This is
because the design of `put` is to mutate, _eg._ _modify_ or _change_, the
`Map`.

Fortunately, Clojure provides a simpler way to write the same code with less
repetition, and less room for mistakes.

```
(defn new-mutable-conversation-db
  [message-limit]
  (doto (new HashMap)
    (.put :limit    message-limit)
    (.put :total    0)
    (.put :counts   (new HashMap))
    (.put :messages (new LinkedList))))
```

This `doto` form will take it's first argument, a new `HashMap` in this
case, use it as the first argument to each of the `(.put ...)` calls,
and return the mutated object.  Clojure contains many such shorthand
forms to reduce common redundancies, and ensure consistent behavior.

### Implementing the algorithm
Clojure is designed for writing expression-oriented, functional logic with
immutable values, but as we saw in the conversation constructor above, the
language doesn't prevent you from writing imperative, mutating logic when
needed.  Mutable data and imperative logic go hand-in-hand.  Since this
function will be adding a new message to a mutable conversation, the code
has an imperative flow to it.

```
(defn mutating-add-message
  [conversation name new-message]
  (let [limit    (.get conversation :limit)
        total    (.get conversation :total)
        counts   (.get conversation :counts)
        messages (.get conversation :messages)]

    (.put counts
      name (inc (.getOrDefault counts name 0)))

    (.addFirst messages
      (doto (new HashMap)
            (.put :name name)
            (.put :message new-message)))

    (loop []
      (when (< limit (.size messages))
            (.removeLast messages)
            (recur)))

    (doto conversation
      (.put :total (inc total)))))

```

To restate this in English:

1. Get the `:limit`, `:total`, `:counts`, and `:messages` fields
from the supplied conversation.
1. Update the `counts` by incrementing the count for the provided `name`.
1. Construct a new message and add it to the front of the `messages` list.
1. Remove any excess `messages` using a loop
1. Update the `total` number of messages for the conversation.
1. Return the conversation.

Each of the steps mutates, or changes, the conversation data in-place.
This is likely a familiar pattern, but as we shall discover not only is
the code more verbose than equivalent expression based, functional code,
making it safe for multi-threaded use can be challenge.

Now that we have these two functions `new-mutable-conversation-db` and
`mutating-add-message` we can now run some simple experiments using these
functions.

```
=> (new-mutable-conversation-db 20)

{:counts {}, :limit 20, :total 0, :messages ()}

=> (mutating-add-message
     (new-mutable-conversation-db 20)
     "Alice"
     "Friends, Romans, countryman")

{:counts {"Alice" 1}, :limit 20, :total 1,
 :messages ({:name "Alice", :message "Friends, Romans, countryman"})}
```

### Testing the implementation
Since we expect multiple people to use this application simultaneously,
having a test case that can simulate this type of parallel activity would
let us verify the correctness of our code.

First let's load the test code, and switch to it's namespace so we
can use some of the tests.

```
=> (load "/chatter/handler_test")

nil

=> (in-ns 'chatter.handler-test)

 #<Namespace chatter.handler-test>
```

In the `chatter.handler-test` namespace you can find a
`simulate-conversation` function that provides an example of we can write
a test that exercises our code in parallel.

```
(defn simulate-conversation
  [constructor message-limit add-message message-count]
  (let [conv     (constructor message-limit)
        names    ["Alice" "Bob" "Cindy" "Doug"]
        messages ["Hello"
                  "Good news everyone!"
                  "What are we going to do tonight Brain?"]]
    (doall
      (pmap (partial add-message conv)
            (take message-count (cycle names))
            (take message-count (cycle messages))
            ))
    conv))
```

This `simulate-conversation` function takes four arguments.  Two of those
arguments, `constructor` and `add-message`, are expected to be functions.
The simulator will use the `constructor` function to create a new
conversation of `message-limit` messages.  The `add-message` function will
be used to add `message-count` messages to the conversation.

> **A few amazing features**
>
> **Infinite Lists:**
> Computers do not have infinite memory, storage, or processing capacity, so
> something like infinite lists may sound like a strange capability.  The
> simulator creates two small lists, `names` and `messages` that it will use
> for the `name` and `message` arguments when calling the `add-message`
> function.  Passing these small lists to the `cycle` function will return an
> infinite list that cycles through the list of provided values.  From this
> infinite list the simulator will `take` a finite number of values.  This is
> made possible by _Lazy Evaluation_.  By using a lazily generated list the
> program will never have a full copy of the list in memory at any
> time.  The contents of the list will be generated as it is read. As new
> values are read from the beginning of the list they will be discarded,
> leaving the remainder of the list, which is just the continuation of the
> lazy list, will retained.
>
> For fun you can try this out in your REPL, but remember to `take` a limited
> number of values, otherwise
> an infinite list can crash the REPL.
>
> ```
> => (take 10 (cycle ["Alice" "Bob" "Cindy" "Doug"]))
>
> ("Alice" "Bob" "Cindy" "Doug" "Alice" "Bob" "Cindy" "Doug" "Alice" "Bob")
> ```
>
> **Currying:**
> If you look for uses of `add-message` in the `simulate-conversation` function,
> you will find it wrapped with the `partial` function.  This is a form of
> _functional composition_.  For example, if I wanted a function that doubles
> a number, I could use `partial` to combine the multiplication function `*`
> with an argument of `2` thus _composing_ a doubling function.
> ```
> (def double (partial * 2))
> ```
> Calling `(double 5)` would become `(* 2 5)`.  In the simulator, I always
> want to call the `add-message` function on `conv`, so `partial` takes care
> of that for me, and turns `add-message` into a function of two arguments
> instead of three.
>
> **Easy Parallelism:**
> Like the `map` function, `pmap` will apply a function to each entry in a
> list of values, and return a new list composed of the results.
> Additionally, `pmap` will apply these functions in parallel using a pool
> of threads.  For long-running tasks, this can reduce the total time needed
> to process the entire collection.

Let's run the conversation simulator and see what we get.

```
=> (simulate-conversation
      new-mutable-conversation-db 20
      mutating-add-message 500)

NoSuchElementException   java.util.LinkedList.removeLast (LinkedList.java:283)
```

The shock, the horror, the failure.  The simple explanation for failures like
this is that Java's basic data structures, like LinkedList and HashMap,
aren't "thread safe", they can't be modified by two threads at the same time.

### Re-implementing the algorithm
Java, and many other languages, provides concurrent, thread-safe versions
of common collections types.  So let's whip up an alternate implementation of
`new-mutable-conversation-db`.  This version will be called
`new-mutable-concurrent-conversation-db` and will use alternate, thread-safe
collections.

```
(defn new-mutable-concurrent-conversation-db
  [message-limit]
  (doto (new ConcurrentHashMap)
    (.put :limit    message-limit)
    (.put :total    0)
    (.put :counts   (new ConcurrentHashMap))
    (.put :messages (new ConcurrentLinkedDeque))))
```

The two classes chosen, `ConcurrentHashMap` and `ConcurrentLinkedDequeu`,
implement the same interfaces as their simpler non-thread-safe counterparts.
So we can still use the `mutating-add-message` function without any changes
to add messages to a conversation.  Below are some simple test to verify
the new constructor function works, and that `mutating-add-message` can
update it correctly.

```
=> (new-mutable-concurrent-conversation-db 20)

{:counts {}, :limit 20, :total 0, :messages #<ConcurrentLinkedDeque []}

=> (mutating-add-message
     (new-mutable-concurrent-conversation-db 20)
     "Alice"
     "Friends, Romans, countryman")

{:counts {"Alice" 1}, :limit 20, :total 1,
 :messages #<ConcurrentLinkedDeque [{:name=Alice, :message=Friends, Romans, countryman}]>}
```

So far, so good.  Next let's try using it with the parallel simulator and
see how it holds up.

```
=> (simulate-conversation
     new-mutable-concurrent-conversation-db 20
     mutating-add-message 500)

{:counts { ... }, :limit 20, :total ...,
 :messages #<ConcurrentLinkedDeque [ ... ]>}
```

For brevity, the result has been truncated.  To quickly check the results,
the value of `:total` should by 500, and the sum of the per-user message
counts should also be 500.  If the values you see don't add up, then try
running the simulation and checking the results a couple more times to see
what comes back.

### What went wrong
After studying the results from simulator using conversations constructed
from either basic mutable collections or even thread-safe collections,
it should be apparent developing thread-safe code can be very challenging.
There are very fundamental reasons for this complexity, as the problem
doesn't just exist in the choice of data structures, but exists all the
way down to the most basic level of the computing machinery, and is even
present everywhere in the physical world.

The most basic reason these coordination problems exist is because there are
three steps that need to occur for any change to be made.  First the current
state of the thing you wish to change must be observed.  Second, a new state,
or value, will be calculated based on the observed state.  Last, the data
will be updated with the new value or state.  If two or more tasks are
actively observing, calculating, and updating even a single address in memory,
changes made by one task are invalidating the observations and calculations
made by the other tasks, but the other tasks continue on anyway, oblivious to
these changes.  This leads to lost changes, or changes that are inconsistent
with other changes.

For example, let's imagine a program has been written where two threads
attempt to increment the same number in memory.  If one thread reads the
value, increments it, and writes it back to memory all before the other
thread performs the same steps, then we will see that the original value
was incremented twice.  On the other hand, if both threads read the value
before the other has made the change, then both threads will independently
increment the same value, and calculate the same result.  Each thread will
then write it's result back to memory, and outcome will be the original
value only incremented once instead of twice.

### Visualizing Mutable Data
<img src="https://i.vimeocdn.com/video/511735825_1280x939.jpg"
      alt="Mutating Data" width="426" height="313"/>
[Mutating State](https://player.vimeo.com/video/122669829?byline=0&portrait=0)

## Attempt #2: Coordination with Locking
### The theory
One way of coordinating changes across threads is through _locking_.  With
locking we can create a barrier that will only allow one thread access to
a variable at a time.  The first thread to acquire the lock will continue
with it's work.  Any other threads that attempt to acquire the lock while
it's being held will pause until the lock has been released.

### The implementation
Since locking is something that is performed to coordinate changes to a
variable, we will write a new function for adding messages to the
conversation database.  

```
(defn locking-add-message
  [conversation name message]
  (locking conversation
    (mutating-add-message conversation name message)))
```

This is simple enough.  Just like the `mutating-add-message` function, this
function takes the same three arguments, `conversation`, `name`, and
`message`.  It uses the `locking` function to acquire a lock on the
`conversation` variable it received, and then reuses the existing
`mutating-add-message` function to perform the change.  Syntactically we
can see how `locking` the `conversation` literally wraps the call to
`mutating-add-message`.  

### Testing the implementation
Let's try running the simulator with the new `locking-add-message` function.

```
=> (simulate-conversation
     new-mutable-conversation-db 20
     locking-add-message 500)

{:counts {"Bob" 125, "Alice" 125, "Cindy" 125, "Doug" 125},
 :limit 20, :total 500,
 :messages ( ... )}


=> (simulate-conversation
     new-mutable-concurrent-conversation-db 20
     locking-add-message 500)

{:counts {"Bob" 125, "Alice" 125, "Cindy" 125, "Doug" 125},
 :limit 20, :total 500,
 :messages #<ConcurrentLinkedDeque [ ... ]>}

```

These results are much better.  The `:total` field matches the number of
messages added to the conversation, and the per-user `:counts` also add up
to the expected number of messages.  Now we can update our application to
build the conversation database with either the `new-mutable-conversation-db`
or the `new-mutable-concurrent-conversation-db` and add messages with the
`locking-add-message` function and all should be well.

The changes can be made in the `chatter.handler` namespace.  Find the
definition of `CONVERSATION-DB` and update it to use the constructor of
your choice, then navigate down a little further and update the route
for `(POST "/" [] ...)` to call `locking-add-message` instead of
`mutating-add-message`.

The server can be started from the command-line using the command
`lein ring server`, or can be started from within the REPL using the command
`(start-server)`.  Either of these options should open up your web brower
and direct it to the application.  Go ahead and add a couple messages to
make sure it works for you.

With the server running there is another round of testing we can perform.
Because almost all of the data in the conversation database is used to build
the HTML shown to the user, we could develop a test that sends a new message
to the server, the same way your web browser would, then reads the HTML
response, and reconstructs the state the conversation database was in
at the time the page was generated.  With this reconstructed view of the
conversation database, the test can verify a couple of requirements:

1. The total number of messages equals the sum of the per-user message counts.
1. The conversation contains the message sent by the test.

If you are interested in seeing how the conversation data is extracted from
the rendered HTML, you can review and experiment with the functions
`parse-chat-response`, `extract-chat-user-counts`, and `extract-chat-messages`.

In the meantime, you can use the `post-message` function to send a message to
your server, and display the parsed response.

```
(defn post-message
  [url name message]
  (parse-chat-response
    (http/post url {:form-params {:name name :msg message}
                    :as :stream})))

=> (post-message
     "http://localhost:8000/"
     "The Cat"
     "I'm hungry!")

{:messages ({:name "The Cat", :message "I'm hungry!"}),
 :counts {"The Cat" 1},
 :total 1}
```

In this example the URL `http://localhost:8000/` should match the URL shown
in your browser.

The only piece of information missing from the reconstructed conversation
is the message limit.  If we knew this, we could also confirm that the number
of messages never exceeded that limit, but we will let that slide.

Building on `post-message` we can write a `post-message-and-check` function
to inspect the result and ensure it meets the expectations.

```
(defn post-message-and-check
  [url name message]
  (let [expected-message {:name name :message message}
        conv (post-message url name message)]
    (and (is (some (partial = expected-message)
                   (:messages conv))
             "Must see my own new message")

         (is (= (:total conv)
                (reduce + (vals (:counts conv))))
             "Total must equal the sum of the user counts")
         conv)
    )))

=> (post-message-and-check
     "http://localhost:8000/"
     "The Cat"
     "Still hungry!")

true
```

Now that the pieces are in place, the `simulate-conversation` function can
once again be used to simulate a conversation using HTTP calls instead of
just operating directly on local data.  If we look at the
`post-message-and-check` function the arguments it expects are similar to
both the `mutating-add-message` and `locking-add-message` functions.  The
second and third arguments are identical, the only difference is the first
argument to `post-message-and-check` is a URL instead of a data structure.
So, in place of the constructor function, we can pass
`(constantly "http://localhost:8000")` and `nil` as the two constructor
parameters to `simulate-conversation`.  Another pair of arguments that
would work are `identity` and `"http://localhost:8000/"`.

```
=> (simulate-conversation
     (constantly "http://localhost:8000/") nil
     post-message-and-check 500)
```

At this stage, this will likely produce a long series of failure messages.

What could have gone wrong?  Every time the program adds a new message, it
uses `locking` to ensure no other user is also adding a message, and then
uses the updated data to render the page.  What happens when we are reading
the data from the conversation?  The `locking` function hasn't been used
anywhere that reads data yet, just for the updating.  So if one thread were
updating the data, another thread, or even several threads, could still read
from the conversation while the update is in progress, and before the data
is once again consistent.  This means that we not only need to lock all places
where the conversation data is modified, we also need to use `locking` when
the data is being read and rendered into HTML.  This affects not only the
`(POST "/" [] ... )` route used to add a new message, but also the
`(GET "/" [] ... )` used to just display the chat messages.

In a small program, like this chat server, finding every place that the
conversation data is used isn't terribly difficult, but in larger programs
tracking every place that some data structure is used and ensuring that
proper locking has been maintained can be a significant amount of effort.
To globally ensure the consistency of the conversation data, we can use a
function, idiomatically referred to as _middleware_, that intercepts every
request made to the server.  This function can enforce the lock on the
conversation database, and then the application will always return consistent
information.

```
(defn wrap-with-lock
  [handler]
  (fn [request]
    (locking CONVERSATION-DB
      (handler request))))
```

In this `wrap-with-lock` function we can see the use of a functional
programming technique called _functional composition_.  The `handler`
argument to `wrap-with-lock` will be a function that can handle the incoming
requests.  The `wrap-with-lock` will return a new function, as indicated by
the `(fn [request] ... )` form that will receive the `request` first, acquire
a lock on the `CONVERSATION-DB`, and then pass the `request` to the original
`handler` function.  Using this technique of wrapping a function in another
function we can progressively build up increasingly elaborate request
handling pipelines while keeping each individual step as simple as it needs
to be.  Another instance of _functional composition_ is the `partial`
function used in `simulate-conversation` to construct a new function with
fewer arguments.

### The analysis
Writing programs that can accurately and consistently manage changes to
mutable data structures across multiple active threads is very difficult.
Often the shortcomings in the code won't be caught through basic correctness
testing, but are found in the wild, on active production systems, where the
impact may range from merely obnoxious to very disruptive and damaging.
Finding every instance in a large program where two procedures may share,
and possibly make changes to, the same data structure often borders on
impossible.

This is a small program with an easy to find hinge point through which all
data in and out of a single data structure will pass.  Because of this simple
design, controlling access to the data through locking is relatively easy
to accomplish.  However there is a significant down side to this approach:
only one thread, representing the actions of one user, can interact with
the conversation data in any way at any time.  The core of the application
has become serialized, single-threaded, and the ability to serve many
users at once has been substantially diminished.


## Attempt #3: The Immutable Approach
### What is immutable data?
Fortunately, Clojure provides a simpler alternative to the lock-and-mutate
approach.  Clojure's standard collections types are very different from the
collections typically found in imperative languages.  Most importantly,
they are immutable.  Immutable collections cannot be modified after they
have been constructed.  If a value can't be modified, it can be shared and
reused without the need to worry about modifications of any sort.  No matter
how many times the value is shared, it will never be modified.  This removes
the need for techniques like _defensive copying_ and _read-locking_.

It may sound strange to try to work with immutable data, but it's surprisingly
easy in Clojure.

```
=> (let [profile {:given-name "Grace"
                  :family-name "Hopper"}]
     (assoc profile :title "Rear Admiral"))

{:given-name "Grace" :family-name "Hopper" :title "Rear Admiral"}
```

In the example above a `Map` is created with the keys `:given-name` and
`:family-name`.  This map value is then given the name `profile`.  The
last line `(assoc profile :title "Rear Admiral")` returns a new map,
containing all of the keys and values in the original `profile` map, but
with an additional entry for `:title`.  Functions like `assoc`, `dissoc`,
`conj`, and `cons` don't modify the collections they operate on, but instead
always return new collections that reuse the structure of the original value
when possible, but always return a new immutable collection.  Also you may
have noticed that since these functions always return a new value, they
can be more easily written as expressions, rather than imperative statements.

```
(def safe-inc
  (fnil inc 0))

(defn new-immutable-conversation-db
  [message-limit]
  {:limit message-limit})

(defn immutable-add-message
  [{:keys [limit total counts messages] :as conversation} name message]
  {:limit    limit
   :total    (safe-inc total)
   :counts   (update-in counts [name] safe-inc)
   :messages (take limit
                   (cons {:name name :message message}
                         messages))})
```

The three functions implement approximately the same functionality present
in `new-mutable-conversation-db` and `mutating-add-message`.

First, the `safe-inc` function is composed with `fnil` and `inc`.  This
provides a variation of `inc` that will replace `nil` arguments with `0`.
So `(safe-inc nil)` returns `1` instead of throwing a `NullPointerException`.

Second, the `new-immutable-conversation-db` constructs an empty conversation
database.  Because all of the functions that will be used to "add" a message
are null-safe, there is no need to pre-populate any fields other than the
`limit`.

Last, is the `immutable-add-message` function.  Just like the
`mutating-add-message` defined earlier, this function will take three
arguments: a conversation database, a name, and a new message.  Unlike
the `mutating-add-message` function, this function will leave the original
conversation database unchanged and return an entirely new conversation
database that reflects what the new state of the conversation is after the
new information has been added.

1. A shorthand syntax called _destructuring_ is used to get important
fields from the original conversation.  This removes several redundant
lines of code.
1. An entirely new map is created using the `{ }` map literal form.  No
need to explicitly call a constructor.
1. The value of the `:limit` field is copied from the original conversation.
1. The `:total` field is populated by incrementing the original `total` with
`safe-inc` so it doesn't matter if it was initially `nil`.
1. The `:counts` field is updated using the very clever `update-in` function.
The `update-in` function "updates" a map by applying a function, `safe-inc`
to the value of a key, `name`.  This increments the message counter for the
user who submitted the message.
1. Finally, the `:messages` field is updated.  The new message item is
constructed with the literal `{:name name :message message}`.  This
message item is added to the front of the `messages` list using `cons`.
If `messages` is `nil`, `cons` will return a new list.  Then `take` is
used to trim the `messages` list down to `limit` entries.

Compared to the `mutating-add-message` function, this function is much more
succinct, and hopefully, the intended result is more evident.

### How can we change it?
Immutable values that can be shared, retained, and reused are great, but
this chat application really does need to add and update it's data, and
preserve the changes.

To safely manage changing values, Clojure provides Atoms.  One way of picturing
an atom is as a box that can hold one, and only one, thing.  The value held
in this box can be replaced with a new value through a very specific set of
steps.

1. The value held be the atom is retrieved.
1. A function is applied to this value, returning a new value.
1. The new value will replace the current value if, and only if, the current
value has not already been replaced by another task.
1. **If the current value of the box, doesn't match the original value, then
the process is repeated from the beginning.**
Through these steps, the logic needed to change a value is encapsulated in
a function.  Intermediate values, like those that would exist in a mutating
process, are never saved in the atom.  The atom can be asked for it's current
value at any time, and you can be confident the value returned will be
consistent for that point in time.

To work with atoms we will use two functions `deref` and `swap!`.  The
`deref` function will return the current contents of an atom, and `swap!`
will be used to change the contents of the atom with a function.

So, let's look at an example and see how this might work.

```
(defn new-atomic-conversation-db
  [limit]
  (atom (new-immutable-conversation-db limit)))

(defn atomic-add-message
  [conversation-atom name new-message]
  (swap! conversation-atom immutable-add-message name new-message))
```

In these new functions, both the `new-immutable-conversation-db` and the
`immutable-add-message` functions have been reused.  The
`new-atomic-conversation-db` builds a new immutable conversation database,
and wraps that value in an atom.  The `atomic-add-message` function will
expect to receive an atom containing one of these immutable conversations.
The `swap!` function will pull out the current contents of the atom, and
apply the `immutable-add-message` function to the contents, and will provide
the `name` and `new-message` values as extra arguments.  Here's a way of
visualizing how `swap!` will apply a function and extra arguments to the
contents of an atom.

```
(=  (swap! conversation-atom immutable-add-message name new-message)
    (immutable-add-message (deref conversation-atom) name new-message) )
```

Now to prove that atoms plus immutable values is both consistent and thread
safe, let's run this atomic conversation through the conversation simulator

```
=> (simulate-conversation new-atomic-conversation-db 20 atomic-add-message 500)

 #<Atom@452bceb5: {:limit 20, :total 500,
                   :counts {"Bob" 125, "Doug" 125, "Alice" 125, "Cindy" 125},
                   :messages ( ... )}>
```

These results look good, the `:total` matches the number of messages we
asked it to add, and the sum of the per-user `:counts` adds up to the total.

### Is it better?
Correctness and consistency are always essential qualities for any
computational tool we use, but you may be wondering what the performance
characteristics are.  If two threads are trying to update the same atom,
one of those will compute a new value for the atom and succeed immediately,
but the other thread will go through all the work of calculating a new
value, determine the contents of the atom has already been change, then
throw away it's work and start over.  That sounds like a lot of wasted
effort.

In the `handler-test` namespace, there is another function called
`parallel-add-messages` this function provides another level of testing
beyond the conversation simulator.  This test case will run the simulator
against different versions of the chat algorithm, check the final conversation
state for correctness, and time how long the process took.  Go ahead and
uncomment (remove the leading `;` character) from the following groups
of functions:

```
         ;;  This should work
         new-mutable-conversation-db
         locking-add-message
         identity

         ;;  This should work too
         new-mutable-concurrent-conversation-db
         locking-add-message
         identity

         ;;  This should work and should perform noticeably better
         new-atomic-conversation-db
         atomic-add-message
         deref
```

Now reload the namespace, run the test, and examine it's results.

```
=> (parallel-add-messages)

Simulation completed in 1076ms: new-mutable-conversation-db locking-add-message

Simulation completed in 1438ms: new-mutable-concurrent-conversation-db locking-add-message

Simulation completed in 120ms: new-atomic-conversation-db atomic-add-message
nil
```
The actual times will vary from system to system, and from run-to-run, but
the overall rankings should be consistent.  The combination of Java's
concurrent data types, and `locking-add-message` will be the slowest,
followed by the simple mutable types implementation with locking, and
the atomic implementations will be substantially faster.

It turns out that "locking" a value is not cheap, and every time a program
locks a value that no other thread attempts to access, referred to as an
_uncontested lock_, the effort of creating the lock is wasted.  With the
atomic approach, no locking is required to safely read a value, and the
computational work of calculating a new value and throwing it away is
usually less than the cost of maintaining locks.  Additionally, the
amount of development time needed to ensure the correct implementation
of a concurrent algorithm is dramatically when using atoms.

Let's wrap up by updating the chat application to use these functions.

```
(def CONVERSATION-DB
  (new-atomic-conversation-db 20))

(defroutes app-routes
  (GET "/" [] (generate-message-view @CONVERSATION-DB))
  (POST "/" {params :params}
    (let [name-param (get params "name")
          msg-param (get params "msg")
          new-messages (atomic-add-message CONVERSATION-DB
                                           name-param msg-param)]
      (generate-message-view new-messages name-param)
      ))
  (route/resources "/")
  (route/not-found "Not Found"))

(def app
  (wrap-params app-routes))
```

## In conclusion
...
