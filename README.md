# nuXmv solves "Die Hard 3" water jug problem

Leslie Lamport does an example with TLA+ showing how you can
get TLA+ to solve the water jug problem from the movie "Die Hard 3".
The basic problem is that you have a 3-gallon and a 5-gallon water jug.
You have to get exactly 4 gallons into the 5-gallon jug. You can fill or
empty either jug at any time, and you can pour one jug into the other until
either the source jug is empty, or the destination jug is full, but you can't
try to measure things in any other way.

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
