# Command–query separation

every method should either be a command that performs an action, or a query that returns data to the caller, but not both. In other words, asking a question should not change the answer.[1] More formally, methods should return a value only if they are referentially transparent and hence possess no side effects.

[Bertrand Meyer - Wikipedia](https://en.wikipedia.org/wiki/Bertrand_Meyer){target=_blank .md-button}


Command-query separation is particularly well suited to a design by contract (DbC) methodology, in which the design of a program is expressed as assertions embedded in the source code, describing the state of the program at certain critical times. 

In DbC, assertions are considered design annotations—not program logic—and as such, their execution should not affect the program state. CQS is beneficial to DbC because any value-returning method (any query) can be called by any assertion without fear of modifying program state.

In theoretical terms, this establishes a measure of sanity, whereby one can reason about a program's state without simultaneously modifying that state. In practical terms, CQS allows all assertion checks to be bypassed in a working system to improve its performance without inadvertently modifying its behaviour. CQS may also prevent the occurrence of certain kinds of heisenbugs.

[Design By Contract](https://en.wikipedia.org/wiki/Design_by_contract){target=_blank .md-button}


