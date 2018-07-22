# Ruby: Day Five
## Code-Along: The Bank

Today, we are going to code a full-fledged Ruby program - a bank/ATM interface system that creates Accounts, and let's us view and modify existing Accounts.

We will call this program THE BANK! (or **the-bank.rb**)

This will be an ATM-like program that helps customer sign-in, create an account, review that account, make desposits and withdrawals, and possibly more...

--------

1. Homework Review (refer to solutions markdown)
2. Live-Coding / Code Along
	* No slide deck for this
	* Instructor: see guide & possible code below

--------

### Instructor Guide

	Today is meant to give students an idea of the coding process. Review the code provided below, and refer to it when needed in class, but try not to just have it open and teach off what's already been written. It can be beneficial for the students to see you code as they look on, and to stop coding and think, and write out some thoughts about what you're about to code.

	Please also feel free to change anything you feel needs changing, but try to keep it in line with what has been taught over the last four days. If you introduce a new subject, make sure it doesn't become the main focus, and that there's enough time to properly cover it as well as finish this program.

### The Code
	
	See comments for explanations.

	This is a dense program, so let's go ahead and introduced the idea of storing some code in separate files, and bringing in to another file using "require_relative".
	Have the students create a folder, and two files within:

**bank_classes.rb**
```ruby
# Let's start off by defining classes.
# We will have two: Customer & Account.
# A Customer will be tied to an Account
# through a "customer" attribute of Account

# pretty standard stuff for the Customer class:
class Customer

	attr_accessor :name, :location

	def initialize(name, location)
		@name = name
		@location = location
	end

	# We could add more about a customer,
	# but in the interest of time,
	# we'll leave it at this.
end

class Account

	# Two things we want to be read
	# and not freely edited: Acct Num & Balance
	# For balanace we'll create methods
	# to all for more code to be within
	# the class definition, rather than out
	# in the free-standing methods.
	attr_reader :acct_number, :balance
	attr_accessor :customer, :acct_type

	def initialize(customer, account_number, account_type, balance)
		@customer = customer
		@balance = balance
		@account_number = account_number
		@account_type = account_type
	end

	# As mentioned above, if we can move some
	# code into the class definition, that's best.
	# So we'll have "balance", "deposit" and "withdrawal" methods

	def deposit
		puts "How much would you like to deposit?"
		print "$"
		amount = gets.chomp.to_f
		@balance += amount
	end

	def withdrawal
		puts "How much would you like to withdraw today?"
		print "$"
		amount = gets.chomp.to_f
		# Check if there are sufficient funds!
		# (We can program in overdraft protection later...)
		if @balance < amount
			#if not, charge overdraft fee of $25
			@balance -= (amount + 25)
			puts "You were accessed an overdraft fee of $35 for having insufficient funds in your account"
		else
			@balance -= amount
		end
	end
	
	def balance
		puts ""
		puts "The current balance in your account is $#{'%0.2f'%(@balance)}."
		puts ""
	end	
end
```

