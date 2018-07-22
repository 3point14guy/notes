# Test Drive Development
## RSpec in Ruby

Let's get a little taste of Test Driven Development. Though normally done within Rails, we're going to it try outside of that framework.

Useful Resources:
[RSpec Official Site](http://rspec.info/)
[RSpec Gem](https://github.com/rspec/rspec)

---

1. Homework Review
2. What is TDD?
3. What is RSpec?
4. Let's Get Started!
5. Testing Methods
6. Passing Specs
7. Fighter Spec

---

### What is TDD?
Test Driven Development is a development process where we write a test before we write code. We watch the test fail, write code to make the test pass, watch the test pass, and then refactor. 

**Why Write Tests First?
Well for a couple of reasons: 
1. If you write a test, add the code, and the test fails, your instinct will always be to check the code over the test.

2. Your tests will drive your design. They will force you to write better code that can be easily modified. How?
 * When you write a test you are making a decision on the design of your application. You have to think of how a piece of code fits into the application.
 * In the refactor stage you check for duplication and ways to improve your code
 * You'll find yourself writing shorter methods, which will make your application easier to change in the future.


### What is RSpec?

The **R** stands for Ruby 
and
**Spec** stands for Specification.

A specification is an expected outcome your code should meet. RSpec is a testing framework with a DSL (Domain Specific Language) that looks a lot like English.

```ruby
describe '#sum' do
  it 'adds two numbers'
    expect(2 + 7).to eql(9)
  end
end
```


### Let's Get Started!
1. In your terminal run: `gem install rspec`

2. Check to see if it installed by running: `rspec -v`

3. Inside of your Ruby directory create a new directory called *ruby_day6*

4. Inside of *ruby_day6* create two more directories *spec* and *lib*

5. In fighter create the file **".rspec"**

6. In lib, create **"playground.rb"**

RSpec will by default use any flags that are listed in the ".rspec" file. For now let's add:

```
--format documentation
--color
```


### Testing Methods

In **plaground.rb**:
```ruby
RSpec.describe "#hola" do
  it 'greets a person with their name' do
    expect(hola('Walid')).to eql('Hello Walid')
  end
end
```

**RSpec.describe "#hola" do describe**
is a method that takes an argument.
The argument can be the name of a class, or a string. When describing a method we use a hash ( # ) to denote it's an instance method and a period ( . ) to denote it's a class method.

** it 'greets a person with their name' do**
*it* is used to describe a spec or example.  The argument should always be descriptive of what's expected.

The code inside of the *it* method is always run. Here our block is saying when the hola method takes 'World' as an argument it should return 'Hello World'

**expect(hola('World')).to eql('Hello World'))**
We start by passing *hola('World')* to *expect()* and then use the *eql* matcher to verify the result.


### Passing Specs
To run this example, cd into lib, then run: 
```sh
rspec playground.rb
```
Your terminal should say the test failed. Go ahead and make this test pass. At the top of **plaground.rb**, write the method:
```ruby
def hola(name)
  "Hello #{name}"
end
```

Run your test again and we should see it pass!
Now this is great and all, but your test shouldn't be in the same file as your code. So let's create **"calculator_spec.rb"** in your spec directory.


### Calculator Spec
By convention Ruby files always go in the lib directory and RSpec files go in the spec directory. RSpec files always end in _spec.rb.  

In calculator_spec.rb:

```ruby
# calculator_spec.rb
require '../lib/calculator'

RSpec.describe Calculator do
    it '.new creates a new instance of calculator' do
	expect(Calculator.new(5,10)).to be_an_instance_of Calculator
    end	 
end	
```

In your project's directory run: 
```sh
rspec calculator_spec.rb
```

The spec failed because there is no **Calculator** class defined yet.

Now in your lib directory make a "calculator.rb" file and let's make the spec pass.

```ruby
# calculator.rb
class Calculator
    def initialize(num1, num2)
    end
end
```

Now in **"calculator_spec.rb"**, under the '.new' spec, let's add a spec that is expecting the num1 to be returned. Once it's in there run the specs.

```ruby
  it '#num1 should return the value of num1' do 
        expect(Calculator.new(5,10).num1).to eql(5)
  end
```

Then in "calculator.rb" write the code to make the spec pass:
```ruby
class Calculator
    def initialize(num1, num2)
		@num1 = num1
    end

    def num1
		@num1
    end	
end	
```

### #num2##

Next, in "calculator_spec.rb" under the '.num1' spec let's add a spec that will num2.

```ruby
 it '#num2 returns the value of num2' do
    expect(Calculator.new(5,10).num2).to eql(10)
  end
```

Then in **calculator.rb** write the code to make the spec pass

```ruby
class Calculator
    def initialize(num1, num2)
		@num1 = num1
		@num2 = num2
    end

    def num1
		@num1
    end	
    def num2
		@num2
    end	
end	
```

Refactor **calculator.rb** to take less code but still have the specs pass:
```ruby
class Calculator
  attr_accessor :num1, :num2
  def initialize(num1,num2)
    @num1 = num1
    @num2 = num2
  end
end
```

We can also refactor our specs. Check out the first line within *describe*:
```ruby
#calculator_spec.rb
require '../lib/calculator'

RSpec.describe Calculator do
    let (:calculation1){Calculator.new(5,10)}
    it '.new creates a new instance of calculator' do
        expect(calculation1).to be_an_instance_of Calculator
    end	 
    it '#num1 should return the value of num1' do 
	expect(calculation1.num1).to eql(5)
    end	
    it '#num2 returns the value of num2' do
        expect(calculation1.num2).to eql(10)
    end
end	


```

*let* allows us to create a variable we can use in any *it* block within the *describe*.

# #add

Let's add a spec to see if we can return the sum of the two numbers 

```ruby
require '../lib/calculator'

RSpec.describe Calculator do
    let (:calculation1){Calculator.new(5,10)}
    it '.new creates a new instance of calculator' do
        expect(calculation1).to be_an_instance_of Calculator
    end	 
    it '#num1 should return the value of num1' do 
	expect(calculation1.num1).to eql(5)
    end	
    it '#num2 returns the value of num2' do
        expect(calculation1.num2).to eql(10)
    end
    it '#add returns the sum of num1 and num2' do
  	expect(calculation1.add).to eql(15)
    end	
end	
```

# calculator.rb

Run the spec and... Nope. Failed.

So let's make it pass:

```ruby
class Calculator
  attr_accessor :num1, :num2
  def initialize(num1,num2)
    @num1 = num1
    @num2 = num2
  end

  def add
    @num1 + @num2
  end 
end
```



# Challenge!

Write the following tests in calculator_spec.rb:

1. returns the difference of num1 and num2 (use ".abs" to get a non-negative result)
2. returns the product of num1 and num2
3. returns the quotient (with decimals) of num1 and num2



Now, write the code in Calculator.rb to make each test pass.

 

**HINT: **

Make each test pass one at a time

until you have them all passed!





# Solution

 calculator_spec.rb:



```ruby
it '.diffence returns the difference of num1 and num2' do
     expect(calculation1.difference).to eql(5)
  end	

  it '.multiply returns the product of num1 and num2' do
     expect(calculation1.multiply).to eql(50)
  end

  it '.divide returns the quotient of num1 and num2' do
      expect(calculation1.divide).to eql(0.5)
  end	
```

# #mystery math

What happens when we apply #mystery math to our numbers?

In calculator_spec.rb, let's add another instance of Calculator called "calculation2"

```ruby
let (:calculation2){Calculator.new(4,12)}
```

Add the following test to your test spec

```ruby
it '#mystery_math returns the result of a secret equation' do
      expect(calculation1.mystery(calculation2)).to eql(146)
  end	
```
# #mystery math

We'll never know... because the test failed.  But we need to know! Let's fix the code! 

One solution is: 

calculator.rb
```ruby
def mystery_math
	num ** 2 + num1 / 2
end
```

The complicated solution (to illustrate a point):

caculator.rb

```ruby
 def mystery_math(calc2)
    (self.multiply) + (calc2.times_two)
 end	
 def times_two
    (self.num1 * self.num2) * 2
 end  
    
```

## WHAT THE FLIP IS GOING ON HERE?!?!

Here ```self``` refers to the instance of a class.  Let's back up a bit and look at this more simply. Comment out the ```mystery_math``` code for now and in ```divide```:

calculator.rb
```ruby
	# lets add some puts statements to get a clearer idea of what we are working with
	def divide
		num1 / num2.to_f
		puts "In divide, num1 is #{num1}"
		puts "num2 is #{num2}"
		puts "self is #{self}"
	end
	
	# commented out code here ...
	
	calculation1 = Calculator.new(5, 10)
	# calculation2 = Calculator.new(4, 12)
	puts calculation1.divide
	# puts calculation2.divide
```

Nothing unexpected from ```num1``` and ```num2```. ```self``` points to the place in memory where the instance of the Calculator class is. It is referring to the calculator1 object.  

*We called the divide method; why don't we see the result of that calculation?*

Let's explore this a little further.

calculator.rb
```ruby
	def divide
		num1 / num2.to_f
		puts "In divide, num1 is #{num1}"
		puts "num2 is #{num2}"
		puts "self is #{self}"
		puts "self.num1 is #{self.num1}"
		puts "self.num2 is #{self.num2}"
		puts "self.multiply is #{self.multiply}"
	end

```

The output to the terminal now shows:

```
In divide, num1 is 5
num2 is 10
self is #<Calculator:0x007fa5df95f2e0>
self.num1 is 5
self.num2 is 10
self.multiply is 50
```

The idea of self here is pretty straight forward, but is it working the same way in the ```mystery_math``` method?  We'll put some comments in those methods too to take a peak.

calculator.rb
```ruby
	def mystery_math(calculation2)
		puts ""
		puts "In mystery_math, self = #{self.num1}, #{self.num2}"
		puts "calculation2 = #{calculation2.num1}, #{calculation2.num2}"
		puts ""
		self.multiply + times_two
	end

	def times_two
		puts "In times_two, self.num1 = #{self.num1}"
		puts "self.num2 = #{self.num2}"
		puts ""
		(self.num1 * self.num2) * 2
	end
	
	#lets now call the mystery_math method. remember it requires an arguement.
	puts calculation1.mystery_math(calculation2)
```

Let's make some predicitions about what we expect to be returned. When we look at the results, pay close attention to the values for ```self``` in ```times_two```:

```
In mystery_math, self = 5, 10
calculation2 = 4, 12

In times_two, self.num1 = 4
self.num2 = 12

146
```

*If time permits, go through the fighter example in the slides*
Copyright © 2013 - 2018 Tech Talent South. All rights reserved. 
