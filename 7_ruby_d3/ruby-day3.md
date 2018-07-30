# Ruby: Day Three
## Methods

Learning how to keep your code DRY, using methods, and some other handy tips.

## Useful Resources
[Official Ruby Docs](http://ruby-doc.org/)
[Code Academy's Ruby Course](https://www.codecademy.com/learn/ruby)

---

1. Homework Review (refer to solutions markdown)
2. Ruby Review
3. Methods
	* How to define
	* How to call
	* Return
	* Passing Arguments
4. Method Exercises
5. Keeping DRY Beyond Methods
6. One-line If Statement
7. Ternary Operator
8. Ternary Exercises

---

### Ruby Review

1. What two values can a boolean take?
	*true* or *false*

2. In a case statement, what do we need to write a one-line when?
	*then*

3. What elements make up an entry in a Hash?
	*key* and *value*

4. How do you avoid an infinite loop?
	*increment* or *decrement*

5. What do you use to iterate through an array, and keep track of where you are in that array?
	*.each_with_index*


### Methods

Loops are great for repeating code, but some times you don't want to repeat the code immediately.  As programs get more complex, it also makes sense to group your code into methods.  We define a method like this:

```ruby
def my_first_method
  # some action is performed here
end
```

Methods come in handy when you have to perform a block of code several times.
```ruby
def dont_repeat_yourself
  #my program will perform this action whenever I ask!
end
```

We want to keep our code as DRY (Don't Repeat Yourself) as possible. 

Methods can do as much or as little as you need them to do.

```ruby
def hello
  puts "Hello universe!"
end

def really_hard_math
  answer = (-5 + Math.sqrt(5**2 - 4 * (8*15)))/(2 * 8)
  puts answer 
end
```

But you have to ask yourself: will this reduce lines of code? For the first one ("hello" method), it would only make sense to make this method if you going to print out "Hello Universe" more than four times (three lines for the method definition, one line for "calling" the method).

A method will not be performed until it is "called". You "call" it's name to activate the code.

```ruby
# define the method:
def add_it_up
  sum = 3 + 5
  puts sum
end

# now call the method!
add_it_up
```

In the Terminal:
```sh
$ ruby adding.rb
8
$
```

A method can do more than just print to screen its results. The result of this code is the integer 4. We could even assign it to a varialble and use that result in other code blocks.

```ruby
def two_plus_two
  sum = 2 + 2
  return sum
end
```

The command return gives the method a value. We have a couple options when calling the method:
```ruby
def two_plus_two
  sum = 2 + 2
  return sum
end

puts two_plus_two 
# prints the value returned by the method returns

answer = two_plus_two
# assigns the value returned by the method to a variable
```
one thing to keep in mind about ```return``` is that any code coming after it will not be executed.
You don't even need to put return at the end of a method: Ruby will return the last value assigned.

```ruby
def two_plus_two
	sum = 2 + 2
end

puts two_plus_two
```

In the Terminal:
```sh
$ ruby add_it.rb
4
$
```

We have to be aware of something called scope.  Scope is the idea that varialbles and methods can only be accessed within given bounds. The variables inside the method only exist while that method is running so we can't reference them from outside the method.

```ruby
def add_up(number)
	number + 2
end

puts add_up(3)
puts number

# This returns 5 and error!
```

We can define our variables outside a method and then they will be accessible outside that method, but take note:

```ruby
number = 1

def add_up(number)
	number + 2
end

puts add_up(3)
puts number

#This returns 5 and 1
```

What if we add little line of code to take a peak at what number the parameter is?

```ruby
number = 1

def add_up(number)
	puts number
	number + 2
end

puts add_up(3)
puts number

#This returns 3, 5 and 1
```

One other variation to mull over:

```ruby
number = 1

def add_up(number)
	puts number
	number + 2
end

puts add_up(number)
puts number

#This returns 1, 5 and 1
```

### Give Me Something to Work With
***slide_7*** **[SHOW](http://techtalentsouth.slides.com/techtalentsouth/ruby-three-ci?token=Rm6saPYA#/7)**

Note to students: This slide doesn't make the definitions clear.
You can give a method parameters (passed values as variables,) to play with!
```ruby
def add_it_up(num1, num2)
  sum = num1 + num2
end
```

When you call those methods, the variables you pass in are called arguements.
```
puts add_it_up(4, 5)
#what would be printed to screen?
```

You can pass variables from outside the method via parameters.
```ruby
def add_it_up(num1, num2)
  sum = num1 + num2
end

time = 4
space = 5

puts add_it_up(time, space)
```

### Method Exercises
***slide 9*** **[SHOW](http://techtalentsouth.slides.com/techtalentsouth/ruby-three-ci?token=Rm6saPYA#/8)**


**Work through with students, asking for their input**

* Create a method for converting weight from pounds to kilos.

**How would we think about solving this if we didn't use a method?**

Possible Solution (**weight_conversion.rb**):
```ruby
def pounds_to_kg(lbs)
	kilos = lbs * 0.453592
	return kilos.round(2)
end

puts "What is the weight of that sack of potatoes?"

weight = gets.chomp.to_i

puts "Okay, that's #{pounds_to_kg(weight)} kg."
```

* Create a method that takes a String argument and outputs the String in reverse. (But you can't use .reverse!)

This one is tricky, and probably has several solutions.

*TALK THIS ONE OUT 1ST*

Possible Solution (**reverse_string.rb**):
```ruby
def reverser(str)
	# split the string up by character
	# into an array
	arr = str.split("")

	# we will store the reversed
	# items in another array
	reversed = []

	# iterate through original array
	arr.each do |character|
		# .push, .pop, .shift, .unshift
		reversed.unshift(character)
	end

	# return the reversed array
	# converted to String
	return reverse.join()
end

puts "Give me a string to reverse:"
str = gets.chomp

puts reverser(str)
```

* Create a method that converts an Array into a Hash
(indexes become keys).

Talk about what we are starting with and the anatomy of an array and then what we want our final product to be and the anatomy of a hash.

Though having integers as keys is probably not the best, we're going for practice rather than practicality.
Possible Solution (**array_2_hash.rb**):
```ruby
def array2hash(arr)

	hash = {}

	arr.each_with_index do |item, index|
		hash[index] = item
	end

	return hash
end

arr = %w(Stegosaurus Ankylosaurus Pteranodon Triceratops)

puts array2hash(arr)
```

#### Method Challenge
In-class activity: lead the students through this exercise.

In Ruby 6 / 4 == 1. But what if we want the remainder also?
Write a program that asks for two (2) Integers, divides the larger by the smaller and returns the result including the remainder.
 
If either of the numbers is not an Integer, then don't accept the number and ask again.
 
Do not accept zero (0) as a number.

We know we will have to get 2 numbers.  There will be some duplication of code there, so let's see about putting that into a method.

```ruby
def get_num
	print "Please enter a number: "
	num = gets.chomp
	return num
end
```

That get's us one number.  We'll have to call this function twice to get both of our numbers.

```ruby
2.times do
	number = get_num.to_i
end
```

The problem with this is that when the loop runs the second time, it overwrites the value that was assigned to ```number``` the first time.  We are going to save these numbers in an array because that will let us do a little trick.

```ruby
arr = []
2.times do
	number = get_num.to_i
	arr.push(number)
end

arr.sort!
```

We're now set up to do our math.  We are going to refer to these numbers by their position in the array. 

```ruby
if arr[1] % arr[0] == 0
	puts "#{arr[1]} / #{arr[0]} = #{arr[1] / arr[0]}"
else
	puts "#{arr[1]} / #{arr[0]} = #{arr[1] / arr[0]} with a remainder of #{arr[1] % arr[0]}"
end	
```

There's some duplication here too, but we will leave that refactoring for you to try at some other time. Our program still has two more conditions that it needs to meet to get to MVP.

```ruby
def int_check(numb)
	if numb.to_i == 0 || numb.include?(".")
		return true
	else
		return false
	end
end
```

It would be nice to test the numbers right after we collect them from the user.  Let's call this method inside our get number method.  Then, if it fails, we can prompt the user to enter a valid number.

```ruby
def get_num
	print "please enter a number: "
	num = gets.chomp
	if int_check(num)
		puts "I can't divide very well, though."
		puts "I only use non-zero integers."
		get_num
	else
		return num
	end	
end
```

The final code all together will look like this:

```ruby
puts "I can divide integers!"

def get_num
	print "please enter a number: "
	num = gets.chomp
	if int_check(num)
		puts "I can't divide very well, though."
		puts "I only use non-zero integers."
		get_num
	else
		return num
	end	
end

def int_check(numb)
	if numb.to_i == 0 || numb.include?(".")
		return true
	else
		return false
	end
end

arr = []
2.times do
	number = get_num.to_i
	arr.push(number)
end

arr.sort!

if arr[1] % arr[0] == 0
	puts "#{arr[1]} / #{arr[0]} = #{arr[1] / arr[0]}"
else
	puts "#{arr[1]} / #{arr[0]} = #{arr[1] / arr[0]} with a remainder of #{arr[1] % arr[0]}"
end	
```

 Similar Possible Solution (**remainder_challenge.rb**):
```ruby
# method to see if input is not a whole number,
# if there is a period/decimal point, we can
# assume they meant to enter a float.
def float_check(num)
	if num.include?(".")
		return true
	else
		return false
	end
end

# method to see if input is zero.
# num will be an integer by this point.
def zero_check(num)
	if num == 0
		return true
	else
		return false
	end
end


def take_number
	print "Please give me a number: "
	num = gets.chomp
	if float_check(num)
		puts "That's a float. We want an integer."
		take_number
	else
		# gotta convert to Integer before zero-checking!
		num = num.to_i	
		if zero_check(num)
			puts "Give an integer that's not zero."
			take_number
		else
			return num
		end
	end
end

arr = []

2.times do
	num = take_number
	arr.push(num)
end

arr.sort!

if arr[1] % arr[0] == 0
	puts "#{arr[1]}/#{arr[0]} = #{arr[1]/arr[0]}"
else
	puts "#{arr[1]}/#{arr[0]} = #{arr[1]/arr[0]}, with a remainder of #{arr[1]%arr[0]}"
end
```


### DRYer Conditions
Part of learning a programming language is learning how to do things faster: to find the least amount of code possible to do the most work.
That's what methods are all about! But what if we could apply this thinking to other aspects of our code... 
What about our control flow conditionals?

### DRYer If/Unless Statements
Take an if statement like this:
```ruby
DRYer Conditions
Take an if statement like this:
if x == 50
  puts "Oh, we're halfway there."
end
```

Note that this is purely an *if*, there is no *else* condition. Why should we take up three lines of code on this?
Let's just it DRY-er...
```ruby
puts "Oh, we're halfway there." if x == 50
```
The code to be performed comes first, then the condition, and we lose the *end*.

The same can apply to *unless* conditionals:
```ruby
unless response.include?("stop")
  puts "Thank you, may I have another."
end
```
Remember, our *puts* will only perform if the condition returns *false*. Let's see that on just one line:
```ruby
puts "Thank you, may I have another." unless response.include?("glitch")
```
*(if you're like, "What does that even mean?"... ask a TTS old-timer)*


### Ternary Operator
Now, what if you do have an else statement, but you're also a fan of keeping it DRY?

Well, you're in luck! The ternary operator is here to help! This how it's laid out:

`condition ? return when true : return when false`
 
We can either print the result to screen, or save in a variable.

#### Ternary Exercises
*Slides are timed to show *if* example, then *ternary* conversion after second click. See if students can work out conversion before showing them the solution.*

Let's convert some if/else statements to ternary:

```ruby
if x > 50
  puts "Over halfway there!"
else
  puts "Still a ways to go"
end
```
With Ternary Operator:
```ruby
puts x > 50 ? "Over halfway there!" : "Still a ways to go"
```

```ruby
if arr.include?(42)
  answer = "everything"
else
  answer = "nothing"
end
```
With Ternary Operator:
```ruby
puts answer = arr.include?(42) ? "everything" : "nothing"
```

#### More Ternary Exercises
*These aren't in the slides, but if you need to fill more time, go back to the if/else exercises from Ruby Day 1 and convert to ternary. Create a new file to house these three exercises, or just return to the original files.*

* Dog Says Cat Says: ask user to enter 'dog' or 'cat', program prints animal's sound
```ruby
# animal_sounds.rb

puts "Which do you prefer? Dog or Cat?"

animal = gets.chomp

puts animal == "dog" ? "Woof!" : "Meow!"
```

* Guessing Game: user provides a number (between 1 and 10), if the number stored in the program is the same, "Wow!", else, "Nope!"
```ruby
# guessing_game.rb
print "Give me a number between 1 and 10: "
guess = gets.chomp.to_i

comp_num = 5

puts guess == comp_num ? "You got it!" : "No, sorry. It was #{comp_num}."
```

* Ask the user for their number grade, if the grade is at least 60, tell them they passed! If it's lower than 60, tell them they have to take the class again.
```ruby
# grades.rb
puts "What number grade did you get in the class?"

grade = gets.chomp.to_i

puts grade >= 60 ? "You passed! Have a cool summer!" : "Oh, dang. You're gonna have to take the class again. That's a bummer."
```
