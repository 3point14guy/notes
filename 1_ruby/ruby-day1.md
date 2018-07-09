# Ruby: An Introduction
## Ruby Basics

Let's learn the very basics of Ruby, the world's friendliest programming language!

## Useful Resources
[Official Ruby Docs](http://ruby-doc.org/)
[Code Academy's Ruby Course](https://www.codecademy.com/learn/ruby)

---

1. What is Ruby
2. Interactive Ruby Shell
3. Data Types
 * Strings
 * Integers & Floats
4. User Input
5. Data Type Conversion
6. Concactentation vs. Interpolation
7. Running Ruby programs
8. Control Flow
 * if/else
 * elsif
 * && and ||
 * case
 * unless

---

### What is Ruby?
Ruby is an object-oriented programming language, based on C. With the popularization of the Rails platform, Ruby has been a favorite of programmers who want to develop apps quickly. It was created by Yukihiro “Matz” Matsumoto in the mid-90s.

#### Why use Ruby?
It has a low learning curve: it is easy to read code. Some say it is the closest programming language to written English. It is object-oriented, which makes for easy use. It has a powerful tool set, and a large, supportive community (so on places like [Stack Overflow](http://stackoverflow.com/), people are more helpful that hurtful).


### Interative Ruby Shell
Open up your Terminal or Command Prompt. From whereever you are, you can open up the Interactive Ruby Shell by typing **"irb"**. Within this console, you can perform Ruby tasks. This makes it a great place to try out smaller bits of Ruby code (instead of running a Ruby program). Each line is numbered, with the version of Ruby you are running preceding that line #.

We will use **irb** for the first several slides, then move into creating and running Ruby programs.


### Output

To output a message or any sort of data in Ruby, we use the commands *puts* or *print*.

**puts**
Prints out the message with a line break.

**print**
Prints the message with no line break.


### Data Types
Ruby has several "Data Types", which are defined "classes" with particular attributes (we'll get into what that means exactly later).
Ruby's Data Types include:
* String (anything between quote marks)
* Integer (whole numbers)
* Float (decimaled numbers)
* Boolean (true or false)
* Array (an ordered collection)
* Hash (an unordered collection)

Also worth noting:
* Time
* Symbols (we will see these used in Hashes and Classes)


### Variables
Creating and assigning variables in Ruby is super simple! You don't have specify, like you do in other programming languages, which data type you want the variable to be. Ruby is smart enough to recognize a data type on its own!

```ruby
#a string:
sentence = "I love Ruby, and I don't care who knows it!"

#Ruby knows it's a string because of the quotation marks

#an integer:
my_number = 42

# Ruby knows it's an integer because it's a whole number 
# (no decimal point and value afterwards)

#a float:
my_other_number = 15.16

# Ruby knows it's a float because
# there's a decimal point and value afterwards

#a boolean:
the_truth = false

# Ruby knows it's a boolean because it's either true or false 
# (and no quotation marks!)
```

When it comes to naming variables, you should use all lowercase letters. If you want the variable name to be two or more words, use an underscore ( _ ) between the words:
```ruby
whats_up = "not much"

my_fav_number = 23
```

Variable names can contain a number, but they must start with a letter:
```ruby
where_2_go = "the beach"

number4 = 8.15
```


### Strings
[Ruby Docs](https://ruby-doc.org/core-2.4.0/String.html)

Strings are text captured within quotation marks. A String can be a word, or a sentence, or several sentences. It can be a number, too - but that number will be seen as text.

```ruby
#this is a string:

my_string = "Nah"

#and so is this:

my_other_string = "Nah nah nah nah nah nah nah nah nah nah nah, hey Jude. Nah nah nah nah nah nah nah nah nah nah nah, hey Jude. Nah nah nah nah nah nah nah nah nah nah nah, hey Jude."
```

You can also think of a String as a set of characters. And you can pull an individual character by it's index (or, it's placement within the String). The index is a number between 0 and one-less-the-length of the String, captured in square brackets ([]).

```sh
> my_string = "Oh what a lovely String this is."
> puts my_string[11]
=> "o"
```

Strings have several "methods" attached to them that help you search through and modify them.

```sh
> my_string = "the best string ever!"

> my_string.reverse
=> "!reve gnirts tseb eht"

> my_string.capitalize
=> "The best string ever!"

> my_string.upcase
=> "THE BEST STRING EVER!"

> shouting = "ARE WE HAVING FUN YET?"
> shouting.downcase
=> "are we having fun yet?"

> shouting.length
=> 22
```


### Numbers
[Ruby Docs - Integers](http://ruby-doc.org/core-2.4.1/Integer.html)
[Ruby Docs - Floats](https://ruby-doc.org/core-2.4.0/Float.html)

Integers are whole numbers. Floats are numbers with decimal places. Even if it's 3.0 - that is still considered a Float.

Though they have some different properties, they are still somewhat compatible (more so than Numbers and Strings).

Here is a guide to how equations between the Integers and Floats will behave:

Integer + (or) - (or) * Integer = Integer
Integer / Integer = Integer
(even if not divisible)

Float + (or) - (or) * Float = Float
Float / Float = Float
(if divisible, #.0, so still a Float)

Integer + (or) - (or) * Float = Float
Integer / Float = Float
Float / Integer = Float

Ruby also has equation shortcuts! We can stack the operators with an equal sign between variables (only) and reassign result to the first variable.

Example:
```sh
> num1 = 2
=> 2
> num2 = 4
=> 4
> num1 += num2
=> 6
> num1
=> 6
```
This works with the other operators, too.

#### The Modulus
Another mathematical operator that you may not be familiar with, but will help you more than you'd imagine is the modulus (or modulo)
It is represented by the percentage symbol. When put between two numbers, it returns the remainder of the division of those two numbers.

```ruby
10 % 5 == 0
# because 10 is completely divisible by 5

15 % 6 == 3
# the remainder of the division of 15 by 6 is 3
```


### Input
In Ruby, **gets** is the command that takes input, but it takes it with a line break.
**.chomp** is a function (method) that works on **gets** to removes the line break.

```ruby
puts "What's your name?"

name = gets.chomp

puts "Oh, hi " + name
```

If we were running a Ruby program with **gets.chomp** & ((puts**:
```sh
$ ruby firstprogram.rb
What's your name?
Mr. Roboto
Oh, hi Mr. Roboto
$
```

It's important to note that **gets.chomp** (or even **gets** alone) takes any input as a String data-type.


### Data Type Conversion
Sometimes, you just need a variable to be something it's not. That's when it's time to convert the data type! These methods are especially handy when dealing with **gets.chomp**.

`.to_s` - convert to String
`.to_i` - convert to Integer
`.to_f` - convert to Float

```ruby
#Converting a string to an integer:

my_number = "1"

my_number.to_i

#so now my_number = 1, no quotation marks

#Converting integer to string:

my_number = 2

my_number.to_s

#now my my_number = "2"

#Converting integer to float:

my_number = 3

my_number.to_f

#now my_number = 3.0
```


### Concatentation vs. Interpolation
When printing out a variable attached or within a String, there are two schools of thought:

**Concatentation**
...uses the + sign to add strings and variables together. But the variable must be a String!
```sh
> age = "24"
> puts "Today I am " + age + "-years-old."
=> "Today I am 24-years-old."
```

**Interpolation***
...uses the symbols #{} to plop a variable inside a string, but only works within double-quotes. With interpolation, you can use any data type!
```sh
> age = 58
> puts "Yesterday I turned #{age}-years-old!"
=> "Yesterday I turned 58-years-old!"
```


### Running Ruby Programs
Let's move beyond irb...

To run a Ruby program:
* Remember to save as an '.rb' file
* Open your terminal/command prompt
* Navigate to where your program is saved
* Run this command: ruby [programname].rb

Create a Ruby folder in your TTS folder. Open the Ruby folder in Sublime. Create a new file within called **firstprogram.rb**.

```ruby
# firstprogram.rb
puts "Hello Universe"
```

Run it in Terminal:
```sh
$ ruby firstprogram.rb
Hello Universe
$ 
```


### Control Flow
"Control Flow" has to do with the idea of **if** a condition is true, **then** a certain thing will happen. For example, if a variable has a value greater than five, we ask the program to print out a particular message. If the variable is five or less, then nothing happens.

But if you do want something to happen if the variable is less than five, then you say **else** and a second **then** block of code.

We can refer to these as *if statements* or *if/then statements* or *if/else statements*.

```ruby
if [condition]  
    #if the condition is true
    #then perform some action
else
    #if that condition above is false
    #then perform some other action
end
```

Ruby needs to be told when a block of code is complete, so you must put **end** when your statement is complete.
Here's a more fleshed-out example:

```ruby
if sum == 13

 puts "Wait...the lucky 13 or the unlucky 13?"

else

 puts "Phew! For a second I thought it was gonna be 13."

end
```


*Ruby Comparison Symbols*

| Symbol |         Meaning          |
| ------ | :----------------------: |
| ==     |          Equals          |
| !=     |      Does Not Equal      |
| <      |        Less Than         |
| >      |       Greater Than       |
| <=     |  Less Than or Equal To   |
| >=     | Greater Than or Equal To |

When doing comparisons, make sure you're using double-equal (==), not just a single-equal (=)! Single-equal is for assigned value, and will mess up your conditional!

#### If/Else Activities
*Write these along with students; ask for their input on how to write the programs*

* Dog Says Cat Says: ask user to enter 'dog' or 'cat', program prints animal's sound

Create a new file called **animal_sounds.rb** for this activity (we will come back and ammend it as we learn more about conditionals).

Possible Solution:
```ruby
# animal_sounds.rb

puts "Which do you prefer? Dog or Cat?"

animal = gets.chomp

if animal == "dog"
	puts "Woof!"
else
	puts "Meow!"
end
```
**What if someone enters "Dog" or "DoG"? The program won't work as intended. Ask the students and see if anyone can come up with adding **.downcase** to gets.chomp.

* Guessing Game: user provides a number (between 1 and 10), if the number stored in the program is the same, "Wow!", else, "Nope!"

Create a new file called **guessing_game.rb**. Once again, we will be returning to this program when we learn more about conditionals.

Possible Solution:
```ruby
# guessing_game.rb
print "Give me a number between 1 and 10: "
guess = gets.chomp.to_i

comp_num = 5

if guess == comp_num
	puts "You got it!"
else
	puts "No, sorry. It was #{comp_num}."
end
```
**See if anyone knows how we can make the computer number choice more random. Suggest the use of **rand(1..10)**.

* Ask the user for their number grade, if the grade is at least 60, tell them they passed! If it's lower than 60, tell them they have to take the class again.

Create a new file called **grades.rb**.

```ruby
# grades.rb
puts "What number grade did you get in the class?"

grade = gets.chomp.to_i

if grade >= 60
	puts "You passed! Have a cool summer!"
else
	puts "Oh, dang. You're gonna have to take the class again. That's a bummer."
end
```

#### elsif
Maybe you want to give your program more choice. You can add as many else ifs (written in Ruby as elsif) in between your if and else.

```ruby
if answer == "do"
    puts "May the Force be with you."
elsif answer == "do not"
    puts "Too old. Yes, too old to begin the training."
elsif answer == "try"
    puts "There is no try."
else
    puts "Mudhole? Slimy? My home this is!"
end
```

#### elsif Exercise

Update the Dog Says Cat Says program:
* Ask the user for an animal
* Use if/elsif to program in a number of animal sounds
* Use else for unknown animals

Possible Solution:
```ruby
# animal_sounds.rb
puts "What is your favorite animal?"

animal = gets.chomp.downcase

if animal == "dog"
	puts "Woof!"
elsif animal == "cat"
	puts "Meow!"
elsif animal == "horse"
	puts "Neigh!"
elsif animal == "snake"
	puts "Hiss!"
elsif animal == "lion"
	puts "Roar!"
else
	puts "Grr?"
end
```

#### && and ||
Sometimes you want two conditions to be met before you perform a task. This is where AND and OR come in handy. AND is written as two ampersands ( && ), OR is written as two pipes ( || ).

Note: you need to make the comparison on both sides of the &&  or ||.

```ruby
if sum > 13 && sum < 26
    puts "Right in the sweet spot."
else
    puts "Too little, too much."
end


if choice == "cash" || choice == "credit"

    puts "Thanks for shopping at our store."
else
    puts "Sorry, we don't accept checks."
end
```

#### AND/OR Exercise
Update the Guessing Game program:
* Ask the user for a number between 1 & 100
* Use both AND/OR and elsif to test for both exactness and closeness
* e.g., if the guess is only five away, have a message to say something like "Oh! So close!"

```ruby
print "Give me a number between 1 and 100: "

guess = gets.chomp.to_i

comp_num = rand(1..100)

if guess == comp_num
	puts "Whoa, you got it!"
elsif (guess >= comp_num - 5) && (guess <= comp_num + 5)
	puts "Oh, so close! It was #{comp_num}."
else
	puts "Nope. It was #{comp_num}."
end
```

#### case Statement
There is an alternative to the if/elsif/else structure: the *case* conditional. *case* is probably better learned from show than tell, so see an example:
```ruby
#we have a variable called 'option' that we want to test

case option
  when 1 #when option == 1
      #do something  
  when 2 #when option == 2
      #do something else
  else
      #do a third thing
end
```

You can use the && and || specifiers with case, too.
There is no specific time when you should use *case* over *if/elsif/else*
...but *case* can be DRY-er.

There's an even DRY-er way to do *case*: using the **then** keyword
```ruby
#we have a variable called 'option' that we want to test

case option
  when 1 then puts "It's one."
  when 2 then puts "It's two." 
  else puts "Guess it's three?"
end
```
One-line *when* statements will only work if there's a *then*. The *else* does not take a *then*, but can be written on one line.

**case Activity**
(not in the slide deck)
Convert your **animal_sounds.rb** if/elsif/else statement into a case statement.
Possible Solution:
```ruby
# animal_sounds.rb
puts "What is your favorite animal?"

animal = gets.chomp.downcase

case animal
	when "dog" then puts "Woof!"
	when "cat" then puts "Meow!"
	when "horse" then puts "Neigh!"
	when "snake" then puts "Hiss!"
	when "lion" then puts "Roar!"
	else puts "Grr?"
end
```

#### unless
One more conditional block of code you should know about is the *unless* statement.
This is sort of a backwards *if* statement:

```ruby
unless sum == 13
  puts "Phew! For a second I thought it was gonna be 13."
else
  puts "Oh, no! The sum is 13! Run for your lives!"
end
```

**unless Activity**
Reverse the *if* statement on **grades.rb** to fit an *unless* statement:
```ruby
# grades.rb

puts "What number grade did you get in the class?"

grade = gets.chomp.to_i

unless grade >= 60
	puts "Oh, dang. You're gonna have to take the class again. That's a bummer."
else
	puts "You passed! Have a cool summer!"
end
```
Copyright © 2013 - 2017 Tech Talent South. All rights reserved. 