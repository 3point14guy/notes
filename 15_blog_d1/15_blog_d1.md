# Blog Project
## Part One: Query Strings, Scaffolding & Association

Useful Resources
[Active Record Associations](http://guides.rubyonrails.org/association_basics.html)

---

1. Homework Review
2. Blog Intro
3. What Makes Up a Scaffold
4. Query Strings
 * Links
 * Forms
 * Post routes
5. Scaffolding Review
 * Scaffold (Blog)Post Resource
6. Bootstrap Gem
7. Scaffold Comment Resource
8. Rails Generate Migration
9. Resource Association
 * One to One
 * One to Many
 * Many to Many
10. Blog/Comment Association

---

### Quick Notes

Newer Rails no longer carries the JQuery Dependency, for users running rails 5.1 or newer they will need to add the gem 'jquery-rails' into their gem file. They will also need to update the application.js file to add:

//= require jquery & //= require jquery_ujs

Without the additon of these files you will recieve errors in the console pertaining to jquery 



— Form Genration for rails 5.1 has changed from <%= form_with(model: @location, local: true) do |f| %> to <%= form_with(model: @location, local: true) do |**Form**| %>. To get f.text or f.field …etc to work properly you will have to change |form| to |f|

### Blog Intro

Why do a blog app?
Because once you know how to create one, you'll realize how so many other apps are basically just blog apps. Sites like Wikipedia, Craiglist, etc. - we don't think of them as "blogs", but they have the same format!

It's time to start a new project/app. Let's start with a name. Here's a suggestion:

```sh
$ rails new tech_talent_blog
...
$ cd tech_talent_blog
```


### What Makes Up a Scaffold
When we run `rails g scaffold`, we tend to take for granted what it's giving us. The scaffold generate command is essentially a combination of two different generate commands:

```sh
$ rails g model Resource attribute:datatype attribute:datatype

$ rails g controller Resource index new show edit create update destroy
```
(though the latter would give us actions & views for "create", "update" and "destroy", while scaffold only gives actions)

In addition, scaffold sets up connections between these pages, which we would have to create manually otherwise.
Let's try making those connections on our own...

We at least need a controller and views to try this out, so let's build a practice controller:
```sh
$ rails g controller Practice index about
```

We're going to use these two pages to pass information back and forth, using *Query Strings*.


### Query Strings
***slide 6***  **[SHOW](http://techtalentsouth.slides.com/techtalentsouth/rails-blog-one-ci?token=Haq_LgJA#/0/6)**

You've seen query strings before, whether you realize it or not. A question mark in the URL denotes the beginning of a query string.

A query string holds additional information, stored in variables, which the URL may not address. In this example (image in the slides), the URL handles the location, while the Query String handles all other data (dates, # of guests, etc.). The ampersands in the Query String separate different variable/value pairs.

So how can we attach query strings to our URLs? It's all in the links!

Let's set our routes to something more manageable for our links:
```ruby
get 'index' => 'practice#index'

get 'about' => 'practice#about'
```

Let's set up several links to test passing different values through query strings.

```html
<!-- index.html.erb -->

<h1>Choose a Color!</h1>

<p>
  <%= link_to "Red", about_path(color: "red") %>
</p>

<p>
  <%= link_to "Blue", about_path(color: "blue") %>
</p>

<p>
  <%= link_to "Green", about_path(color: "green") %>
</p>

<p>
  <%= link_to "Yellow", about_path(color: "yellow") %>
</p>
```

Whatever link we chose on the view page, that value will come to the controller actions as "params[:color]", and we then want to store it in an instance variable for use on the view.

```ruby
class PracticeController < ApplicationController

  def index
    # we're not using any variables in the view
    # so nothing to setup in here...yet...
  end

  def about
    # here we'll pull from the query string
    # and save that value in an instance variable
    @color = params[:color]
  end

end
```

How can we use our color of choice? Let's use it for CSS! We'll set a particular background color to a <div> with a customized message.

```html
<!-- about.html.erb -->

<!-- Remember, internal stylesheet is not the best,
    but we have no way to get a Ruby instance
    variable into a CSS file, so we don't have
    much of an alternative here -->
<style>

  #color_me_in {
    background-color: <%= @color %>;
    color: white;
    width: 250px;
    height: 250px;
  }

</style>

<div id="color_me_in">
  <p>
    <%= @color.upcase %> BOX ACTIVATED!
  </p>
</div>
```

Try out those links now!

Notice something: That a link, with or without passing value(s) to the query string, leads to a get route.

But what if we move from page to page via a form? Let's say...to get back to the Index page from the About page, we have to fill out a form (we'll leave it at just one input).

```html
<!-- about.html.erb -->

<!-- here's where all the stuff
  we did previously is (CSS & div w/ id -->

<%= form_tag index_path do %>
  <p><%= text_field_tag :name, nil, placeholder: "What's your name?" %></p>
  <p><%= submit_tag :submit %></p>
<% end %>
```

Now, we're going to need a *post* route for Index, but we can keep the get route, for when we come to the page not via the form.

```ruby
get 'index' => 'practice#index'

post 'index' => 'practice#index'

get 'about' => 'practice#about'
```

We can create a message in our Index view using the name provided in the form in the About view. But if we don't arrive via that form (say, if we just type in localhost:3000/index into the URL bar) we don't want to print out the incomplete message, so we'll print the message conditionally:

```html
<h1>Choose a Color!</h1>

<% if !@name.nil? %> <!-- if @name is not nil -->
  <h2>Welcome Back, <%= @name %></h2>
<% end %>

<!-- all those links are down here -->
```

The last step: we need to set @name to params[:name], for when we do arrive via the About page form.

Rails gives us access to the data passed in query strings and the data posted in forms by stroing it in a params "hash". You can access the values in this params "hash" using either symbols or keys.  These values are always saved as strings.

```ruby
class PracticeController < ApplicationController
  def index
    @name = params[:name]
    # @name will be nil if we come to this
    # page via the 'get' route
  end

  def about
    @color = params[:color]
  end
end
```

Try that out!


### Scaffolding Review
Let's put some thought into what we're building. Let's think backwards about it.
	* What we want a blog post to look like
	* Then, what are the attributes of the post?
We may run into attributes we wish we had at the beginning, but Rails will help us add them later!

It's time scaffold!
```sh
$ rails g scaffold Post title:string author:string blog_entry:text
```
Accuracy is important! Check your spelling: Rails does not recognize the "sting" data type. You could fall back on `rails d scaffold` if you make a mistake, but it may save time just to be diligent the first time.

Remember: what do we need to do after we scaffold?
```sh
$ rails db:migrate
```

Now "Post" is a real database table!

NOTE! Once your do migrate, `rails d scaffold` will only delete the files the generator created - it will not delete the table! (And you would get an error if you tried to run that `rails g scaffold` and `rails db:migrate` again.)

Let's see what we got from scaffolding!

All these files:
	Assets:
	* posts.coffee
	* posts.scss
	Controller:
	* posts_controller.rb
	Helper:
	* posts_helper.rb
	Model:
	* post.rb
	Views:
	* _form.html.erb
	* index.html.erb
	* new.html.erb
	* edit.html.erb
	* show.html.erb

```ruby

In **routes.rb**:
Rails.application.routes.draw do
  resources :posts
  # practice controller routes down here
end
```

#### Ready-to-Go Controller

*Instructors: walk the students through the different controller actions*
```ruby
class PostsController < ApplicationController

	# This is a filter.  Filters are applied "before", "after", or "around" a controller action. 
	# A common before action is one set for login. If not loged in, don't honor any requests.
	# The before action here is for all methods that act on a single resource and sets @post to the values in that row.
	
	
  before_action :set_post, only: [:show, :edit, :update, :destroy]

  # GET /posts
  # GET /posts.json
  def index
    @posts = Post.all
  end

  # GET /posts/1
  # GET /posts/1.json
  def show
  end

	# This just renders the form for creating a new Post object. 
	# Post.new here just creates an new instance of Post with no attributes yet assigned.
  # GET /posts/new
  def new
    @post = Post.new
  end

	# Again, this get request is just rendering a form.
  # GET /posts/1/edit
  def edit
  end

  # POST /posts
  # Here we are passing in the attributes that will actually make the Post object/instance.
  def create
    @post = Post.new(post_params)

    respond_to do |format|
    # "redirect_to" makes a new HTTP request and retrieves updated data.  
    # "render" just displays the cached page. Which is great for the user, 
    # because their inputs are still there and they only need fix the failed input.
      if @post.save
        format.html { redirect_to @post, notice: 'Post was successfully created.' }
        format.json { render :show, status: :created, location: @post }
      else
        format.html { render :new }
        format.json { render json: @post.errors, status: :unprocessable_entity }
      end
    end
  end

  # PATCH/PUT /posts/1
  def update
    respond_to do |format|
      if @post.update(post_params)
        format.html { redirect_to @post, notice: 'Post was successfully updated.' }
        format.json { render :show, status: :ok, location: @post }
      else
        format.html { render :edit }
        format.json { render json: @post.errors, status: :unprocessable_entity }
      end
    end
  end

  # DELETE /posts/1
  def destroy
    @post.destroy
    respond_to do |format|
      format.html { redirect_to posts_url, notice: 'Post was successfully destroyed.' }
      format.json { head :no_content }
    end
  end
	# private methods are not accessible from our routes...all these other methods have have routes that bring them to the actions in this controller.	
  private
    def set_post
      @post = Post.find(params[:id])
    end

    # Never trust parameters from the scary internet, only allow the white list through.
    def post_params
      params.require(:post).permit(:author, :title, :blog_entry)
    end
end
```

#### Active Record Review

Notice all the ActiveRecord calls in your controller:

```ruby
@posts = Post.all
# A collection of all the instances of BlogPost in your database.

@post = Post.new
# Creating a new instance of BlogPost (remember initialize from Ruby classes?)

@post.save
@post.update
@post.delete
# "save", "update" and "delete" are all methods from the Post class!

@post = Post.find(params[:id])
# We're looking for just one instance of the Posts (using the ID #)
```

It's hard to conceptualize the look of our app without any content. So let's make some! Create at least 4 blog posts!

*Instructors: give students about 5 minutes to make some blog posts. Encourage them to use Ipsum to fill in the Blog Entry area.*

### Scaffold Comment Resource
Unless it's been disabled because users are just being abusive or obnoxious, Commenting is usually a common aspect of Blog (or blog-like) web apps.

Let's create a new scaffold for Comments...it'll be very similar to the Posts scaffold:

```sh
$ rails g scaffold Comment author:string comment_entry:text

$ rails db:migrate
```

So we can now create Comments...but they're just out there in the ether, unattached to any blog post. Let's fix that with some migration & association!


### Rails Generate Migration

The `rails g migration` command simply create a migration file (found in the **db/migrate** folder). Often it creates an empty file that you fill in with code. But one use for through some clever naming, we can get it to fill in the code we need:

```sh
$ rails g migration AddValueToComments post_id:integer

$ rails db:migrate
```

We have the generator command, plus a title:

**AddValueToComments**

Notice the camel-casing and the wording.

	* Add - we're looking to add to a table
	* Value - this is a placeholder, we could've also written "PostId", or if we were adding multiple attributes: "Values", or even "Thing"/"Things" would work
	* To - to
	* Comments - the name of the table we want to add to

And after that, we have the name of the attribute and its data-type that we want to add to the table.

`rails g migration` will also be used to:
	* Change a data-type
	* Change attribute name
	* Remove attribute
	* Delete a table

Returning to the project at hand...
In the Comment Controller, we need to add this new attribute to the permitted parameters, found at the very bottom of the file:

```ruby
# Never trust parameters from the scary internet, 
# only allow the white list through.
def comment_params
    params.require(:comment).permit(:author, 
    :comment_entry, :post_id)
end
```


### Resource Association
Adding "post_id" as an attribute to Comments is the first step in setting up an **Association** between the two resources!

*Associations* allow us to retrieve data from one resource, through another. Let's take a quick look at the different types of Associations...

#### One-to-One Association
The simplest form of *Association*, where one instance of a Resource "has one" of the other, and that instance of the other resource "belongs to" one of the first.
Like a parent-child relationship.

```sh
$ rails g scaffold User name:string

$ rails g scaffold Vehicle make:string model:string user_id:integer
```

The child carries the ID of the parent. The ID of a table is also known as it's **Primary Key** (PK). The ID of a table as an attribute of another table (like "post_id" in our app), is called the **Foreign Key** (FK).

```ruby
class User < ApplicationRecord
  has_one :vehicle
end
```

```ruby
class Vehicle < ApplicationRecord
  belongs_to :user
end
```

#### One-to-Many Association
Where one instance of a resource "has many" of the other, and those instances of the other resource only "belong(s) to" that instance of the first. It's still a parent-child relationship, but with several children.

```sh
$ rails g scaffold State name:string

$ rails g scaffold City name:string state_id:integer
```

Once again, we see the child with a Foreign Key attribute (the Primary Key of the parent).

And then in the models:

```ruby
class State < ApplicationRecord
  has_many :cities
end
```

```ruby
class City < ApplicationRecord
  belongs_to :state
end
```

#### Many-to-Many Association
This one takes a little more work. Where instances of a resource can "have many" instances of the other resource, and many instances of that resource can "have many" right back. But this type of Association will require a third database: the Join Table!

```sh
$ rails g scaffold Course subject:string

$ rails g scaffold Student name:string gpa:float

$ rails g scaffold StudentCourses student_id:integer course_id:integer
```

In this association, the main tables involved are both parent and child of each other, so neither takes the FK of the other. The Join Table takes both Foreign Keys.

In the models:
```ruby
class Course < ApplicationRecord
  has_many :student_courses
  has_many :students, :through => :student_courses
end
```

```ruby
class Student < ApplicationRecord
  has_many :student_courses
  has_many :courses, :through => :student_courses
end

​```ruby
class StudentCourse < ApplicationRecord
  belongs_to :course
  belongs_to :student
end
```
Often, a Join Table is named by combining the names of the two table it is joining. It does not *have* to be named that, though. For example: a join table between Patients and Doctors could be called "Appointments" (and it could take more attributes that just the FKs).


### Blog/Comment Association
So what type of association do we need for our Posts-Comments relationship?

It is a One-to-Many Association (A Post has many Comments, a Comment belongs to a Post)!

```ruby
class Post < ApplicationRecord
  has_many :comments
end
```

```ruby
class Comment < ApplicationRecord
	belongs_to :post
end
```

This association allows us to view data from the Comment table through the Post table (and vice versa).

The last piece of the puzzle is assigning the post_id...
It would be best if we could do it without the author's knowledge. Let's add to my Comments' _form a hidden field that auto-populates the value for post_id.



**Form generation for rails 5.1 is slightly different, to use f.text users will need to change |form| to |f|

```html
<div class="field">
  <%= f.text_field :author, placeholder: "Written By..." %>
</div>

<div class="field">
  <%= f.text_area :comment_entry, placeholder: "Tell Me Something..."  %>
</div>

<!-- THIS ONE -->
<div class="field">
  <%= f.hidden_field :post_id, value: @post.id %>
</div>
<!-- THIS ONE -->

<div class="actions">
  <%= f.submit %>
</div>
```

By default, Rails builds it's forms with form labels.  If we keep these, then we also need to pass form field ids like seen here:

```
  <div class="field">
    <%= form.label :author %>
    <br>
    <%= form.text_field :author, id: :post_author %>
  </div>
```

We want to create new comments on the Post show page. We can render the "_form" partial from the Comments views, in the post_controller, we initiate the variable @comment, and in the comments_controller we redirect the page after comment creation:

```html
<!-- posts/show.html.erb -->
<p>
  <%= link_to 'Edit', edit_post_path(@post) %>
</p>

<div>
	<!-- show a template from another controller's views  
	populate it with a new instance of comment (@comment) -->
	<%= render 'comments/form', comment: @comment %>
</div>

<p>
  <%= link_to 'Back', posts_path %>
</p>
```

```ruby
# posts_controller.rb

  def show
    @comment = Comment.new
  end
```

```ruby
# comments_controller.rb
  @comment = Comment.new(comment_params)

  respond_to do |format|
    if @comment.save
    	# IN HERE!
    	# Changed the redirect_to to go back
    	# to the Post Show page
      format.html { redirect_to post_path(id: @comment.post_id), 
      notice: 'Comment was successfully created.' }
    # more below, unchanged
```

You can go head and make all the comments you want, but none are going to show...until we do this: On the posts/show.html.erb page, between the comment creation div and the link back to the index:

```html
<div>
	<%= render 'comments/form', comment: @comment %>

  <% @post.comments.each do |comment| %>
    <p>
    	<%= comment.author %> said...<br />
    	<%= comment.comment_entry %>
		</p>
  <% end %>
</div>
```

Try it all out!


### Bootstrap Gem
Now that we have content, we can tell what a mess it's going to be with the design Rails gave us.

Let's use Bootstrap to make this app at least more readable. But this time, let's do it the Rails ways: let's use Gems!

According to the [Bootstrap Sass Gem](https://github.com/twbs/bootstrap-sass) documentation, we need their gem along with the 'sass-rails' gem -- which comes standard in a Rails app Gemfile:

```ruby
#already installed:
gem 'sass-rails', '~> 5.0'

# original instructions:
# gem 'bootstrap-sass'

#fix as of 7/31/18
# gem 'bootstrap', '~> 4.1.3'
gem 'jquery-rails' **(required for users using rails 5.1 or newer)
```

Then run in Terminal: 
```sh
$ bundle install
```

A couple more steps are required to activate the Bootstrap libraries.

In assets/stylsheets/application.css:

```css
// below all the comments at the top you ad:
# original instruction:
# @import "bootstrap-sprockets";
# @import "bootstrap";

# fix as of 7/31/18, use only:
@ import "bootstrap";

# and remove
*= require_tree.
*= require_self
```
You also need to change the extension of the file. In Sublime, right-click (or ctrl+click) and choose "Rename" from the menu. It will open up a bar at the bottom of the window. Leave the file name as "application", but add an "s" before "css".

Your file should now be named **application.scss**. This will make it recognize SASS ([Stylistically Awesome Style Sheets](http://sass-lang.com/)) notation.

Then, in your 'application' javascript file add:
```javascript
//= require jquery  **(must be added for rails 5.1)
//= require jquery_ujs **(must be added for rails 5.1)
//= require bootstrap-sprockets
//= require turbolinks
//= require_tree .
```
It's the third one down that we added. Make sure that Bootstrap line comes after the jQuery lines, as Bootstrap is jQuery-dependent.

And now we have Bootstrap!
Try it out: Go add the container <div> around the <%= yield %> in **application.html.erb**.

Scaffold Re-Design will be homework! Have fun with that!