**the_bank.rb**
```ruby

# we need the code from bank_classes.rb,
# so we call it into this file using
# "require_relative" - which will look
# in the current folder/directory

require_relative 'bank_classes'

# we're going to create Arrays to
# collect both types of Objects:
@customers = []
@accounts = []


# Let's create a method that will act
# as a sort of "home screen" for the
# program - it welcomes and asks for direction

def welcome_screen
	@current_customer = ""
	
	# What is this about? We're creating a way
	# to save the currently "signed in" customer
	# and allows us access to that variable without
	# having to pass it through every proceding
	# method that is called after its set.

	puts "Welcome to Tech Talent Bank"
	puts "_________________________________"
	puts "Please choose from the following:"	
	puts "1. Customer Sign-In"
	puts "2. New Customer Registration"
	choice = gets.chomp.to_i

	case choice
		when 1
			sign_in
		when 2
			sign_up("","")
		else
			puts "Invalid selection."
			welcome_screen
	end
end


# this is for a user who has
# already gone through registration (sign_up)
def sign_in
	puts "Sign In"
	puts "_________________________________"
	print "What's your name? "
	name = gets.chomp.downcase
	print "What's your location? "
	location = gets.chomp.downcase

	# Are there even any customers at all?
	if @customers.empty?
		# There are no customers, let's just sign up customer no. 1
		puts "No customer found with that information. Registering now..."
		sign_up(name, location)
		# See, that's why it takes arguments!
	end

	# Okay, there are customers...
	# but is this user's info correct?
	customer_exists = false
	@customers.each do |customer|
		if name == customer.name && location == customer.location
			@current_customer = customer
			customer_exists = true
		end
	end

	# Notice that we saved the current customer to
	# @current_customer - that way we can access that
	# variable thoroughout the proceding methods.
	# Once the user chooses to return to the welcome screen,
	# that variable will be reset to an empty String,
	# effectively, signing them out.

	if customer_exists
		# They gave us matching info to our records,
		# let's go ahead...
		puts "Welcome back name.capitalize"
		account_menu
	else
		# Not finding a record of that:
		# try it again or you don't really have an
		# account so Sign Up?
		puts "No customer found with that information."
		puts "1. Try again?"
		puts "2. Sign Up"
		choice = gets.chomp.to_i

		case choice
			when 1
				sign_in
			when 2
				sign_up(name, location)
				# There! Happened again!
		end
	end
end


def sign_up(name, location)
	if name == "" && location == ""
		puts "Sign Up"
		puts "_________________________________"
		print "What's your name? "
		name = gets.chomp.downcase
		print "What's your location? "
		location = gets.chomp.downcase
	end

	# Time to create a new instance of a Customer!
	# We're also going to save them in an instance variable,
	# so we don't have to keep passing an argument down
	# the line of methods.
	
	@current_customer = Customer.new(name, location)

	# And save them in our customer collector!

	@customers.push(@current_customer)
	
	puts "Registration successful! Welcome #{@current_customer.name.capitalize}"
	
	# And now we can move on to dealing with Accounts...
	account_menu
end


# Creating/Looking Up Account - what will it be?
def account_menu

	puts "_________________________________"
	puts "Account Menu"
	puts "_________________________________"
	puts "1. Create an Account"
	puts "2. Review an Account"
	puts "3. Sign Out"

	choice = gets.chomp.to_i

	case choice
		when 1
			create_account
		when 2
			review_account
		when 3
			sign_out
		else
			puts "Invalid selection."
			account_menu
	end
end

# Let's create a new instance of an Account...
def create_account
	puts "_________________________________"
	puts "Create an Account"
	puts "_________________________________"
	puts ""
	print "How much will your initial deposit be? $"
	amount = gets.chomp.to_f

	print "What type of account will you be opening? "
	account_type = gets.chomp.downcase

	# The account number will be based on how many accounts 
	# we have. So we'll check the length of the array before
	# we put this new Account into it, and add by one.
	new_acct = Account.new(@current_customer, account_type, (@accounts.length+1), amount)
	@accounts.push(new_acct)
	puts "#{account_type.capitalize} account created successfully!"

	# Now we'll go back a step, so they can stay signed-in,
	# and either create another Account or review the one(s)
	# already created.
	account_menu
end

# We can look up the account using who they are signed-in as
# and what type of Account they want to review...
def review_account
	@current_account = ""
	print "Which account (type) do you want to review? "
	type = gets.chomp.downcase

	account_exists = false
	@accounts.each do |account|
		if @current_customer = account.customer && type == account.account_type.downcase
			@current_account = account
			account_exists = true
		end
	end

	if account_exists
		account_actions
	else
		puts "An account of that type does not exist for #{@current_customer.name.capitalize}"
		puts ''
		puts "1. Return to Account Review"
		puts "2. Create Account"
		puts "3. Sign out"
		choice = gets.chomp.to_i

		case choice
		when 1 
			review_account
		when 2
			create_account
		when 3
			sign_out
		else
			puts "Invalid selection"
			review_account
		end
	end
end

# Alright... we have an Account.
# Now what are we going to do with it?
def account_actions
	puts "Choose From the Following:"
	puts "_________________________________"
	puts "1. Balance Check"
	puts "2. Make a Deposit"
	puts "3. Make a Withdrawal"
	puts "4. Return to Account Menu"
	puts "5. Sign Out"
	choice = gets.chomp.to_i

	case choice
		when 1
			# This method is built into the class:
			@current_account.balance
			account_actions
		when 2
			# This method is built into the class:
			@current_account.deposit
			@current_account.balance
			account_actions
		when 3
			# This method is built into the class:
			@current_account.withdrawal
			@current_account.balance
			account_actions
		when 4
			# Go back a step:
			account_menu
		when 5
			# Sign Out:
			sign_out
		else
			puts "Invalid selection."
			account_actions
	end
end

# gives customer time to review info before proceeding to next task
def continue
	puts ""
	puts "1. to continue."
	puts "2. to sing out"
	choice = gets.chomp.to_i

	if choice == 1
		return true
	elsif choice == 2
		sign_out
	else
		puts "Invalid selection"
		choice
	end
end

def sign_out
	puts ""
	puts "Thank you for using Da Bank!"	
	puts ""
	# sleep(2)
	welcome_screen
end

# Let's get it started...
welcome_screen
```

Demonstrate [Python Tutor](http://www.pythontutor.com/) {:target="blank"}

```ruby
marker = false
i = 0
while marker == false
 if i < 4
   puts "I think I can"
 else
   marker = true
 end
 i += 1
end
```
