# Ruby: Day Four
## Classes

Learning about what a Ruby Class is, what an Object refers to, and how to create your own.

## Useful Resources
[Official Ruby Docs](http://ruby-doc.org/)
[Code Academy's Ruby Course](https://www.codecademy.com/learn/ruby)

---

1. Homework Review (refer to solutions markdown)
2. Ruby Review
3. Object vs. Class
4. Data Types == Classes
5. Creating Classes
	* Initialize Method
	* Instance Variables
	* Class Exercises
6. Accessing Object Data
	* Class Methods
	* Attribute Accessors
7. More Class Exercises
8. Collecting Objects
9. User-Created Objects
10. In-Class Activity: Team Tournament

---

### Ruby Review

1. What elements is a String made up of?
*Characters*

2. What would you use if you wanted to check a numerical range in an If statement?
*Two possible answers:*
	* two comparisons on either side of an AND (&&)
	* .include? on a Range (0..10)

3. How do you remove the last item in an Array?
*.pop*

4. What two data types are best used for keys in a Hash?
*String* and *Symbol*

5. How many arguments can a method take?
*Trick question! No limit!*

6. What’s a method? What’s it good for?
*A block of code that acts like a variable. Keeping code dry.*

7. How do you get a value back from a method?
*return*

8. How do you write a one-line if/else statement?
*condition ? code if true : code if false*


### Object vs. Class
**SHOW first 3 slides**

An object is a piece of data.

A class is what type of data that is.

| Object        	| Class        					 |
| ----------------|:----------------------:|
| 4      					| Integer (or Fixnum)		 |
| "what?"					| String 					 			 |
| true      			| Boolean								 |
| [8,15,16,23,42] | Array				 					 |

When we talk about an object we can talk about its' properties and functions.

Marker
	Properties
		Color
		How much ink
	Functionality
		Write
Cat
	Properties
		Name
		Color
		Fullness (internal)
	Functionality
		Digest (internal)
		Lounge
		Scratch stuff

You can think of objects in programming the same way:

String
	Properties
		the character data e.g. "Money"
	Functionality
		.reverse
		.upcase
		.split()
		
We can make our own objects.  First we need to make a Class to create the objects.
A class is a definition of what is in a n object

### Data Types == Classes
So in Ruby, the different data types are considered classes
(whereas any instance of that data type is an object)
 
There's even a way to tell what class and object is:
```sh
> 2.class
 => Fixnum
> "Two".class
 => String
> nums = [1,2,3]
 => [1,2,3]
> nums.class
 => Array 
```

We also have the ability to create our own classes!


### Creating Classes
We define an Class using... well, "class" (and there's end to tell Ruby when the definition is over).

```ruby
class Thing

end
```

Notice that "Thing" - what the class is to be called - is capitalized.
And then we fill in with our methods:
```ruby
class Thing

	def method1
	end

	def method2
	end

end

There's one method that is a must: **initialize**. This method will allow for the creation of a new Object (a new instance of this Class).

```ruby
class Object

  def initialize(attr1, attr2)
    @attr1 = attr1
    @attr2 = attr2
  end

end
```

We pass the method attributes as arguments, and set these attributes to corresponding instance variables (variables preceded by an @-symbol). Instance variables can be used throughout the class definition.

#### Variables: Instance vs. Local

What's the difference?

*Local variable*:
```ruby
instructor = "Nick"

def display_name
  puts instructor
end
```
This will result in an error: "variable name inside the method is undefined." We would need to pass the outside definition of 'name' to the method for anything to happen.

```ruby
instructor = "Nick"

def display_name(x)
  puts x
end

display_name(instructor)
```

A poor alternative is to use an instance variable

```ruby
@name = "Nick"

def display_name
  puts @name
end
```

The instance variable can be seen inside the method without passing it as an argument. While this is possible, it is not practical (or recommended). Why? Because that method is not bound to that specific variable. Whereas if we pass the method an argument, any variable (and the values within) could be used.

#### Class Exercises

*Create a new file called **class_examples.rb**.
Let's start with a full example:
```ruby
class Person

  def initialize(name, age)
    @name = name
    @age = age
  end
end

my_profile = Person.new("Aaron", 34)
```

*Now let the ask for input from the students on creating these classes:*

* Create a User class.
```ruby
class User
	def initialize(email, password, username)
		@email = email
		@password = password
		@username = username
	end
end
```

* Create a Pet class.
```ruby
class Pet
	def initialize(name, age, species)
		@name = name
		@age = age
		@species = species
	end
end
```

* Create a Product class.
```ruby
class Product
	def initialize(name, price, quantity, brand)
		@name = name
		@price = price
		@quantity = quantity
		@brand = brand
	end
end
```


### Accessing Object Data
We need a way to access the data in our new Object, and there's actually two ways to go about it. The hard way, is creating a method to call each instance variable.

```ruby
class Person

    def initialize(f_name, age)
        @f_name = f_name
        @age = age
    end

    def name
        @name
    end

    def age
        @age
    end

end

my_profile = Person.new("Aaron", 34)

puts "Hi, I am #{my_profile.name} and I am #{my_profile.age}-years-old."
class Person

  def initialize(f_name, age)
    @f_name = f_name
    @age = age
  end

  def f_name
    @f_name
  end

  def age
    @age
  end

end

my_profile = Person.new("Aaron", 34)

puts "Hi, I am #{my_profile.f_name} and I am #{my_profile.age}-years-old."
```

The "name" and "age" methods are just returning those values.

And then creating methods that let you modify the data:

```ruby
# added to the Person class:
  def older
    @age += 1
  end

  def change_name(new_name)
    @f_name = new_name
  end
```

Then we can run code like this:

```ruby
my_profile = Person.new("Aaron", 33)

puts my_profile.age

my_profile.older

puts my_profile.age

puts "I am no longer #{my_profile.f_name}..."

my_profile.change_name("King Ruby")

puts "My name is now #{my_profile.f_name}."
```

The easy (or, at least, DRY-er) way can be approached with three different keywords:

| Keyword        	| Meaning        					              | 
| ----------------|:-------------------------------------:|
| attr_reader     | Allows you to only read the data.     |
| attr_writer			| Allows you to only re-write the data. |
| attr_accessor   | Allows you to read and write data.    |

Following those keywords would be one or more of your attributes, written as symbols (same variable name, but with a colon in front).

#### When to Use?

**attr_reader** would be good for attributes that you do not want changed (like ID #s).

**attr_writer** would be for some situation where you want to the ability to override data, but not read it back.

**attr_accessor** would be the combination of the other two: the power to read and the power to override.

So if we use attr_accessor with the Person object:
```ruby
class Person

  attr_accessor :name, :age

  def initialize(name, age)
    @name = name
    @age = age
  end

  # we can get rid of all the other methods

end

my_profile = Person.new("Aaron", 33)

puts "I am no longer #{my_profile.name}..."

my_profile.name = "King Ruby"

puts "My name is now #{my_profile.name}."
```


### More Class-y Exercises
*As in previous in-class exercises, get input from students to see what they've picked up from examples. Create a new file or continue using **class_examples.rb**.*

* Create methods within the Product class to control quantity.

Possible solution (point out both ways - attr_accessor & methods):
```ruby
class Product
	
	attr_accessor :name, :price, :quantity, :brand

	def initialize(name, price, quantity, brand)
		@name = name
		@price = price
		@quantity = quantity
		@brand = brand
	end
	
	sale_item = Product.new("tv", 499.99, 13, "Samsung")

	puts sale_item.qty

	sale_item.qty = 34
	puts sale_item.qty

	# if we didn't have the attr_accessor
	# we would need two methods...
	# one for when product is sold:
	# def qty_sold(amount = 1)
	#	 @quantity -= amount
	# end

	# another for when more stock comes in:
	# def qty_recieved(amount = 1)
	# 	@quantity += amount
	# end
end

sale_item = Product.new("tv", 499.99, 13, "Samsung")
puts sale_item.qty
sale_item.qty(34)
puts sale_item.qty
```


* Create a method for the Pet class to return the animal's sound.

Possible Solution (importing something like what we did in the first Ruby lesson into a class method):
```ruby
class Pet
	attr_accessor :name, :age, :species

	def initialize(name, age, species)
		@name = name
		@age = age
		@species = species.downcase
	end

	def sound
		case @species
			when "cat" then puts "Meow!"
			when "dog" then puts "Woof!"
			when "snake" then puts "Hiss!"
			when "fish" then puts "[bubbling sounds]"
			else puts "Rawr?"
			# could go own for a while, but we'll call it there.
		end
	end
end
```


* Create a brand new class: Vehicle What should the attributes be? What methods should we create?

Possible Solution:
```ruby
class Vehicle

	attr_accessor :make, :model, :year, :color, :quantity

	def initialize(make, model, year, color, quantity)
		@make = make
		@model = model
		@year = year
		@color = color
		@quantity = quantity
	end

	# possible method:
	def full_profile
		"#{@color} #{@year} #{@make} #{@model}"
	end
	# easier to write vehicle.full_profile
	# than all the vehicle.attributes separately
end
```


### Collecting Objects
Custom class objects can be handily stored in Arrays!

```ruby
class Person
    #bunch-o-code in here
end

all_the_people = []

new_profile = Person.new("Gayle", 33)

all_the_people.push(my_profile)

new_profile = Person.new("Frank", 59)

all_the_people.push(my_profile)
```

We can continue to use the same variable to create new instances of the Person object, and then push them into an array for storage.

How could we use this approach, along with a loop, to create a collection of objects created by the user? (See next section!)


### User-Created Objects
Letting the use provide data for our objects, and saving the objects in an array:

```ruby
class Person
    #bunch-o-code in here
end

all_the_people = []
completion = ""

puts "Enter personnel data. Type 'done' when finished."
while completion != "done"
    print "Name: "
    name = gets.chomp
    if name.downcase = "done"
    	completion = "done"
	# break exits out of the innermost loop
	break
	# # or use return to exit out of the entire method. you'll need to put your final message in here tho
	# puts "Personnel entry complete!"
	# return
    end
    print "Age: "
    age = gets.chomp
    profile = Person.new(name, age)
    all_the_people.push(profile)
    puts "Profile saved."
end

puts "Personnel entry complete!"
```

Now to retrieve this data, it seems like we could just puts the array, but that doesn't work.  We can do this:

```ruby
	puts all_the_people[0].f_name
```

And that will call the name method on the person object in the first position of the array. To put out all the data we need to iterate:

```ruby
	all_the_people.each do |person|
		puts person.f_name
	end
```


Try it with Pets (*ask for student input*):
```
class Pet
	attr_accessor :name, :age, :species

	def initialize(name, age, species)
		@name = name
		@age = age
		@species = species
	end

	def sound
		case @species
			when "cat" then puts "Meow!"
			when "dog" then puts "Woof!"
			when "snake" then puts "Hiss!"
			when "fish" then puts "[bubbling sounds]"
			else puts "Rawr?"
			# could go own for a while, but we'll call it there.
		end
	end
end

pets = []
completion = false

puts "Tell us about your pets. Type 'done' when finished."
while !completion
  print "Name: "
  name = gets.chomp
  if name.downcase == "done"
  	completion = true
  	break
  end
  print "Age: "
  age = gets.chomp
  print "Species: "
  species = gets.chomp.downcase
  pet = Pet.new(name, age, species)
  pets.push(pet)
  puts "Pet saved!"
end

puts "That's all the pets!"
```


---


### In-Class Activity: Team Tournament
*Not in slides; see comments in code for explanations*

Help match up teams for the first round in a seed-based tournament In a seeded tournament, and during the first round, the top seed is matched with the bottom seed, the 2nd highest team is matched with the second lowest, etc.  

Example:

Seeds
1. Wisconsin
2. Michigan
3. Michigan State
4. Indiana

Matchups:
(1) Wisconsin versus (4) Indiana,
(2) Michigan versus (3) Michigan State 

Create the first round matches for a tournament using seeds.

Your program should include a menu, where you can enter teams, and then seed them.  

Example: 

Welcome to My Tournament Generator. Enter a selection:
1. Enter teams
2. List teams
3. List matchups
0. Exit program
 

Your program should check for the following: 

1. If an odd number of teams are entered, the top-seeded team gets a bye (doesn't play)

Example:

Seeds
1. Wisconsin
2. Michigan
3. Michigan State
4. Indiana
5. Purdue

Matchups: 
(1) Wisconsin has a bye
(2) Michigan versus (5) Purdue
(3) Michigan State versus (4) Indiana

Hints:
You should utilize Classes (probably just one).
Make sure you can do multiple match-ups (that is, match up once, then come back and match-up again).

```ruby
# First we define a class to work with:
class Team

	attr_accessor :name, :ranking

	def initialize(name, ranking)
		@name = name
		@ranking = ranking
	end

end

# next, we'll initiate an Array
# to save Team objects in
@teams = []

# Now we can start defining methods
# to be used. Since we asked that the
# various aspects of the program be re-usable,
# we'll need to utitlize methods.

# We'll start with the "menu" method:
def menu
	puts "Welcome to My Tournament Generator. Enter a selection:"
	puts "1. Enter teams"
	puts "2. List teams"
	puts "3. List matchups"
	puts "0. Exit program"

	choice = gets.chomp.to_i

	# use case or if/elsif
	case choice
		when 1
			clear_screen # see very next method defined!
			enter_teams
		when 2
			clear_screen
			list_teams
		when 3
			clear_screen
			matchups
		when 0
			clear_screen
			puts "Okay, catch ya later."
		else
			clear_screen
			# return to the top of this method
			# if given a number not listed in menu
			puts "Not a valid selection. Try again, please."
			menu
	end
end

# here is a method to clear the screen
# no matter what platform you're running
# this program on.
def clear_screen
	Gem.win_platform? ? (system "cls") : (system "clear")
end

# if we want to go back to the menu
# at any point: 
def return_to_menu
	puts "Return to Menu? [y/n]"
	ans = gets.chomp.downcase

	case ans[0]
		when "y"
			clear_screen
			menu
		when "n"
			clear_screen
			puts "Okay. Well...see you around, then. I guess."
		else
			clear_screen
			puts "Huh? Yes or No, please."
			return_to_menu
	end
end

# a method for creating new teams,
# and storing them in the @teams Array
def enter_teams
	puts "Enter team names & rankings. When finished, type in 'done' instead of team name."

	name = ''
	while name != 'done'
		print "School Name: "
		name = gets.chomp
		if name.downcase == 'done'
			break
		end
		print "Ranking: "
		ranking = gets.chomp.to_i
		
		# what if the user enters
		# team with the same ranking
		# as one already entered?
		while rank_check(ranking) # check below for this method
			puts "There's already a team with that ranking."
			print "Please check your facts and re-enter ranking: "
			ranking = gets.chomp.to_i
		end
		
		@teams.push(Team.new(name, ranking))
	end
	# sorting the teams by ranking:
	@teams.sort! { |a,b| a.ranking <=> b.ranking }
	#@teams = @teams.sort_by {|team| team.ranking}
	return_to_menu
end

# a method to see if the ranking
# entered for a new Team is the
# same as one for a Team already
# in our @teams Array.
def rank_check(num)
	ranking_exists = false
	@teams.each do |t|
		if num == t.ranking
			ranking_exists = true
		end
	end
	return ranking_exists
end

# a method to simply list out
# the Teams in our Array
def list_teams
	puts "Here are the Teams..."
	# new thing! sleep() tells your program
	# to wait a certain amount of seconds
	# (passed as argument) before continuing
	sleep(1)
	puts "---------------------"

	@teams.each do |team|
		puts "#{team.ranking}. #{team.name}"
	end
	return_to_menu
end

# finally, a method to perform
# the team matchups
def matchups
	# we will create a temporary array
	# for use only in this method,
	# so we don't effect the original
	# @teams array with an deletions
	temp_arr = []
	@teams.each do |t|
		temp_arr.push(t)
	end

	if temp_arr.length % 2 != 0
		# check if there is an odd number of teams,
		# if there is an odd number, we pull pull
		# out the top one and give them a bye week
		puts "(1) #{temp_arr.delete_at(0).name} has a bye"
		# and now we have an even number of teams
	end

	# match up the even amount of teams,
	# assuming there are any teams...
	while temp_arr.length > 0
		team1 = temp_arr.delete_at(0)
		# team 1 is the first one in array
		team2 = temp_arr.pop
		# team 2 is last in array
		puts "(#{team1.ranking}) #{team1.name} versus (#{team2.ranking}) #{team2.name}"
		# this continues until no teams are left in the array
		# Example: 
		# [1,2,3,4,5,6]
		# team1 = 1
		# team2 = 6
		# [2,3,4,5]
		# team1 = 2
		# team2 = 5
		# [3,4]
		# team1 = 3
		# team2 = 4
	end

	return_to_menu
end

# let's get this thing going!
# call the "menu" method to
# start things off!
menu
```
