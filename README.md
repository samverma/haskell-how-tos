# COP 4020 Help Guide
You are probably a student over at the University of Central Florida and want some guidance on how to really make it through the class! Well look no more! This is a repository to help students find a curated list of questions and potential answers by the Spring 2016 class taught by Professor Wocjan.

Here is what our current topics are:

- [Installing Haskell (OSX)](#installing-haskell-osx)
- [Installing Haskell (Windows)](#installing-haskell-windows)
- [Installing Haskell (Linux)](#installing-haskell-linux)
- [How To Haskell](#how-to-haskell)
- [Testing By Hand](#testing-by-hand)
- [Testing Using Suites](#testing-using-suites)
- [Regular Expressions](#regular-expressions)

### Installing Haskell (OSX)
If you run OSX, you can install Haskell simply by running an awesome and simple command through [Homebrew](http://brew.sh/)(if you don't have this now, please get it because this will do a great job for you as far as package manager)

`brew install ghc cabal-install`

This should install both `ghc` and the dependency `cabal-install`. (**Do not perform the deprecated command `brew install haskell-platform`.** It will refer you back to the previous command).

If you wish to not have Homebrew, you can also install the latest [Haskell Platform](https://www.haskell.org/platform/).

### Installing Haskell (Windows)
**UNVERIFIED**

Simply download the appropriate installer from the [Haskell Platform website](https://www.haskell.org/platform/#windows). Run the installer and then you can launch the WinGHCi application! (**NOTE: I do not own a Windows machine, therefore I cannot guarantee the installation will work properly, so be sure to do your own research if not able to properly work. If you own a Windows machine, feel free to fork and set up a Pull Request.**)

### Installing Haskell (Linux)
You should be able to find the appropriate installation commands from the [Haskell Platform website](https://www.haskell.org/downloads/linux) as far as the Haskell bundle necessary. It has a list of commands for Ubuntu and Fedora and Arch Linux. It also has guided instructions as far as manually setting up Haskell as well (however, I would strongly suggest utilizing the packages that are already curated for you).

### How to Haskell
We will go through this portion in baby steps and will expand within the Wiki later on but for now let's take a brief look at what Haskell can do for us.

As we already know, Haskell is a purely functional language. Languages that are like C, which is completely imperative, mean that they execute instructions as a sequence and **change states**. However, in functional languages, you are really just telling what a variable **is**. Really what happens is things are **transforming**. Haskell is also **statically typed** meaning that you cannot combine data of two different types. It also is good at guessing what type a piece of data is and allows you to write your code a bit more generally.

From here, I actually am going to utilize an example from Professor Wocjan until I can write more to make things more understandable. You can find the code [here](https://bitbucket.org/schneider128k/2016-spring-cop4020/src/5994bdf94842c95959de801c36a1bd82e06f3d79/1_lectures/basic-haskell/0_basic-stuff/Maximum.hs?at=master&fileviewer=file-view-default) but I am also going to go out of my way to explaining it piece by piece.

You should code this up in your favorite text editor.

```haskell
module Maximum where

import Prelude hiding (maximum)

maximum :: (Ord a) => [a] -> a
maximum []      = error "empty list"
maximum [x]     = x
maximum (x:xs)  = max x (maximum xs)

-- maximum [1,2,3]
-- -> max 1 (maximum [2,3])
-- -> max 1 (max 2 (maximum [3]))
--
```

The biggest thing about this code before we get started is that this particular methodology of writing the different states that it could appear as, is the pattern matching technique. It is a way to sort of write out all the cases that you wish for the function to handle.

```haskell
module Maximum where
-- We are letting Haskell know that we are within a module called `Maximum`
-- and that we are going to be giving more instructions after this
```
```haskell
import Prelude hiding (maximum)
-- From my understanding it helps remove ambiguity from the Haskell
-- compiler as it will bring about a compile-time error
-- This is especially necessary since we are technically redefining
-- the maximum function that already exists.
```
```haskell
maximum :: (Ord a) => [a] -> a
-- Essentially our function, we are telling it what it needs and
-- what it is being constrained by. So we are telling the compiler
-- `a` needs to be constrained to be a value that has some ability of
-- being 'Ordered'. So it can be something >, <, == .
--
-- From there we are saying that there is a list of `a` as far as an
-- input and it should return a single ordered `a`
```
```haskell
maximum []      = error "empty list"
-- Here we are performing our first test, seeing whether or not
-- the list is empty, if so, throw an error and say the list is empty.
```
```haskell
maximum [x]     = x
-- Here we are telling the program another case, we are saying:
-- If there is a single value within in our list, by definition
-- it is considered to be the maximum value.
```
```haskell
maximum (x:xs)  = max x (maximum xs)
-- Here is what we really care about though and this is probably
-- going to be the hardest thing to explain. We are going to have a
-- list combined of the head (x) and the rest of the list (xs) and
-- we are going to compare the current head and the rest of the list
-- until we can find a maximum and so this actually uses a tad of
-- recursion to figure out. But it will keep stepping through to
-- find the maximum of the list.

-- Now you are probably asking why would we want it this way, well
-- really, this is just an implementation of the function that
-- already exists within Haskell and so this is just a demonstration -- of how the language works.

-- However, notice, we do not have any variables in this program.
-- It is all just a state from what we were given. It just has a
-- different representation for it.
```

### Testing By Hand
This is probably going to be one of the most referred sections here beyond the explanations that I will try to work on (or if you feel so inclined you can do yourself, and help the class).

In Haskell, really the best things we can do is test our code by ourself and understand how and why something might not work. If you want to try it out in the interactive mode just open up your `ghci` where it is through an application or terminal. and it will bring up some variation of:
```haskell
GHCi, version 7.10.3: http://www.haskell.org/ghc/  :? for help
Prelude>
```
Let's say I have the script from earlier and if you close this repo down it will be `maximum.hs`, you can then run this new command:
```haskell
GHCi, version 7.10.3: http://www.haskell.org/ghc/  :? for help
Prelude> :l maximum.hs
```
So here we notice that the `:l` command loads in whatever script we want to give it and it just finds it from the current directory (we can access files from other directories as well similar to how we navigate through terminal).
And it will now look like:
```haskell
GHCi, version 7.10.3: http://www.haskell.org/ghc/  :? for help
Prelude> :l maximum.hs
[1 of 1] Compiling Maximum          ( maximum.hs, interpreted )
Ok, modules loaded: Maximum.
*Maximum>
```

You can also do the same thing by just simply having `ghci maximum.hs` and it will load up the script into `ghci` along with it booting up.
```haskell
GHCi, version 7.10.3: http://www.haskell.org/ghc/  :? for help
[1 of 1] Compiling Maximum          ( maximum.hs, interpreted )
Ok, modules loaded: Maximum.
*Maximum>
```

As you can notice now, you see the `Prelude> ` has now changed to `*Maximum> `. This shows we are in the proper location for our testing. And so we can run the `maximum` function with any **lists** of *Ordering*.
```haskell
*Maximum> maximum [1,4,3,7,22,3]
22
```
Notice what happens when we do not have a list:
```haskell
*Maximum> maximum 4

<interactive>:4:1:
    Non type-variable argument in the constraint: Num [a]
    (Use FlexibleContexts to permit this)
    When checking that ‘it’ has the inferred type
      it :: forall a. (Num [a], Ord a) => a
```
It whines and says that we do not have a correct value in here for our function to understand. This is because we gave it a single value which is different from when we give `[4]`:
```haskell
*Maximum> maximum [4]
4
```

This is a methodology you can do for testing your functions correctness on the fly. You just load the appropriate modules and then interact with the Haskell as you go. You also can use the `:r` command to reload your scripts to allow for it to receive changes you made into them.

### Testing using Suites
Now you probably are here for this one out of everything else. A simple way of testing your code with what Professor Wocjan is to put all of the necessary files in the same directory (an example of this can be seen in [testing](https://github.com/maxcell/haskell-how-tos/tree/master/testing)).

The biggest thing we need as far as making progress in testing is reading what is necessary for our particular test file. We notice that we were given an import file `Testing.lhs`. This is the backbone to all our testing in this class. We need to be sure to at least have this file in the same directory as whatever file we are given as far as a test.

If you check the testing file that you are trying to use so, for instance `PolygonAreaTests.hs`. You can see at the top it is trying to import these modules:
```haskell
import PolygonArea
import Testing
import FloatTesting
```
This means we need to be sure to have these loaded in properly or have them in the same directory to allow Haskell to do our testing. So we want to be sure that the `testing/` directory has all of these files so `testing/PolygonArea.hs`, `testing/Testing.lhs`, `testing/FloatTesting.hs` are within the same folder and then we can just move over to that directory.

So once you move everything within the same directory or perform your preferred method of loading in modules whether it is `ghci <module>` or loading up `ghci` and running `:l <module>`, you should see:
```haskell
GHCi, version 7.10.3: http://www.haskell.org/ghc/  :? for help
[1 of 4] Compiling Testing          ( Testing.lhs, interpreted )
[2 of 4] Compiling FloatTesting     ( FloatTesting.hs, interpreted )
[3 of 4] Compiling PolygonArea      ( PolygonArea.hs, interpreted )
[4 of 4] Compiling PolgonAreaTests  ( PolygonAreaTests.hs, interpreted )
Ok, modules loaded: PolgonAreaTests, PolygonArea, Testing, FloatTesting.
*PolgonAreaTests>
```
And now we can see some of the stuff within the file that has been made for us to test with:
```haskell
*PolgonAreaTests> square
[(1.5,0.0),(3.5,0.0),(3.5,2.0),(1.5,2.0)]
*PolgonAreaTests> triangle
[(1.566,3.0),(0.0,0.0),(2.0,0.0)]
*PolgonAreaTests> pent
[(0.0,0.0),(1.0,-2.5),(10.2,20.0),(5.0,15.0),(1.0,30.0)]
*PolgonAreaTests>
```
So we can run our test manually through each one:
```haskell
*PolgonAreaTests> computeArea square
4.0
*PolgonAreaTests> computeArea triangle
3.0
*PolgonAreaTests> computeArea pent
116.75
```
Or you can do the efficient way and see:
```haskell
*PolgonAreaTests> main

Testing PolygonAreaTests v1.2...
Trying 'computeArea []' to see if it throws an exception...
No error, because: Error.
Trying 'computeArea [(0x600d,0xf00d)]' to see if it throws an exception...
No error, because: Error.
4.0
      ~=~ 4.0
3.0
      ~=~ 3.0
2.0
      ~=~ 2.0
21.0
      ~=~ 21.0
165.75
      ~=~ 165.75
116.75
      ~=~ 116.75
149.40800000000002
      ~=~ 149.408
3.1415719827794306
      ~=~ 3.1415719827794306
Finished with 0 failures!
```

### Regular Expressions
A beast of enormous proportions. The big baddy but is useful to us all whether we recognize it or not.

Regular expressions are what we use to keep us from accepting strings that could
damage our systems, [Bobby Droptables](https://xkcd.com/327/) would disagree though.

With Haskell, even though we have a library for regular expressions in existence we
actually are going to be breaking down piece by piece.

Referring to [Professor Wocjan's Regular Expression base](https://bitbucket.org/schneider128k/2016-spring-cop4020/src/db8292a754cd00d0cd4d8606dca233e0803f2115/1_lectures/basic-haskell/3_higher-order-func/RegularExpressions.hs?at=master&fileviewer=file-view-default):

```haskell
import Prelude hiding ((<*>))

type RegExp = String -> Bool

epsilon :: RegExp
epsilon = (== "")

char :: Char ->  RegExp
char ch = (== [ch])

(|||) :: RegExp -> RegExp -> RegExp
(|||) e1 e2 =
  \s -> e1 s || e2 s


(<*>) :: RegExp -> RegExp -> RegExp
(<*>) e1 e2 =
  \s -> or [ e1 prefix && e2 suffix | (prefix,suffix) <- splits s]

(<**>) :: RegExp -> RegExp -> RegExp
(<**>) e1 e2 =
  \s -> or [ e1 prefix && e2 suffix | (prefix,suffix) <- drop 1 (splits s)]

star :: RegExp -> RegExp
star p = epsilon ||| (p <**> star p)
```

Each of these things are going to be useful for us to performing regular expressions. I would like to preface this with, you should probably reference
this document if you intend to read through it all. Just to be safe. I also would
like to say that these are just ways of showing you how each piece is used and
how it can be utilized. It does not explain how to do the rest of the homework.

This is relatable to DFAs and regular expressions that can be seen by Computer Science students in both Discrete 2 and System Software.

```haskell
import Prelude hiding ((<*>))
-- It is hiding the original definition to the (<*>), which actually through kcolley's
-- explanation is why there is <*> and <**>, you should not need or use <*> unless
-- it is through its original intention. However, if you don't want to risk anything
-- just keep this and the new definition in as it is used in the testing
```

```haskell
type RegExp = String -> Bool
-- This is the crux of why it all works. Here we are describing this type that
-- we want to exist. We want to make RegExp be a transformation of String to Bool
-- It is a function that will return true if the string is accepted and will return
-- false otherwise
```

```haskell
epsilon :: RegExp -- Is simply a RegExp
epsilon = (== "")
-- This is the definition for epsilon, the empty string. For some definitions, you
-- will need to account for the pattern and the empty string (epsilon).
-- Essentially it will test for if the given input is empty, if so, it will return
-- True, otherwise it will go and return False.
-- We can test this if it is true by calling our pattern and giving it an input:

-- We also want to be sure to set our pattern p so that way we can get it to
-- output the correct value. This will be better shown by more examples

-- We define our regular expression to only accept the empty string or epsilon
let pattern = epsilon -- ε

pattern ""  -- True
pattern "a" -- False
pattern "The meaning of life is 42."  
-- Though the regular expression may be False, doesn't necessarily make my statement
-- False
```

```haskell
char :: Char ->  RegExp -- Takes a Char and returns a RegExp
char ch = (== [ch])
-- This is the definition for any element that we can take in at its basic roots,
-- a character. This will check if we have any character as input and will return
-- True if so, otherwise False.

-- We define our regular expression to only accept strings of only 'a'
let pattern = char 'a'  -- a

pattern ""  -- False
pattern "a" -- True
pattern "b" -- False
pattern "aaa" -- False  
```

```haskell
(|||) :: RegExp -> RegExp -> RegExp -- It takes to RegExps and returns one RegExp
(|||) e1 e2 =
  \s -> e1 s || e2 s
-- This actually is trying to say the ||| operator. Which is equivalent to our OR
-- (but don't tell Dr. Gerber that I said that one). It will essentially allow us
-- to say that either pattern can be accepted but it has to be one or the other,
-- exclusively.

let pattern = char 'a' ||| char 'b' -- (a | b)

pattern ""  -- False
pattern "a" -- True
pattern "b" -- True
pattern "ab" -- False
pattern "aa" -- False
pattern "bb" -- False

-- Let's see what happens with it when we use epsilon though
let pattern = epsilon ||| char 'a' -- (ε | a)
pattern ""  -- True
pattern "a" -- True
pattern "b" -- False
pattern "ab" -- False

-- The key thing to note about this operator is that it only accepts two RegExs
-- at a time. What if we were to combine them....
let pattern = (epsilon ||| char 'a') ||| char 'b' -- (ε | a) | b
pattern ""  -- True
pattern "a" -- True
pattern "b" -- True
pattern "ab" -- False
```

```haskell
(<**>) :: RegExp -> RegExp -> RegExp
(<**>) e1 e2 =
  \s -> or [ e1 prefix && e2 suffix | (prefix,suffix) <- drop 1 (splits s)]

-- Ignoring the <*> operator as we will just use <**> to do the same thing but
-- better. This operator is actually going to concat two patterns together
let pattern = char 'o' <**> char 'x' -- ox
pattern ""   -- False
pattern "o"  -- False
pattern "x"  -- False
pattern "ox" -- True

let pattern = ((char 'r') <**> (char 'a' ||| char 'u')) <**> char 'n' -- r(a | u)n
pattern "" -- False
pattern "read" -- False
pattern "ran" -- True
pattern "run" -- True
pattern "raun" -- False
```

Now for `star` this will really take everything we have done and put it together:
```haskell
star :: RegExp -> RegExp
star p = epsilon ||| (p <**> star p)

-- So what we are saying is we will accept a pattern BUT to know if it is valid
-- We will take the pattern concat with the function call of pattern again OR
-- be stopped by the empty string, while also being able to be the empty string

let pattern = star (char 'a') -- a*
pattern "" -- True
pattern "a" -- True
pattern "aaaaaaaa" -- True
pattern "abaaaa" -- False

-- And with star we can do a bit more complex ones
let pattern = ((char 'l' <**> char 'o') <**> star (char 'o')) <**> char 'l' -- ((lo)o*)l
pattern "" -- False
pattern "l" -- False
pattern "ll" -- False
pattern "lol" -- True
pattern "looooooooooool" -- True
pattern "lololol" -- False
```

Now you saw that with making sure we could do `lol` and not have `ll` we had to
have an extra `o` in the expression. This is what the `plus` symbol will take
care for us with, making sure we have at least `1` occurrence or as many as we want. And the `option` will allow us to define `0` or `1` occurrences.

We can utilize these functions to defining `number` and `fractional` as well. However, we will also need to be sure to avoid returning any functions as their definitions are similar to epsilon and that we aren't transforming anything but a `String -> Bool`, which is simply `RegExp`.
