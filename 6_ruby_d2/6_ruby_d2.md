# Ruby: Day Two
## Arrays, Hashes, and Loops

Looking at Ruby's collection data types, Array & Hash, and their properties, as well as moving into a new type of conditional: Loops.

## Useful Resources
[Official Ruby Docs](http://ruby-doc.org/)

[Code Academy's Ruby Course](https://www.codecademy.com/learn/ruby)

---

1. Homework Review (refer to solutions markdown)
2. Ruby Review
3. Ruby Collection Data Types
4. Arrays
	* Associated Methods
	* .include?
	* Array to String conversion
5. Hashes
	* Associated Methods
6. Loops Intro
7. .times
8. until
9. while
10. .each
	* Array.each
	* Array.each_with_index
	* Hash.each

---

### Ruby Review

1. What are the six main Ruby data types?
	* String, Integer, Float, Boolean, Array, Hash
	* Extra Points for: Time, Symbol

2. Why do we append .chomp to gets?
	* to remove the line break that comes with *gets*

3. What data type does gets.chomp provide?
	* String

4. What came come between if  and else ?
	* elsif

5. What are alternatives to the if statement? 
	* *case* and *unless*

---

### Ruby Collections
Two of the data types we mentioned yesterday we didn't have time to cover: Arrays & Hashes.
Both act as collections: you can store any other data type within. But there are some key differences...

### Arrays
[Ruby Docs - Arrays](https://ruby-doc.org/core-2.4.1/Array.html)

Arrays are a collection of data housed within square brackets []
An array can hold any sort of data type, or even a mix. Though it's probably best to keep it relegated to just one type.

There are many ways to initiate an Array:
```ruby
empty_array = []
new_array = Array.new    # also empty
my_array = [1,2,3,4]
my_other_array = ["uno","dos","tres","cuatro"]
yet_another_one = [1.0, "duece", 3, true]
single_item_array = %w(you can also use curly brackets)
```
Just like characters in Strings, items in an Array can be called by their index:
```sh
> my_array = ["mary","sybil","edith"]
=> ["mary","sybil","edith"]
> puts my_array[1]
=> "sybil"
```
Remember: In programming, indexes start at 0!

Also like Strings, Arrays have several **methods** associated with them that let us inspect and modify.

```sh
:001 > arr = ["Atlanta","Charlotte","Raleigh"]
 => ["Atlanta", "Charlotte", "Raleigh"] 
:002 > arr.first
 => "Atlanta" 
:003 > arr.last
 => "Raleigh"
:004 > arr.pop
 => "Raleigh" 
:005 > arr
 => ["Atlanta", "Charlotte"] 
:006 > arr.push("New Orleans")
 => ["Atlanta", "Charlotte", "New Orleans"]
:007 > arr.reverse
 => ["Atlanta", "Charlotte", "New Orleans"] 
 ```

Note that, besides .push & .pop, none of these modifications are permanent. To make the modifications stick, you have two approaches:

Re-assign the variable:
```sh
:001 > arr = [1,2,3,4,5]
 => [1, 2, 3, 4, 5] 
:002 > arr = arr.reverse
 => [5, 4, 3, 2, 1]
:003 > arr
 => [5, 4, 3, 2, 1]
```

Using method ending with a !
```sh
:001 > arr = [1,2,3,4,5]
 => [1, 2, 3, 4, 5] 
:002 > arr.reverse!
 => [5, 4, 3, 2, 1]
:003 > arr
 => [5, 4, 3, 2, 1] 
```

Another handy method for Arrays is: **.include?**
And yes, you need the question mark!
This method will return true or false, depending on if the searched-for item is found in the array.
```sh
:001 > nums = [1,2,3,4,5]
 => [1, 2, 3, 4, 5] 
:002 > nums.include? 5
 => true
:003 > nums.include? "bears"
 => false
:004 > arr = ["Richard","James","Jeremy"]
 => ["Richard","James","Jeremy"]
:005 > arr.include? "Chris"
 => false
```

You can also use **.include?** on Strings!
```sh
:001 > str = "Tech Talent South"
=> "Tech Talent South"
:002 > str.include?("Talent")
=> true
```

Sometimes, it may be easier to think of an Array as a String, or vice versa. For conversion between Array & String, we have **.join()** and **.split**. Notice that **.join()** can take an argument.
```sh
:001 > arr = ["Hey", "this", "is", "fun"]
 => ["Hey", "this", "is", "fun"] 
:002 > str = arr.join(" ")
 => "Hey this is fun"
:003 > back_to_arr = str.split
 => ["Hey", "this", "is", "fun"]
```
If you were to leave **.join**'s parentheses empty, it would just make a String with all the words squished together.

Or you can try a different approach...
```sh
:004 > new_str = %w(Ooh that's neat) * " "
 => "Ooh that's neat"
```
But **.join()** and **.split** are probably the easier way to go.


### Hashes
[Ruby Docs - Hashes](https://ruby-doc.org/core-2.4.0/Hash.html)
Hashes are similar to Arrays, in that they are collection of data. But while Arrays are stored by index (ordered), Hashes store data as "key"/"value" pairs (unordered).

```ruby
empty_hash = {}
also_empty = Hash.new
my_hash = {"key" => "value"}
my_other_hash = {"Name" => "Finn", "Role" => "Hero", "Age" => 16}
```
Each "key" & "value" are separated by a hash-rocket. And, you'll notice, a Hash is contained within curly brackets {}.

Methods associated with Hashes are a bit more limited than Array methods. Here's some important ones:
```sh
2.1.1 :001 > instructor1 = {"Name"=>"Aaron","Location"=>"Atlanta","Hair"=>"Red",
"Awesomeness Level"=>9}
 => {"Name"=>"Aaron", "Location"=>"Atlanta", "Hair"=>"Red", "Awesomeness Level"=>9} 
#find the first key/value pair:
2.1.1 :002 > instructor1.first
 => ["Name", "Aaron"]
#find a value by its key: 
2.1.1 :003 > instructor1["Location"]
 => "Atlanta" 
#delete a key/value pair (by it's key):
2.1.1 :004 > instructor1.delete("Hair")
 => "Red" 
2.1.1 :005 > instructor1
 => {"Name"=>"Aaron", "Location"=>"Atlanta", "Awesomeness Level"=>9}
```

**.include?** works on Hashes, but will only be checking the *keys*.

### Symbols
Though a key could be any data type, it will most commonly be a String (as we have seen), or a Symbol.
Symbols are represented with colons (:). Normally, the colon is in front (:name). But if it's pointing to something with a hash rocket, we can put the colon on the left and remove the hash rocket. Like this:
```sh
:001 > instructor1 = {name: "Aaron", location: "Atlanta", hair: "Red",
awesome_level: 9}
 => {:name=>"Aaron", :location=>"Atlanta", :hair=>"Red", :awesome_level=>9}
```
Notice how what Ruby spit back at us is formatted with the colon on the right and hash rocket on left. Different ways to write the same thing.

Symbols are actually considered another Data Type, and can be stored within variables - but should not be confused for variables themselves!

**When To Use Array vs. Hash?**
Arrays usually contain items that are just one sort of thing
	* Answers to a multiple choice quiz (["A", "C", "B", "A"])
	* Just names of people (["Betsy", "Zack", "Katie"])
Hashes contain items that require a description
	* Profiles of People ({""})


### Loops
In Ruby, code blocks are chunks of code that come between the keywords **do** and **end**. Some blocks come pre-defined, with particular rules. These blocks we can consider "loops": they check if a condition is true, then perform a block of code until the condition becomes false
(through some manipulation in the code block).

###.times Loop
This loop does exactly what it sounds like: *do* something some *value of times*.

```ruby
3.times do
   puts "Beetlejuice"
end

# you can also use a variable:
num = 4

num.times do
  puts "Something clever."
end
```

This loop only takes integers! A string has no a numerical value (nil), and how exactly would you suggest performing an action 4.5 times?

#### .times Activities
These should be simple enough that you can ask the students how to write them. Create a file called **times_loop.rb** and do both activities within (you can comment out the first when doing the second).

* The Little Coder That Could: Print out "I think I can" 5x!
```ruby
# times_loop.rb

5.times do
	puts "I think I can!"
end
```

* Times Square: Initiate a variable called 'count' at 0. For ten times, display the square of 'count', and then increment each time by 1.

(You may have to inform them how to do exponents for this one.)
```ruby
# times_loop.rb

# 5.times do
# 	puts "I think I can!"
# end

count = 0

10.times do
	puts count**2
	count += 1
end
```

### until Loop
In an until loop, a variable is incremented or decremented, and an action is performed until a certain condition true.

Example:
```ruby
num = 1

until num == 10
  puts num
  num += 1
end
```
While we loop, we increment the variable, so that 'num' will eventually equal 10. Otherwise, we'd have created an infinite loop, and it will just be printing "1" forever.

#### until Activities
These activities you may want to lead the students through, and create separate files for.

* Ask the user for a number (1-10), print the doubles of their number through 10. (Make sure you include the double of 10!)

Possible Solution (**doubles.rb**):
```ruby
puts "Give me a number between 1 and 10: "
num = gets.chomp.to_i

until num > 10
	puts num*2
	num += 1
end
```

* Now reverse it! Ask for again for a number between 1 and 10, then count down to 0.
In same file:
```ruby
puts "Give me a number between 1 and 10: "
num = gets.chomp.to_i

until num == 0
	puts num*2
	num -= 1
end
```

* Until Dad says yes, keep asking him if we can go to Itchy and Scratchy Land (or Mt. Splashmore).

This one is trickier, but we don't increment or decrement, we keep asking for input.

Possible Solution (**askdad.rb**):
```ruby
puts "Can we go to Mt. Splashmore?"
ans = gets.chomp.downcase

until ans == "yes"
	puts "Can we go to Mt. Splashmore?"
	ans = gets.chomp.downcase
end

puts "Yay! You're the best!"
```

What if Dad answers affirmatively, but not with "yes"? We can use && or || with loops:
```ruby
puts "Can we go to Mt. Splashmore?"
ans = gets.chomp.downcase

until ans == "yes" || ans == "yep" || ans == "sure thing"
	puts "Can we go to Mt. Splashmore?"
	ans = gets.chomp.downcase
end

puts "Yay! You're the best!"
```

### while Loop
The while loop is pretty similar to the until loop, just testing the parameters in a different way. Let's convert the above until loop into a while loop:
```ruby
num = 1

while num < 10
  puts num
  num += 1
end
```
It's really all about what type of comparator you're using!

#### while Activities
Once again, you may have to lead the students through these, but ask for their input first.

* Pretend the computer is being passed around during class introductions. The student at the very back is named Jacob, so have your program take names until his name is entered.

Possible Solution (**student_names.rb**):
```ruby
puts "What's your name?"
name = gets.chomp.downcase.capitalize

while name != "Jacob"
	puts "What's your name?"
	name = gets.chomp.downcase.capitalize
end

puts "I guess that's everyone. Let's start class."
```
The last *puts* line is a way for us to know that the *while* loop has ended, instead of just having the program end

* Write a loop that continues to display random numbers between 1 and 10 until the number generated is 7.

Possible Solution (**random_numbers.rb**):
```ruby
num = rand(1..10)

while num != 7
	puts num
	num = rand(1..10)
end
```

### .each Loop/Iterator
There is one "loop" that is specific to Ruby data collections: the .each block!
This is likely the "loop" we'll use the most when moving into Rails projects. The .each block works on both Arrays & Hashes, though in slightly different ways.
Let's take a look at them individually...

### Array.each
Here is how an .each iterator through an Array is formed:
```ruby
my_array = [1,2,4,5]
my_array.each do |x|
    puts x
end
```

When run in Terminal:
```sh
$ ruby number_array.rb
1
2
4
5
```

The 'x' in the pipes (||) represents each item in the array.
You can choose to call this variable anything you like, but it should probably be descriptive of what sort of thing the items in the Array are.
For example: iterating through an Array of cars would like this:
```ruby
cars = ["Ford", "Toyota", "Honda", "Fiat"]
cars.each do |car|
	# something done in here
end
```

### Array.each_with_index
A maybe less-often-used version of the .each iterator includes an index count.
```ruby
peeps = ["Jane","Luke","Francis","Martha","Jimbo"]

peeps.each_with_index do |name, index|
    puts "#{name} is number #{index+1}!"
    #remember, index count starts at 0    
end
```

When run in the Terminal:
```sh
$ ruby names_with_index.rb
Jane is number 1!
Luke is number 2!
Francis is number 3!
Martha is number 4!
Jimbo is number 5!
```

#### Array.each Exercises
These exercises are tied to one file (**array_example.rb**). The slides are paced so that you do things in steps...

* Create an Array of animals
```ruby
animals = animals = %w(cat dog rhinoceros flamingo kangaroo)
```

* Use .each to iterate through and print out the contents of your array.
```ruby
animals = animals = %w(cat dog rhinoceros flamingo kangaroo)
animals.each do |animal|
	puts animal
end
```

* Set an animal as your "favorite", if your favorite animal is in the array, print to the screen: "I love [that animal]!"; if it's not in there, print to screen: "No, I don't care for those."

For this part, initially write it so it doesn't work quite right:
```ruby
animals = animals = %w(cat dog rhinoceros flamingo kangaroo)
favorite = "flamingo"

animals.each do |animal|
	if animal == favorite
		puts "I love #{animal}!"
	else
		puts "No, I don't care for those."
	end
end
```
This will result in printing out the if statement and the else statement (four times!). We just want one of the other (and certainly not multiple of one). Let's use a boolean to check if the "favorite" is within the array.
```ruby
animals = %w(cat dog rhinoceros flamingo kangaroo)

favorite = "flamingo"

presence = false

animals.each do |animal|
	if animal == favorite
		presence = true
	end
end

if !presence # or, presence != true, presence == false
	puts "No, I don't care for those."
else
	puts "Oh, I love #{favorite}!"
end
```

* What would be a way of doing this without the .each iterator?
We could just use **.include?**!
```ruby
animals = %w(cat dog rhinoceros flamingo kangaroo)

favorite = "flamingo"

if animals.include?(favorite)
	puts "Oh, I love #{favorite}!"
else
	puts "No, I don't care for those."
end
```

### Hash.each
Iterating through a Hash, you must pass two values in the pipes (||), whether or not you intend to use both. Here is how an .each iterator through a Hash is formed:
```ruby
state1 = {"Name"=>"Georgia","Capital"=>"Atlanta", "Population"=>10097343,"Area"=>59425}

state1.each do |key, value|
  puts "#{key}: #{value}"
end
```

When run in Terminal:
```sh
$ ruby state_hash.rb
Name: Georgia
Capital: Atlanta
Population: 10097343
Area: 59425
```

As with the Array, the variables in the pipes (||) could be called anything you like, but in this case, "key" & "value" may be the most descriptive (and precise).

#### Hash.each Exercises

* Create a Hash with keys "Name", "Age", "Hometown" and "Favorite Food". Ask the user for the values! Iterate through the Hash and introduce this person.

Using the iterator is obviously not the DRY-est way of doing this, but it serves as an example to differentiate Array iterating for Hash iterating.

Possible Solution (**introduce_yourself.rb**)
```ruby
puts "Tell us about yourself..."

profile = {}

puts "What's your name?"
profile["Name"] = gets.chomp

puts "What's your age?"
profile["Age"] = gets.chomp

puts "What city did you grow up in?"
profile["Hometown"] = gets.chomp

puts "What's your favorite food?"
profile["Favorite Food"] = gets.chomp

profile.each do |key, value|
	case key
		when "Name"
			puts "This is #{value}."
		when "Age"
			puts "They are #{value}-years-old." 
		when "Hometown"
			puts "They grew up in #{value}."
		else
			puts "Their favorite food is #{value}."
	end
end
```

#### More .each Exercises

* Ask the user for 5 numbers, store them in an array, then find the sum, product, largest, and smallest of those numbers.

Possible Solution (**five_numbers.rb**):
```ruby
puts "Give me five numbers: "
numbers = gets.chomp
# they can give all the numbers at once, hopefully with a space between each
numbers = numbers.split
# then we convert the string into an Array

# then we run through the array and convert the items to Integers
numbers.each_with_index do |num, index|
	numbers[index] = num.to_i
end

# other option is to use a .times loop and ask 5 times for a number, pushing them to an Array as we ask

sum = 0
product = 1

numbers.each do |num|
	sum += num
	product *= num
end

puts "The sum is #{sum}"
puts "The product is #{product}"
puts "The smallest number is #{numbers.sort.first}"
puts "The largest number is #{numbers.sort.last}"
```

* You are a car dealer - create a hash of vehicles: The model is the Key, the make is the Value (Keys must unique, Values can repeat). Ask the customer what car (model) they are looking for, use the Hash to determine if you have that vehicle, and what make it is (e.g., "Oh, you're looking for a Civic? Our Honda selection is right over here...").

Possible Solution (**carlot.rb**):
```ruby
vehicles = {"Prius"=>"Toyota", "CRV"=>"Toyota", "Civic"=>"Honda", "Fusion"=>"Ford", "Veyron"=>"Bugati"}

puts "What car are you looking for today?"
car = gets.chomp

in_stock = false
vehicles.each do |vehicle|
	if vehicle == car
		in_stock = true
	end
end

if in_stock
	puts "Ah, yes, our selection of #{vehicle[car]} vehicles is right over here!"
else
	puts "Sorry, we're all out of stock at the moment."
end
```	
