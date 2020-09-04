---
layout: post
title: Simple Notes of a Learning Experience about Clojure.
categories: [Clojure]
tags: [Clojure]
modified: 2020-09-04
---

#### Table of Content
* TOC
{:toc}

## Background

Due to the need of project in work, I need to learn Clojure basics, so can understand the code and compile it to JavaScript using Clourescript, then call the functions in JavaScript.


## Install Clojure

The following steps help setup Clojure environment under ``Ubuntu 20.04``, 
```bash
$ sudo apt update
$ sudo apt install rlwrap
$ sudo apt install default-jdk

$ curl -O https://download.clojure.org/install/linux-install-1.10.1.561.sh
$ chmod +x linux-install-1.10.1.561.sh
$ sudo ./linux-install-1.10.1.561.sh
```

Have a check,
```bash
$ clj 
Clojure 1.10.1
user=> 
```

## Clojure Basics

Numeric types
```bash
user=> 42
42
user=> -1.5
-1.5
user=> 22/7
22/7
```

Character types
```bash
user=> "hello"
"hello"
user=> \e
\e
user=> #"[0-9]"
#"[0-9]"
```

Symbols and idents
```bash
user=> map
#object[clojure.core$map 0x3301500b "clojure.core$map@3301500b"]
user=> +
#object[clojure.core$_PLUS_ 0x6e9c413e "clojure.core$_PLUS_@6e9c413e"]
user=> clojure.core/+
#object[clojure.core$_PLUS_ 0x6e9c413e "clojure.core$_PLUS_@6e9c413e"]
user=> nil
nil
user=> true
true
user=> false
false
user=> :alpha
:alpha
user=> :release/alpha
:release/alpha
```

Literal collections
```bash
user=> '(1 2 3)                                                                  
(1 2 3)
user=> [1 2 3 4]
[1 2 3 4]
user=> #{1 2 3 4}
#{1 4 3 2}
user=> {:a 1, :b 2}
{:a 1, :b 2}
```

Clojure Evaluation
* The reader read character source from `.clj` files or series of experssions interactively.
* The reader produces Clojure data.
* The compiler produces the bytecode form the JVM.
* JVM runs and takes effect.

Delaying evaluation with quoting
```bash
user=> '(1 2 3)
(1 2 3)
user=> x
Syntax error compiling at (REPL:0:0).
Unable to resolve symbol: x in this context
user=> 'x
x
user=> '(1 2 3)
(1 2 3)
user=> (1 2 3)
Execution error (ClassCastException) at user/eval19 (REPL:1).
class java.lang.Long cannot be cast to class clojure.lang.IFn
```

REPL (Read-Eval-Print-Loop)
* Read an expression (a string of characters).
* Evaluate the expression and yield Clojure data.
* Convert data back to characters and print the result.
* Loop back to the beginning.

Access docs
```bash
user=> (doc +)
-------------------------
clojure.core/+
([] [x] [x y] [x y & more])
  Returns the sum of nums. (+) returns 0. Does not auto-promote
  longs, will throw on overflow. See also: +
nil

user=> (find-doc "trimr")
-------------------------
clojure.string/trimr
([s])
  Removes whitespace from the right side of string.
```

List functions
``` bash
user=> (dir clojure.repl)
apropos
demunge
dir
dir-fn
doc
find-doc
pst
root-cause
set-break-handler!
source
source-fn
stack-element-str
thread-stopper
nil
```

def
```bash
user=> (def x 7)
#'user/x
user=> x
7
```

printing
```bash
user=> (println "The value of x: " x)
The value of x:  7
nil
user=> (prn "The value of x: " x)
"The value of x: " 7
nil
user=> (print "The value of x: " x)
The value of x:  7nil
user=> (pr "The value of x: " x)
"The value of x: " 7nil
```

Knowledge Test
```bash
user=> (+ 7654 1234)
8888
user=> (/ (+ 7 (* 3 4) 5) 10)
12/5
user=> (doc rem)
-------------------------
clojure.core/rem
([num div])
  remainder of dividing numerator by denominator.
nil
user=> (doc mod)
-------------------------
clojure.core/mod
([num div])
  Modulus of num and div. Truncates toward negative infinity.
nil
```


**Reference**

[1] [https://clojure.org/guides/learn/syntax](https://clojure.org/guides/learn/syntax)
