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

As programs get more complex, it makes sense to group your code into methods.

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

A method will not be performed until it is "called".

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

A method can do more than just print to screen its results.
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

You can give a method arguments or parameters (passed values) to play with!
```ruby
def add_it_up(num1, num2)
  sum = num1 + num2
end

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

#### Method Exercises
**Work through with students, asking for their input**

* Create a method for converting weight from pounds to kilos.

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

Possible Solution (**reverse_string.rb**):
```ruby
def reverser(str)
	# split the string up by character
	# into an array
	arr = str.split("")

	# we will store the reversed
	# items in another array
	reverse = []

	# iterate through original array
	arr.each do |x|
		# instead of pushing,
		# insert at index 0 each item
		# so that the next one is first
		reverse.insert(0,x)
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
Write a program that asks for two (2) Integers, divides the first by the second and returns the result including the remainder.
 
If either of the numbers is not an Integer, then don't accept the number and ask again.
 
Do not accept zero (0) as a number.

Possible Solution (**remainder_challenge.rb**):
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
unless problems.include?("glitch")
  puts "I got 99 problems..."
end
```
Remember, our *puts* will only perform if the condition returns *false*. Let's see that on just one line:
```ruby
puts "I got 99 problems..." unless problems.include?("glitch")
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
answer = arr.include?(42) ? "everything" : "nothing"
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