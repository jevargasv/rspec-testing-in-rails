# RSpec + Rails Testing
A breakdown of how to test in Ruby on Rails and why it's important

## What is a test and why should we test?
Tests verify that your code behaves correctly and produces your desired result. It should set up a state with a known result in order to compare the expectation to the actual behavior. 

Example:
You want to write code to validate the presence of an email…
Or make sure a user’s password includes certain special characters...
Or make sure a user’s age is being calculated correctly...

Proper testing relies on careful pseudocoding which accounts for edge-cases and specifically defines what counts as a successful output.

### Basic Procedure:
1. Write the smallest and possible test case that matches the program’s needs.
2. Run several tests through many trials and errors with the chance of rewriting the correct code to make it work.
3. Rewrite the correct code to make the test run successfully.
4. Run the test suite and repeat Steps 3 and 4 until all tests pass.
5. Finally, refactor the code by making sure that it is simple and clear with no errors while running the tests successfully by keeping the test suite green.

## What tools can we use?
By default, every Rails application has three environments: development, production, and test. It’s created every time you run “rails new,” and is referred to as MiniTest.
The contents of this directory:

Each folder is designed to hold tests for that specific feature.
“Controllers” holds tests for controllers, routes, and views. 
“Integration” is meant to hold tests for interactions between controllers.

## How it works
Say you were making a sign-up form and want to make sure someone doesn’t submit an invalid email or blank name field.
Create the model/migrate:
	$ rails g model User name:string email:string
Create the test:
#test/models/user_test.rb

3. Run the test with — $ rake. Note that three tests were run but zero assertions were made. We have to add assertions:

4. If we $ rake again we should still get errors, but they should point us to what we forgot… 

## Alternatives: Rspec
Rspec is the largest and most well-maintained alternative to MiniSpec. Latest version was 3.8 released earlier this month (August 2018). It focuses on readability and composability of tests, using more plain English in naming tests and test groups. It was created for behavior-driven development, an evolution of its predecessor, TDD (test-driven development).

The main idea is to write tests as specifications of system behavior. It means that it has different 
methods of approaching the same problem which helps engineers think more clearly and make sure their refactoring is successful each step of the way. Because it can be very easy to get stuck and get lost on unnecessary code with no plans at all.

This is an example written with test/unit in Ruby on Rails:

With RSpec, it gets very detailed and clear to understand its behavior:


## How to Install and Interpret Rspec 
Install RSpec and run rspec --init to set up project to use Rspec
First line of test file, connects test to code
require_relative '../current_age_for_birth_year.rb'
The describe method in RSpec describes the method we are testing in the code file
describe "current_age_for_birth_year method" do
The it method in RSpec states an expectation or behavior of that method is used to only provide the description of what behavior is currently being tested
it "returns the age of a person based on the year of birth" do
Rspec reports on what is working and what is not and why
Common errors
NoMethodError: if the method was not defined
ArgumentError: if the method definition does not exist

## Conclusion

There are a ton of resources that you can use to learn more of the ins and outs of RSpec and testing. We’ve included links below. Understanding testing is important, as test-driven development is considered the most reliable methodology for delivering clean, quality code.



## Resources:
http://rspec.info/about/ 
http://rspec.info/documentation/
https://relishapp.com/rspec
https://learn.co/tracks/bootcamp-prep/ruby-fundamentals/methods/tdd-rspec-and-learn
https://semaphoreci.com/community/tutorials/getting-started-with-rspec
https://robots.thoughtbot.com/how-we-test-rails-applications
https://hackernoon.com/your-guide-to-testing-in-ruby-on-rails-5-c8bd122e38ad
