# RSpec + Rails Testing
A breakdown of how to test in Ruby on Rails and why it's important

## What is a test and should we do it?
Tests verify that your code behaves correctly and produces your desired result. It should set up a state with a known result in order to compare the expected result to the actual behavior. 

Example:
If you have code for a form that includes email, password, and age input, you want to test to:
*validate the presence of an email
*user’s password includes specific special characters
*user’s age is being calculated correctly

We should test to deliver quality code, which doesn’t break and is easy for other developers to understand.

### Basic Procedure:
1. Write the smallest possible test case that matches the program’s needs.
2. Run several tests to try and catch every possible edge case.
3. Attempt to correct errors wherever they appear in the app. Run tests again.
4. Run the test suite and repeat Steps 2 and 3 until all tests pass.
5. Continue to develop and refactor, running old tests and creating new ones to make sure you're not breaking anything as you add new functionality.

![alt-text](https://media.giphy.com/media/igDIvcIMMGIne/giphy.gif)

## What tools can we use?
By default, every Rails application has three environments: development, production, and test. They're created every time you run “$rails new." Rails' default testing suite is referred to as MiniTest.

Find and review of the the contents of the test folder by running: 
```
$ ls -F test
```
Response:
```
controllers/           helpers/               mailers/               system/                test_helper.rb
fixtures/              integration/           models/                application_system_test_case.rb
```
Each folder is designed to hold tests for that specific feature.

For example:
- “Controllers” holds tests for controllers, routes, and views. 
- “Integration” is meant to hold tests for interactions between controllers.

## DEMO: How it works
Say you were making a sign-up form and want to make sure someone doesn’t submit an invalid email or blank name field. Here's how you would build the appropriate test to make sure you're validating correctly:

1. Create the app a new app and model:
	```
	$ rails new testapp -d postgresql
	$ rails g model User name:string email:string
	```
	
2. Create 3 tests to 1) make sure a proper user sign-up comes up valid, 2 & 3) make sure the user is invalid if they forgot to fill out either the name or email forms:

#test/models/user_test.rb
```
require 'test_helper'

class UserTest < ActiveSupport::TestCase
  # test "the truth" do
  #   assert true
  # end

  test 'valid user' do
  end

  test 'invalid without name' do
  end

  test 'invalid without email' do
  end
  
end
```

3. Run a sample test: 
```
$rake.
```
Your response should look something like this:
```
Finished in 0.039215s, 76.5013 runs/s, 76.5013 assertions/s.
3 runs, 3 assertions, 2 failures, 0 errors, 0 skips
```
Note that three tests were run but zero assertions were made. That's what we need to do next: add assertions.

4. Fill out tests with assertions for actions we want to work and refutions for actions we don't want to allow. We'll also define a 'setup' to help DRY up our code:
```
require 'test_helper'

class UserTest < ActiveSupport::TestCase
  # test "the truth" do
  #   assert true
  # end

  def setup
    @user = User.new(name: 'John', email: 'john@example.com')
  end
  
  test 'valid user' do
    assert @user.valid?
  end

# This should 'assert' true, since we passed in all the proper information.

  test 'invalid without name' do
    @user.name = nil
    refute @user.valid?, 'saved user without a name'
    assert_not_nil @user.errors[:name], 'no validation error for name present'
  end
  
# We put 'refute' here, because if the person puts a blank name, our app should return an error. The optional comment will help explain to us what went wrong.

  test 'invalid without email' do
    @user.email = nil
    refute @user.valid?
    assert_not_nil @user.errors[:email]
  end
  
 # We used 'refute' here as well, because if the person puts an email field blank, our app should return an error.
 
end
```

4. Now if we $rake again we should still get errors, but they should point us to what we forgot: to add the proper validations to make sure we can't save users without a name.
```
F

Failure:
UserTest#test_invalid_w/o_name [/Users/tomxbarnesx/Documents/exs/rails/testing/testapp/test/models/user_test.rb:17]:
saved user without a name
```

## Alternatives: Rspec
Rspec is the largest and most well-maintained alternative to MiniSpec. The latest version was 3.8 released earlier this month (August 2018). It focuses on readability and composability of tests, using more plain English to compose them. It was created for behavior-driven development, an evolution of its predecessor, TDD (test-driven development).

The main idea is to write tests as specifications of system behavior. It means that it has different 
methods of approaching the same problem, which helps engineers think more clearly and make sure their refactoring is successful each step of the way.

Here is a shorthand example written with test/unit, which comes standard with Rails:
```
def test_making_order
  book = Book.new(:title => "RSpec Intro", :price => 20)
  customer = Customer.new
  order = Order.new(customer, book)

  order.submit

  assert(customer.orders.last == order)
  assert(customer.ordered_books.last == book)
  assert(order.complete?)
  assert(!order.shipped?)
end
```

With RSpec, it gets very detailed and clear to understand its behavior:
```
describe Order do
  describe "#submit" do

    before do
      @book = Book.new(:title => "RSpec Intro", :price => 20)
      @customer = Customer.new
      @order = Order.new(@customer, @book)

      @order.submit
    end

    describe "customer" do
      it "puts the ordered book in customer's order history" do
        expect(@customer.orders).to include(@order)
        expect(@customer.ordered_books).to include(@book)
      end
    end

    describe "order" do
      it "is marked as complete" do
        expect(@order).to be_complete
      end

      it "is not yet shipped" do
        expect(@order).not_to be_shipped
      end
    end
  end
end
```

## How to Install and Interpret Rspec 
```
require_relative '../current_age_for_birth_year.rb'

describe "current_age_for_birth_year method" do
  it "returns the age of a person based on the year of birth" do
    age_of_person = current_age_for_birth_year(1984)

    expect(age_of_person).to eq(19)
  end
end
```
- Install RSpec and run rspec --init to set up project to use Rspec
- First line of test file, connects test to code:
`require_relative '../current_age_for_birth_year.rb'`
- The describe method in RSpec describes the method we are testing in the code file
`describe "current_age_for_birth_year method" do`
- The `it` method in RSpec states an expectation or behavior of that method is used to only provide the description of what behavior is currently being tested:
`it "returns the age of a person based on the year of birth" do`
- RSpec reports what is working and what is not and why

#### Common errors
- NoMethodError: if the method was not defined
- ArgumentError: if the method definition does not exist

## Conclusion

There are a ton of resources you can use to learn more of the ins and outs of RSpec and testing. We’ve included links below. Understanding testing is important, as test-driven development is considered the most reliable methodology for delivering clean, quality code.

![alt text](https://media.giphy.com/media/LLR3cBvqxpamY/giphy.gif)

## Resources:
MiniTest:
- https://hackernoon.com/your-guide-to-testing-in-ruby-on-rails-5-c8bd122e38ad
- https://medium.com/@ethanryan/getting-started-with-testing-in-rails-using-minitest-and-rspec-113fe1f866a

RSpec:
- http://rspec.info/documentation/
- https://relishapp.com/rspec
- https://learn.co/tracks/bootcamp-prep/ruby-fundamentals/methods/tdd-rspec-and-learn
- https://semaphoreci.com/community/tutorials/getting-started-with-rspec
