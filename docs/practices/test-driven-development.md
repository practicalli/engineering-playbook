# Test Driven Development

Test driven development is a technique to help write the correct code and write it well.

TDD is design tool to support understanding of thebehaviour the software should satisfy.  The ability to test the code is a consequence of this design activity.

## TDD approach

Start by writing a single test representing a single piece of behaviour, an assumption of the understanding so far gathered from the requirements or other communications.  

Behaviour defined through a test provides a specific constraint to focuses the implementation of code that satisfies the test.

## TDD basic steps

1. Write a test
    - this may require creation of a non-functioning class or mock object to compile the test

2. Run the test
    - the test should fail, go red, so that you know it is testing something

3. Write the minimum amount of code in your class to make the test pass
    - write your code as simple as possible, but no simpler - leave options open to last responsible moment

4. Run the tests
    - the test should pass, go green

5. Write another test / refactor code

## Tools

Unit Testing frameworks
JUnit is the most common unit test framework for Java development and is widely supported by IDE's, build tools, continuous integration servers and many other supporting tools.  Typically, when you create a new Java project in your IDE, JUnit is included as a project library or added when you create a JUnit Test class from your IDE menu.

Hamcrest is often used with JUnit to make the tests more human readable, great when you are coming to a new code base and are reading through lots of tests or when you are debugging code you wrote more than few days ago.

Hamcrest core is now included in JUnit since version 4.5 onwards.

## Learning TDD well

The best way to learn TDD well and feel comfortable with this technique is to practice, see the section on deliberate practice for ideas on how to practice TDD.

## Tips

Use meaningful test names
With JUnit 4.x you use the @Test Java annotation, meaning you no longer need to use the word test in the method name.  The test method name should represent what you are trying to test from the user or domain point of view.

A nice way to start the test is to use the word should, as this invites anyone looking at the test to consider if it represents meaningful behaviour and change or throw away the test if it does not add value.

Test one thing at once
Be wary about multiple assert statements in a test.  If you have multiple asserts in your test method, you may be too ambitious in what you are trying to test.  Or you may have a behaviour that needs further decomposition.

When refactoring code, those tests that have a number of assertions are more likely to break if they are testing several concepts within the same test.

Managing test data
Each test will reset the state - that's the way JUnit works (nUnit maintains state, so has to be cleared manually)

## TDD Kanban

![TDD Kanban](TDD-Kanban.png){align=right loading=lazy style="height:150px;width:150px"}

To help keep you in a good flow when you are following the test first approach, it can be useful to use a simple kanban board to manage your flow.

The kanban board has three lanes as follows

- Test - you are writing a single test
- Code - you are writing code to pass a particular test that is failing
- Refactor - you are changing the internal workings of your code

You only have one card on your kanban board (this is your work in progress limit), this reminds you which activity you should be working on and should help you get into the test-code-refactor routine.

Using the TDD Kanban board

To start, place your one and only card on the test lane of the kanban board.

Once you have written a failing test and run your tests, move the card on the kanban board to the code lane and write enough code to make the test pass.

Once you have written enough code to make the test pass (running all tests), move the card on the kanban board to the refactor lane.

When you have finished your refactor work and have run the tests, move the card back to the test lane and write another failing test.

## Resources

- [List of TDD Problems](https://sites.google.com/site/tddproblems/all-problems-1)
- [Effective exercises for teaching TDD](https://gojko.net/2010/04/19/effective-exercises-for-teaching-tdd/)
