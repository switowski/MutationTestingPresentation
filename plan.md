## Presentation planning (slide by slide):
* Opening slide (title, my name?)
* Do you write tests ?
* Do you test your tests ? (yo dawg picture)
* Do you test the test for your tests (just kidding) (yo dawg picture surprised)
* There is a technique proposed first in 1970 by Richard Lipton (a picture of Lipton, no, the other Lipton) that is called mutation testing
* So, how does it work ? You start by changing your program in a small ways.
* How small changes are we talking about ? Well, very small:
    + Changing the operator or a value in a mathematical equation (changing + with -)
    + statement duplication
    + changing true to false in boolean expressions
    + replacing one variable with other
    + more here: https://en.wikipedia.org/wiki/Mutation_testing#Mutation_operators
  So the changes should be similar to small programming errors that each of us do.
* After that you run your tests and see what happens. There are two possibilities (diagram of mutation_schema.png):
* Your tests start failing (yaay !), which means that they reached the modified code and:
    + either died with some error message, because the test didn't expect that - this means that you test is good and in case something changes, you will notice it (this is called weak mutation testing).
    + or the test code detected the change properly and gave you assertion error - which means that you code is even better (this is called strong mutation testing and it's more powerful - it means that tests are actually catching the possible problems, but it's more difficult to achieve).
* Or your tests are still working which is bad because it can mean one of two things:
    - The code that was mutated (changed) was never executed (so you have dead code, which is even worse that bad tests)
    - Your tests can't detect the faults that was introduced during the mutation (so your tests are bad)
* After all tests have been test, you will get the mutation score =  number of mutants killed / total number of mutants. Instead of having 100% of test coverage (as for normal tests), here we are aiming to have 100% of mutants killed.
* And that's it, it's that simple.
* Let me show you an example:
* You write a simple calculator.py program:
    def multiply(a, b):
        return a * b
* And you write the simplest test that is possible:
    from unittest import TestCase
    from calculator import multiply

    class CalculatorTest(TestCase):
        def test_multiply(self):
            self.assertEqual(multiply(2, 2), 4)
* Who can tell me, why this test is wrong ?
* Right, you can replace multiplication operator with addition operator (or even the power operator) and this test will still pass. If you run a mutation testing tool, it will try to replace multiplication operators with various other operators or statements. If the tool is good, it should detect the mutant that survived and inform you about that.
* So now we know that we need to improve our test:
    class CalculatorTest(TestCase):
        def test_multiply(self):
            self.assertEqual(multiply(3, 3), 9)
* So this is how mutation testing can help you improve your tests
* But where is the catch (why_are_we_not_funding_this.jpg)?
* Well, I'm glad you asked !
* One of the main problem with mutation testing is called 'Equal Mutant Problem' - it means that some operations can create a mutant that you won't be able to detect with any tests, for example look at this function:
    index = 0
    while True:
        do_stuff()
        index = index + 1
        if index == 10:
            break
If the mutation replaces the "==" operator with ">=" the code will still act the same and there is no test to detect this mutation. Unfortunately there is no easy way to automate the process of 'Equal Mutants' detection.
* So to wrap up:
* The good parts of mutation testing:
    - They can help you detect problem with your tests
    - They might also discover some dead code in you software
    - They are usually automated (there are SOME libraries can be used for automating the process) - nobody is going to write mutation testing by hand since it's too tedious.
    - How else would you test your tests (when you don't have access to code reviews or pair programming) ?
* What are the downsides ?
    - Mutation testing is very slow - you apply one modification at a time, then you run whole test suite, then you apply next modification. You might need thousands of mutants to test even a simple application.
    - There are SOME libraries but not so many as for example for normal unit testing and often not so well maintained anymore. It's not a very popular technique (not everyone is even testing their code, so the amount of people testing their tests is even lower) and everyone's code is different, so it's way more difficult to write good library for mutant testing that will work for everyone.
    - The aforementioned Equal Mutants Problems makes it even more difficult for mutant testing.
    - Writing complex mutant tests (beyond simple operators changing or variables replacement) is difficult.
* Examples of libraries for mutation testing in various languages
* Thank you, any questions ?