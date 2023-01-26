# Solving the "Die Hard 3" water jug problem with

# nuVmx and Formula

Leslie Lamport does an example with TLA+ showing how you can
get TLA+ to solve the water jug problem from the movie "Die Hard 3".
The basic problem is that you have a 3-gallon and a 5-gallon water jug.
You have to get exactly 4 gallons into the 5-gallon jug. You can fill or
empty either jug at any time, and you can pour one jug into the other until
either the source jug is empty, or the destination jug is full, but you can't
try to measure things in any other way.

## NuXmv

When I first implemented this in nuXmv, there was some weirdness about
trying to do an operation while also trying to pick the next operation, so
I added a state variable so that you are either picking an action or
executing it.

To run this, you need nuXmv from [https://nuxmv.fbk.eu/pmwiki.php]
Then just run:

```
nuxmv diehard.xmv
```

Here is the counter-example output from nuXmv showing the solution:

```
Trace Description: LTL Counterexample
Trace Type: Counterexample
  -- Loop starts here
  -> State: 1.1 <-
    bucket3 = 0
    bucket5 = 0
    action = empty3
    state = choosing_action
  -> State: 1.2 <-
    action = fill5
    state = executing_action
  -> State: 1.3 <-
    bucket5 = 5
    state = choosing_action
  -> State: 1.4 <-
    action = pour5into3
    state = executing_action
  -> State: 1.5 <-
    bucket3 = 3
    bucket5 = 2
    state = choosing_action
  -> State: 1.6 <-
    action = empty3
    state = executing_action
  -> State: 1.7 <-
    bucket3 = 0
    state = choosing_action
  -> State: 1.8 <-
    action = pour5into3
    state = executing_action
  -> State: 1.9 <-
    bucket3 = 2
    bucket5 = 0
    state = choosing_action
  -> State: 1.10 <-
    action = fill5
    state = executing_action
  -> State: 1.11 <-
    bucket5 = 5
    state = choosing_action
  -> State: 1.12 <-
    action = pour5into3
    state = executing_action
  -> State: 1.13 <-
    bucket3 = 3
    bucket5 = 4
```

## Formula

I also implemented this in Formula. You can get Formula
here: [https://github.com/VUISIS/Formula], where there are
also instructions for running it.

I have Formula pre-compiled to an executable, so I just type
`formula` to run it. To solve the puzzle, I ask formula to locate
a CurrState instance where bucket3 can be anything, but
bucket5 is 4.
To run the original Formula version, I do this:

```
[]> l DieHard.4ml
    (Compiled) DieHard.4ml
0.28s.
[]> qr StartFromEmpty CurrState(_,4)
    Parsing text took: 0
Visiting text took: 0
Started query task with Id 0.
0.04s.
[]> pr 0

_Query_0000000000000000.requires :- /Users/mark/vandy/6315/solving-diehard3-water-jugs/~Query (2, 0)
  ~dc0 equals
   _Query_0000000000000000.~requires0 :- /Users/mark/vandy/6315/solving-diehard3-water-jugs/~Query (2, 0)
     ~dc0 equals
      CurrState(3, 4) :- /Users/mark/vandy/6315/solving-diehard3-water-jugs/DieHard.4ml (6, 2)
        ~dc0 equals
         CurrState(0, 4) :- /Users/mark/vandy/6315/solving-diehard3-water-jugs/DieHard.4ml (14, 2)
           ~dc0 equals
            CurrState(3, 4) :- /Users/mark/vandy/6315/solving-diehard3-water-jugs/DieHard.4ml (23, 2)
              ~dc0 equals
               CurrState(2, 5) :- /Users/mark/vandy/6315/solving-diehard3-water-jugs/DieHard.4ml (10, 2)
                 ~dc0 equals
                  CurrState(2, 0) :- /Users/mark/vandy/6315/solving-diehard3-water-jugs/DieHard.4ml (23, 2)
                    ~dc0 equals
                     CurrState(0, 2) :- /Users/mark/vandy/6315/solving-diehard3-water-jugs/DieHard.4ml (14, 2)
                       ~dc0 equals
                        CurrState(3, 2) :- /Users/mark/vandy/6315/solving-diehard3-water-jugs/DieHard.4ml (23, 2)
                          ~dc0 equals
                           CurrState(0, 5) :- /Users/mark/vandy/6315/solving-diehard3-water-jugs/DieHard.4ml (10, 2)
                             ~dc0 equals
                              CurrState(0, 0) :- ? (0, 0)
                              .
                           .
                        .
                     .
                  .
               .
            .
         .
      .
   .
.
```

Notice that even after it first gets to a state where bucket5 has
4 gallons in it, it keeps going. I created a second version called
DieHardCheck4.4ml that includes guards to not create a new CurrState
if bucket5 already has 4 gallons in it, and this is the output:

```
❯ formula

[]> (Compiled) DieHardCheck4.4ml
0.28s.
[]> Parsing text took: 1rrState(_,4)
Visiting text took: 0
Started query task with Id 0.
0.04s.
[]> pr 0
_Query_0000000000000000.requires :- /Users/mark/vandy/6315/solving-diehard3-water-jugs/~Query (2, 0)
  ~dc0 equals
   _Query_0000000000000000.~requires0 :- /Users/mark/vandy/6315/solving-diehard3-water-jugs/~Query (2, 0)
     ~dc0 equals
      CurrState(3, 4) :- /Users/mark/vandy/6315/solving-diehard3-water-jugs/DieHardCheck4.4ml (23, 2)
        ~dc0 equals
         CurrState(2, 5) :- /Users/mark/vandy/6315/solving-diehard3-water-jugs/DieHardCheck4.4ml (10, 2)
           ~dc0 equals
            CurrState(2, 0) :- /Users/mark/vandy/6315/solving-diehard3-water-jugs/DieHardCheck4.4ml (23, 2)
              ~dc0 equals
               CurrState(0, 2) :- /Users/mark/vandy/6315/solving-diehard3-water-jugs/DieHardCheck4.4ml (14, 2)
                 ~dc0 equals
                  CurrState(3, 2) :- /Users/mark/vandy/6315/solving-diehard3-water-jugs/DieHardCheck4.4ml (23, 2)
                    ~dc0 equals
                     CurrState(0, 5) :- /Users/mark/vandy/6315/solving-diehard3-water-jugs/DieHardCheck4.4ml (10, 2)
                       ~dc0 equals
                        CurrState(0, 0) :- ? (0, 0)
                        .
                     .
                  .
               .
            .
         .
      .
   .
.
```
