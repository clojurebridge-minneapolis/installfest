# Older koans documentation

The Clojure Koans are a series of exercises designed to teach Clojure. Periodically
through the track 2 curriculum we'll be referencing them. For this document we'll
make sure we can get them downloaded, setup, and you can even start doing the first
couple sets of problems.

More information can be found at the Clojure Koans website: http://www.clojurekoans.com

** Getting started **

First lets clone the koans repository:

`git clone https://github.com/clojurebridge-minneapolis/clojure-koans`

Lets poke around the repo we just cloned:

`cd clojure-koans`

Inside here there should be a src directory containing the following:

```
└── src
    └── koans
        ├── 01_equalities.clj
        ├── 02_lists.clj
        ├── 03_vectors.clj
        ├── 04_sets.clj
        ├── 05_maps.clj
        ├── 06_functions.clj
        ├── 07_conditionals.clj
        ├── 08_higher_order_functions.clj
        ├── 09_runtime_polymorphism.clj
        ├── 10_lazy_sequences.clj
        ├── 11_sequence_comprehensions.clj
        ├── 12_creating_functions.clj
        ├── 13_recursion.clj
        ├── 14_destructuring.clj
        ├── 15_refs.clj
        ├── 16_atoms.clj
        ├── 17_macros.clj
        ├── 18_datatypes.clj
        ├── 19_java_interop.clj
        ├── 20_partition.clj
        └── 21_group_by.clj
```

From the `clojure-koans` directory run:

`lein koan run`

This will start executing the koans. You should see something similar to the following:

```
Starting auto-runner...
Considering /home/brian/repos/clojure-koans/src/koans/01_equalities.clj...

Now meditate upon /home/brian/repos/clojure-koans/src/koans/01_equalities.clj:6
---------------------
Assertion failed!
We shall contemplate truth by testing reality, via equality
(= __ true)
```

Now open up the clojure-koans base repository directory in your preferred editor.
Open up the first set of koans `01_equalities.clj`. The objective of the koans is to
fix the assertion that is currently being displayed in your terminal by filling in
the `__`. As you save the file it will check to see if your answer is correct and
move on to the next koan.

To solve the first one, lets change:

```clojure
  "We shall contemplate truth by testing reality, via equality"
  (= __ true)
```

to:

```clojure
  "We shall contemplate truth by testing reality, via equality"
  (= true true)
```

Save the file, we should now see the koan runner move on to the next problem.
Before tomorrow try to get through this list of koans:

```
01_equalities.clj
02_lists.clj
03_vectors.clj
04_sets.clj
05_maps.clj
```
