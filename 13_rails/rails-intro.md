# Intro to Ruby on Rails

Useful Resources
[Official Rails Site](http://rubyonrails.org/)
[Official API of RoR](http://api.rubyonrails.org/) - Provides an explanation of every class and functionality.
[Rails Gem on GitHub](https://github.com/rails/rails)

---

1. Homework Review
2. What is Rails?
3. MVC Framework
4. Starting a New Rails App
 * Ground Rules
 * rails new
 * Why Do This?
 * What Does It Give Us?
5. Tour Through a Rail App
 * Gemfile
 * README.rdoc
 * Rakefile
 * app/
 * bin/
 * config/
 * db/
 * lib/
 * log/
 * public/
 * test/
 * tmp/
 * vendor/
6. Rails Server
7. Rails Generate
8. Creating a Controller
9. Routes
10. Views
    * Updating from template
    * Challenge: HTML update
11. New App!
12. Scaffolding
    * What Did That Do?
    * Migrating
13. Tour of the Views
14. Customizing the Index View
15. Deleting a Record

---

### What is Rails?
Rails is a Platform for creating web applications, using the Ruby programming language. Rails allows us to utilize Ruby in the Back-End and Front-End, and to collect and retrieve data from a database.

So, it is not a language unto itself - think of it as a skeleton (and organs, too, I guess) of a website, written in Ruby, that is then supplemented with the muscles and skin of HTML, CSS, and JavaScript (I'll let you decide which is which).


### MVC Framework

Model - View - Controller
(See diagram in the slide deck)

### Quick Notes

— Form Genration for rails 5.1 has changed from <%= form_with(model: @location, local: true) do |f| %> to <%= form_with(model: @location, local: true) do |**Form**| %>. To get f.text or f.field …etc to work properly you will have to change |form| to |f|


### Starting a New Rails App
To start a brand new Rails app, there is a simple command:

```sh
$ rails new my_awesome_project
```
There will be a bunch of messages printed out, showing the creation of many, many folders and files.

Then we have to 'cd' into the new directory:
```sh
$ cd my_awesome_project
```

Some ground rules for naming a Rails app:
1. Appropriately Name
   * what does the project do as a whole?
2. Starts with a letter
3. All lowercase
4. Underscores or dashes for spaces

#### Why Did We Do This?
Why didn’t we just open up Sublime and create each of the individual files for our application? Rails is just Ruby source code, and we all know how to use Ruby.

Well, first off, that would've been a major pain in the rear end. You got that kinda time to make all those files and folders? That's not what Rails is about - it's about doing things quickly!

And anyway, Rails does a lot of “magic” behind the scenes to get our applications up and running. It generates all of the necessary directories (folders) for us to write our code, but it will be up to us to populate these directories with the code that will make them run.

#### What Did We Get?
Make sure you're in your "my_awesome_project" directory and type "ls" (or "dir").

You'll see all the directories (and some individual files) that we just generated with one line of code! Each one is essential to creating a working application and many of them contain other subdirectories as well.


### Tour Through a Rail App

#### Gemfile
A gem is a Ruby program or library in a standard format available through RubyGems. 

Each gem has a name, version, and platform. For example, the rails gem has a 5.0.2 version (for now).

Your Gemfile is a list of all gems that you want to include in the project. It is used with bundler (also a gem) to install, update, remove and otherwise manage your gems.

#### Gemfile.lock

Ensures that the gem versions used to build the app, will be the ones used whenever it is deployed or downloaded on other machines. 

#### 	README.rdoc
This file contains basic details about the Rails Application and a description of the different directories in an application. 

This file can be converted to an .md file to display on Github.

#### Rakefile
Rake is a software task management and build automation tool.  This file helps with building, packaging and testing the Rails code. It will be used by rake utility supplied along with Ruby installation.  You can put code in here that can be run from the command line.
#### app/
This organizes your application components. It's got subdirectories that hold the html files (views), controllers (controllers), and the backend logic (models), as well as assets, mailers, and more!

#### bin/
This directory contains the commands and utilities that you use day to day. These are executable binary ruby files - hence the the directory name bin.  There's some overlap of functionality with executables that you could put in the rake file but with some subtle differences. Some of the commands we'll use in terminal are routed through here.

#### config/
The config/ contains the small amount of configuration code that your application will need, including your database configuration (in database.yml), your Rails environment structure (environment.rb), and routing of incoming web requests (routes.rb). The database connection and application behavior can be altered by the files inside this directory.

#### db/
Your Rails application will have model objects that interact with relational database tables. You can manage the relational database with code that you would add to this directory.

When we did the atm project, we had to keep logging in and creating accounts to test our code.  We weren't persisting (or saving) our data.

For right now, you probably on see the seeds.rb file.
The migrate folder and other files won't appear until
we first tell Rails to create a new table in our database.

#### lib/
This is a space for application specific libraries that aren't in Gem form. These include any kind of custom code that doesn’t fit in anywhere else. Think of these files as extended features for our application that aren’t available in Rubygems.

#### log/
The log files from your application. Literally everything we do with our application will be recorded here, including errors which can be useful for debugging.

A great place to see how it is working. You can also see your code run in terminal.

#### public/
Like the public directory for a web server, this directory has web files that don't change, such as JavaScript files (public/javascripts), graphics (public/images), stylesheets (public/stylesheets), and HTML files (public).

You should store your assets in the app/assets directory locally. In production, Rails precompiles these files to public/assets by default.

#### test/
All of the tests that Rails created for us as well as ones that we will write ourselves are stored here.

#### tmp/
The tmp directory holds temporary files.

Caching involves temporarily storing recently used information, which can improve application performance.

#### vendor/
Libraries provided by third-party vendors (such as security libraries or database utilities beyond the basic Rails distribution) go here.


### Rails Server
In order to run our shiny new web application we have to start our local server. We'll do this in the terminal.

```sh
$ rails server
```

Or, because coders are lazy:

```sh
$ rails s
```

And that laziness is a badge of honor - maybe it's not so much laziness as getting things done is shorter amount of time!

The output of "rails s" tells us a bunch of good stuff:
	* The version of Rails (5.0.2)
	* Where to browse our app locally (http://localhost:3000)
	* How to shutdown the server (Ctrl-C)

Go to Chrome and type in the URL bar: localhost:3000 (you can also use 0.0.0.0:3000). HOT TIP!: bookmark this URL! You'll be coming back here all the time. All. The. Time.

If all went well you should see Rails' heart-warming generic welcome page. We will soon replace this with our own custom page.


### Rails Generate
The rails generate command uses templates to create lots of things in Rails. By itself it generates a list of all available generators. Try it!

```sh
$ rails generate
```

And, once again, there's shorter way:

```sh
$ rails g
```

### Creating a Controller
You *need* a controller to have Views. Remember: Rails is an MVC Framework - and it can actually exist just fine without the M(odel), but it **NEEDS** the V(iew) and C(ontroller), or else it's just a shell.

Let's use the generator to build a Controller

```sh
$ rails g controller Welcome index about contact
```

g = generate
Welcome = the name of the controller
(but you can call it whatever you like; capitalized, camelcase)
index about contact = views (or, html pages) that you want associated with this controller (snake_case); important: no commas between views, just a space.


### Routes
When you generate a Controller, Rails provides the routes. Routes connects the URL entered (*domain/page*) and the controller/action pair (the specific controller and which part of it) associated with that page.

After generating our Welcome controller, here's what our Routes file (found in the "config" folder) looks like:
```ruby
# [railsprojectname]/config/routes.rb

Rails.application.routes.draw do

#using these default setting, rails automatically pairs with the correct action in the welcome controller
  get 'welcome/index'

  get 'welcome/about'

  get 'welcome/contact'
end
```

If a URL is formatted as "controller/action", it is assumed you want it pointing to that specific controller/action pair.  When I receive a Get Request for this address I’ll tell the controller to return the matching page in the views folder.

A Root path is your landing (or: home) page. Call it by it's associated controller action!

```ruby
Rails.application.routes.draw do

  root 'welcome#index'

  get 'welcome/about'

  get 'welcome/contact'

end
```

We can make URLs shorter, or more descriptive, with this format:
'custom_URL' => 'controller#action'

```ruby
Rails.application.routes.draw do

  get 'homepage' => 'welcome#index'

  get 'thisisus' => 'welcome#about'

  get 'drop_us_a_line' => 'welcome#contact'

  root 'welcome#index'

end
```

Also one page can have multiple URLs (see 'welcome#index' above).

Also note that setting a "root" route is shorthand for:
```ruby
get '/' => 'controller#action'
```

### Views
In Sublime, open up welcome/index.html.erb from the views folder.

OMG it's HTML again! You remember this, right? Go ahead and customize the <h1> and <p> tags, then reload your page.

```html
<!-- app/views/welcome/index.html.erb -->

<h1>Helen's Hopeful Homepage</h1>
<p>For now it's under construction...</p>
```

#### Challenge!
Might as well update the other 2 views we created!

	* Add dummy content to the about and contact pages
	* Customize the routes to be /about and /contact.

*Instructors: give students 5 minutes. Ask for raised hands when student is finished and look over their work.*

### Try It Again: New App
Let's take it again from the top. Re-build everything we just did in a new Rails app.
	* Call the new app: pet_store

	* Add a controller called "Welcome" with two corresponding views

	* Route a homepage

	* Add some content to the views

### Scaffolding
We're going to use Rails Generate to build what is called a "Scaffold", which generates a whole lot of good stuff.

```sh
$ rails g scaffold Pet name:string breed:string age:integer
```

This is strikingly similar to defining custom Classes in Ruby. In Rails, we refer to these as "Resources".

By the way: if you mistype something or hit enter too soon, 
that's OK. You can use Rail Destroy (or "rails d") to undo the Generate command.

```sh
$ rails destroy scaffold Pet
```
(and you don't have to re-write all the attributes)

Now, let's break down the Scaffold generator:

`rails g scaffold` - The Rails command to create a new table and controller in your database

`rails g scaffold Pet` - 'Pet' (notice it's capitalized, and singular), will be the name of our resource and table

`rails g scaffold Pet name:string 
breed:string age:integer` - 'name', 'breed' and 'age' will be the attributes of your 'Pet'; each is assigned a data type

Think of your Table as if you were drawing it on paper...
	* Your Attributes are the columns.
	* Each time you add a new Pet, a new row in the table is created.

Rails is a generous framework, and it gives three extra columns, just for stopping by:
	* ID - an integer from 1 thru ...
	* created_at - datetime when row was created
	* updated_at - datetime when the row was updated

Rails also handles routing for us after we scaffold a resource. We see one simple line added to routes.rb:

```ruby
resources :pets
```

That tiny bit of code actually maps to several pages. We'll explore what's going on behind the scenes here tomorrow.

For now, browse to:
localhost:3000/pets
(notice pets is plural and lowercase)

Did you get a Red Error Screen?
Don't worry. All hope is not lost.
We just forgot to run a command...
And look at the error: Rails is trying to tell us what's went wrong.

#### Migration
We need to "run a migration".

Running 'rails g scaffold [...]' creates all the folders and files, and it creates a set-up file in the db/migrate directory, but it does not actually create the table in the database.

We need to go to our Terminal and run the command:
```sh
$ rails db:migrate
```

Now our table is created! Check the db/schema.rb file to see all the tables in your database.


### Tour of the Views
You can refresh localhost:3000/pets and you should see a proper page now. Go ahead and add a few pets using the form fields in your browser. Play around and get a feel for the various pages.

Let's take a look at the generated code on each page:

	1. Index view
		* Table that houses all the instances of our Resource
		* Link to Show a single instance of the Resource
		* Link to Edit an instance of the Resource
		* Link to Deleting an instance of the Resource
		* Below the tab, a Link to entering a New instance of the Resource
	2. Show view
		* This page consists of paragraphs to display the attributes of a single instance of the Resource.
		* Plus a Link to Edit this instance of the Resource.
		* And a Link back to the Index
	3. New & Edit views
		* These are a little sparse. That's because the actual guts of the pages are located on the "form". The render tag is what displays the content on these views.
	4. Form view
		* This page is what's called a *partial* . A partial (denoted by an underscore) is reusable code that can be *render*ed in many locations which keeps our code DRY. This code drops a form into our views and gathers data from the user.


### Tour the controller and look at the methods for the CRUD actions

### Customizing the Index View
Each pet id exists in our table but doesn't show by default in the view. Let's add an id column to our index.

```html
<table>
  <thead>
    <tr>
      <th>ID</th>
      <th>Name</th>
      <th>Breed</th>
      <th>Age</th>
      <th colspan="3"></th>
    </tr>
  </thead>

  <tbody>
    <% @pets.each do |pet| %>
      <tr>
        <td><%= pet.id %></td>
        <td><%= pet.name %></td>
        <td><%= pet.breed %></td>
        <td><%= pet.age %></td>
        <td><%= link_to 'Show', pet %></td>
        <td><%= link_to 'Edit', edit_pet_path(pet) %></td>
        <td><%= link_to 'Destroy', pet, method: :delete, data: { confirm: 'Are you sure?' } %></td>
      </tr>
    <% end %>
  </tbody>
</table>
```


### Deleting a Record
Go ahead and delete the last pet record in your list. Then add a new pet.

Notice the ID skips to the next number incrementally. Once an ID is deleted the database does not reuse it.

