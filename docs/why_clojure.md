**Why Clojure?**

## What is programming?

Programs are a set of instructions for a computer to follow.

Computer programming is about expressing the instructions in
a language that can be translated down to the detailed steps that
a microprocessor can understand.

Programs that do something interesting enough that we would
recognize -- like sending an e-mail, uploading a photo or
making a restaurant reservation -- are, themselves, made up of
many smaller programs.

Software developers often start with a big goal like
"make a restaurant reservation application" and break it up
into smaller pieces. In fact this process happens repeatedly
until the pieces are tiny and bite-sized.

Because everyone likes to eat let's use preparing an Italian
dinner as an analogy. The goal is:

* Italian Dinner

So let's start by breaking that into independent parts: we'll combine all three parts to make the meal.

* Italian Dinner
  1. Fettuccine Alfredo
  1. Salad
  1. Red Wine


We can even break the parts into steps:

* Italian Dinner
  1. Fettuccine Alfredo
     * Cook pasta
     * Make Alfredo sauce
  1. Salad
     * Cut veggies
     * Make homemade vinaigrette
  1. Red Wine

Now each of the steps (except maybe making the sauce :) is short and straightforward.

In computer lingo we would say that each of these steps are "functions":
each is designed to accomplish a specific task. A "program" is just
a big function that's built on top of lots of other little functions.


## What is "functional programming"?

The words "functional programming" mean more than just programming
with functions. It means programming with small functions that
take some input and provide some output without depending on
other knowledge of how "the system" is running.

By contrast "imperative programming" describes functions that
are changing things in "the system" that you have to remember.
Set the variable *X* to $19.95 and the variable *Y* to 3.
At first the "change this, then change that..." approach
seems easy to understand until you get a lot of details that
you have to keep in your head to understand the function.

In "functional programming" the idea is to have short
functions where you can *see* everything that's happening
without having to remember what's going on outside the function
(i.e. in "the system"). Clojure programmers like to talk about
functions taking in data of a certain shape and returning
data of a different shape. The data could be numbers or text
or collections of things.

## Why Clojure??

Clojure is a modern adaptation of of the classic functional programming
language LISP (which stands for list processing language) which dates
back to 1954!

In LISP everything is a list. The functions you write are lists.
The data for your functions are lists. The clever bonus of having
everything is a list is that programs and data _look the same_.
That means we can have functions that create or improve other functions
because they all look like data!

Many programmers are surprised by LISP because the parentheses are
always on the outside:

    (italian-dinner main-course side beverage)

... because many popular computer languages put the function name
first (instead of at the beginning of the list of arguments)

    italian_dinner(main_course, side, beverage);

In designing Clojure Rich Hickey combined the good parts of LISP
and combined it with the powerful Java Virtual Machine.
As a result Clojure is an *opinionated* LISP that...

* Is simpler to learn and understand (much easier than Java)
* Has higher performance and works on a broad range of computers thanks to the Java Virtual Machine (_NOTE: Clojure also works on JavaScript and the CLR_)
* Provides easy inter-operation with many existing Java libraries (makes it practical to write real-world programs)
* Can be used for both client and server development (ClojureScript has one of the
  best stories for web development)

And it turns out the Clojure community is wonderful
* Tends to be full of explorers: people that are curious, eager to learn and happy to help each other
* Even though typically big companies are hesitant to adopt new technologies Clojure is beginning to see more commercial use.
* Values diversity (e.g. conference opportunity grants, ClojureBridge)

## Open Source: why the Community is important

Clojure is open source software... what does that mean?

Open source software is...

* Designed to be shared for free with everyone (using a hack on copyright)
* Created by volunteers (and increasingly by companies)
* Published publicly to encourage collaboration and improvement

Typical examples are Linux (runs in every Android phone), the Firefox web browser,
and the Libre Office suite of tools.

Why makes open source software great?

1. Volunteers work on projects that they are passionate about
1. You can adapt and change it to make it work for you
1. The investment in learning an open source program often pays off because (good ones) often last a long time
1. Because the code is open anyone can inspect it for security weaknesses or privacy vulnerabilities.
1. Everyone can get involved simply by being a user, reporting bugs, working on documentation and, yes, contributing code.

Why is diversity in technology (especially open source software) important?
* Women are 50% of users, but only 15% contributors
* For any software to be successful it *has* to take the needs of users into account
* In order to make better software we need YOU!


## A journey begins...

Today you will publish a program on the Internet!

After today you can continue your learning...

* Join the [clojure.mn](http://www.meetup.com/clojuremn/) user group
* Continue to converse with people you've met on [Slack](http://clojurebridge-minneapolis.github.io/setup.html#instructions-for-all-slack)
* Join us at the [Clojure Conj](http://2016.clojure-conj.org/) - registration is free for ClojureBridge alumnae! *note: lambda ladies, opp grants*
* Going further with the [Google Summer of Code](https://developers.google.com/open-source/gsoc/) (e.g. [Maria Geller's work on ClojureScript](https://www.youtube.com/watch?v=Elg17s_nwDg)) and [Outreachy](https://www.gnome.org/outreachy/)
* Code academies (e.g. [Prime Digital Academy](https://primeacademy.io/))

Software is everywhere!

*speaker note: reveal the secret now*
