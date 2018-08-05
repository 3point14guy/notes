# Twitter Project
## Part One: Association Review & Validation

Useful Resources
[Font Awesome Gem](https://github.com/bokmann/font-awesome-rails)
[Rails Validation](http://guides.rubyonrails.org/active_record_validations.html)

---

1. Homework Review
2. Intro & Outline
3. Gems to Add
4. Devise Set-Up
* User model
* More User attributes
5. Tweet Scaffold
6. Association Review: Users & Tweets
7. Authenticate Tweeters
8. Validation
9. Using current_user 
10. Following Other Users

---

### Quick Notes

Newer Rails no longer carries the JQuery Dependency, for users running rails 5.1 or newer they will need to add the gem 'jquery-rails' into their gem file. They will also need to update the application.js file to add:

//= require jquery & //= require jquery_ujs

Without the additon of these files you will recieve errors in the console pertaining to jquery 



— Form Genration for rails 5.1 has changed from <%= form_with(model: @location, local: true) do |f| %> to <%= form_with(model: @location, local: true) do |**Form**| %>. To get f.text or f.field …etc to work properly you will have to change |form| to |f|



### Intro & Outline

You now have the coding skills (5k1lL2) to create your own version of one of the most popular sites on the Internet...TWITTER!

Twitter itself was originally built using Ruby on Rails. It may seem like a daunting task to create an app that mimics one of the top social media sites, but we'll take it step-by-step.

Today's Checklist (basically what it is above):
	1. Create the Project
	2. Add to your Gemfile (Devise, CarrierWave, Bootstrap)
	3. Add Bootstrap to CSS & JS
	4. Set Up Devise User & add User attributes
	5. Construct Tweet scaffold
	6. Set up Associations between User & Tweets
	7. Validations for Users & Tweets
	8. How do we go about "Following" another User?

Let's create a new project. Here's a few ideas for project names:
```sh
$ rails new twitter-talent-south

$ rails new tech-talent-twitter

$ rails new tweety_mctweetface
```


### Gems to Add
Let's add the Devise, CarrierWave, and Bootstrap gems to our Gemfile:

```ruby
# Gemfile
gem 'devise'
gem 'jquery-rails' **(required for users running rails 5.1 or newer)

gem 'carrierwave'  
gem 'paperclip'

gem 'bootstrap-sass'
gem 'font-awesome-rails'
# Remember Font Awesome 
# from our Bootstrap lesson?
# They have a Gem, too!
```
And of course: bundle install!

We'll continue with Devise set-up, and add CarrierWave functionality a little later on.

First, let's finish up Bootstrap set-up.

```css
// application.css
// becomes application.scss
// and we add:

@import "bootstrap-sprockets";
@import "bootstrap";
@import "font-awesome";
```

```javascript
// application.js
// we add to the requires (below jquery require):

//= require bootstrap-sprockets
```


### Devise Set-Up
Let's just blow through all the Terminal commands to set-up Devise and be done with!

```sh
$ rails g devise:install

$ rails g devise User

$ rails db:migrate

$ rails g devise:views
```

Let's let the User add a little more data about themselves.

```sh
$ rails g migration AddValuesToUsers name:string username:string bio:text location:string

$ rails db:migrate
```

"Username" will be the @-fronted handle. "Bio" and "location" will be optional fields the User can fill in once signed-in/registered (in a "Profile" page).

Remember: because there is no User controller, we need to use the application_controller.rb...

```ruby
# application_controller.rb
class ApplicationController < ActionController::Base
  # Prevent CSRF attacks by raising an exception.
  # For APIs, you may want to use :null_session instead.
  protect_from_forgery with: :exception

  before_action :configure_permitted_parameters, if: :devise_controller?

  protected

  def configure_permitted_parameters
    devise_parameter_sanitizer.permit(:sign_up, keys: [:username, :name, :bio, :location])
    devise_parameter_sanitizer.permit(:account_update, keys: [:username, :name, :bio, :location])
  end
end
```

And we need to add the fields to the registration views
(**app/views/devise/registrations/new.html.erb** + **.../edit.html.erb**).

```html
<!-- same for both files -->

<div class="form-group">
  <%= f.text_field :name, placeholder: "Your Name", class: "form-control" %>
</div>

<div class="form-group">
  <%= f.text_field :username, placeholder: "Choose a @username", class: "form-control" %>
</div>

<div class="form-group">
  <%= f.text_field :location, placeholder: "Location", class: "form-control" %>
</div>

<div class="form-group">
  <%= f.text_area :bio, placeholder: "Tell us about yourself...", class: "form-control" %>
</div>
```


### Tweet Scaffold
It's time to build the scaffold for your Tweet resource!

But hold your horses: we need to think about Association first! Remember that we want to Associate Users with Tweets, using a Foreign Key. This would be a One-to-Many relationship, as one User would have many Tweets. 

With that in mind, have at it:

```sh
$ rails g scaffold Tweet message:string user_id:integer
```

migration time!!


### Association Review: Users & Tweets
Let's add the association definitions to our models...

```ruby
# tweet.rb
class Tweet < ApplicationRecord
  belongs_to :user
end
```

```ruby
# user.rb
class User < ApplicationRecord
  # Include default devise modules. Others available are:
  # :confirmable, :lockable, :timeoutable and :omniauthable
  devise :database_authenticatable, :registerable,
         :recoverable, :rememberable, :trackable, :validatable

  has_many :tweets
end
```

### Authenticate Tweeters
Let's put the Tweet pages behind Devise's wall of protection. Add to your tweets_controller.rb:

```ruby
# tweets_controller.rb
class TweetsController < ApplicationController
  before_action :set_tweet, only: [:show, :edit, :update, :destroy]

  before_action :authenticate_user!
```

This will keep those who are not signed-in out of our app for the most part. And forcing a sign-in before tweeting will give us access to 'current_user'!



**Pick one of these as an Avatar Uploader**

(note Paperclip doesn't work well with PC)

#### CarrierWave

```ruby
gem 'carrierwave'
```

*bundle install*.

Next step is to generate an uploader:

```shell
rails generate uploader Avatar
```

This will create an uploader folder (in the app folder), with a file within called *avatar_uploader.rb*. Inside there, make sure that this line is not commented out:

```ruby
class AvatarUploader < CarrierWave::Uploader::Base
  storage :file
end
```

Now we need to add to our User table.

```sh
$ rails g migration AddAvatarToUsers avatar:string
$ rails db:migrate
```

We need to update the sanitizers in application_controller.rb

```ruby
  before_action :configure_permitted_parameters, if: :devise_controller?

  protected

  def configure_permitted_parameters
    devise_parameter_sanitizer.permit(:sign_up, keys: [:username, :name, :bio, :location, :avatar])

    devise_parameter_sanitizer.permit(:account_update, keys: [:username, :name, :bio, :location, :avatar])
  end
```

Then we need to "mount" the uploader on the User model:

```ruby
  # Include default devise modules. Others available are:
  # :confirmable, :lockable, :timeoutable and :omniauthable
  devise :database_authenticatable, :registerable,
         :recoverable, :rememberable, :trackable, :validatable

  has_many :tweets

  mount_uploader :avatar, AvatarUploader
```

Finally, we need to add a configuration in **config/application.rb**:

```ruby
config.autoload_paths += %W(#{config.root}/app/uploaders)
```

That takes care of the back end, now we can set-up the front end. First, where to upload:

```ruby
<!--add to both devise/registrations/new.html.erb
  and /edit.html.erb-->

<div class="form-group">
  <strong>Upload a Profile Pic:</strong>
  <%= f.file_field :avatar %>
</div>
```

Then we can display the image like this (depending on the location):

```ruby
<%= image_tag current_user.avatar.url %>
<!-- or -->
<%= image_tag post.user.avatar.url %>
```

#### Paperclip

**Installing Paperclip OSX**

```shell
brew install imagemagick 
```

**Gemfile**

```ruby
gem 'paperclip'
```

*bundle install*

Then we need to create a migration file to add avatar to users.

```shell
$ rails generate AddAvatarToUsers
$ rails db:migrate
```

Next we need to modify our **User.rb** file to so we can properly use paperclip, were going to add the following code to the top of User.rb in our models

```ruby
class User < ActiveRecord::Base
  has_attached_file :avatar, styles: { medium: "300x300>", thumb: "100x100>" }, default_url: "/images/:style/missing.png"
  validates_attachment_content_type :avatar, content_type: /\Aimage\/.*\z/
```

We need to update our sanitizers so that files uploaded will save. We will go into our application_controller and make the following changes to the sanitizers

```ruby
  before_action :configure_permitted_parameters, if: :devise_controller?

  protected

  def configure_permitted_parameters
    devise_parameter_sanitizer.permit(:sign_up, keys: [:username, :name, :bio, :location, :avatar])

    devise_parameter_sanitizer.permit(:account_update, keys: [:username, :name, :bio, :location, :avatar])
  end
```

That takes care of the back end, now we can set-up the front end. First, where to upload:

```ruby
<!--add to both devise/registrations/new.html.erb
  and /edit.html.erb-->

<div class="form-group">
  <strong>Upload a Profile Pic:</strong><br>
  <%= f.file_field :avatar %>
</div>
```

Then we can display the image like this (depending on the location):

```ruby
<%= image_tag current_user.avatar.url %>
<!-- or -->
<%= image_tag post.user.avatar.url %>
```

#### Time to Test

Let's set the root page to be the Tweets index (you should know how to do this by now). You also might have some migrations pending (you should know how to handle those as well). And then, it's high time we fired up our server and made sure at least some of this stuff we set up is working!

What are we looking for? 
	1. Login page renders
	2. Bootstrap styling

It'd be great if we could sign out to test multiple users, so let's add (with a few tweeks) that Navbar from the Blog App:



**To get this navbar to work without errors you need to first install carrierwave or you will get an error for avatar not defined. You can also remove the avatar tags and it will still work properly **

```html
<!-- app/views/layouts/application.html.erb -->
<body>
	<nav class="navbar navbar-expand-lg navbar-light bg-light justify-content-between">
		<%= link_to "Home", root_path, class: "navbar-link" %><span class="sr-only">(current)</span>
		<button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
			<span class="navbar-toggler-icon"></span>
		</button>

		<div class="collapse navbar-collapse" id="navbarSupportedContent">
			<ul class="navbar-nav mr-auto">
				<li class="nav-item active">
					<%= link_to "Tweet", new_tweet_path, class: "nav-link" %>
				</li>
			</ul>
			<ul class="navbar-nav float-right">
				<% if user_signed_in? %>
					<li class="nav-item dropdown"><span class="caret"></span>
						<a class="nav-link dropdown-toggle" id="navbarDropdown" role="button" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
							<% if current_user.avatar.blank? == false %>
								<%= image_tag current_user.avatar.url, size:25, class: "user-pic-nav" %>
							<% end %>
							@<%= current_user.username %>
						</a>
						<div class="dropdown-menu" aria-labelledby="navbarDropdown">
							<%= link_to "Edit Profile", edit_user_registration_path, class: "dropdown-item" %>
							<%= link_to "Sign Out", destroy_user_session_path, method: :delete, class: "dropdown-item" %>
						</div>
					</li>
				<% end %>
			</ul>
		</div>
	</nav>
         
	<div class="container">
    <%= yield %>
	</div>
</body>
```


### Validation
In Rails, "Validation" refers to a sort of gate-keeping on the data this is input from the User. Using Validation we can make sure - to an extent - that the data that is being input matches what we expect (e.g., a string attribute has a certain length).

Devise has built-in *presence* validations on email and password, so let's add two for username - we want to make sure it's given & unique:

```ruby
# user.rb
  validates :username, presence: true, uniqueness: true
```
We also need validation on the Tweets - not just presence, but also: what makes a Tweet a Tweet? Its length!

```ruby
# tweet.rb
  validates :message, presence: true
  validates :message, length: {maximum: 250, too_long: " - If you need more than 250 characters, create a blog."}
```

Test it out.
We set up that "username" must be present and unique, Devise took care of the other two validations.

And then there's the validation errors we set for the Tweets. What if we wanted to customize the error messages?

#### YAML
**Y**et **A**nother **M**arkup **L**anguage
In the config folder, down into the locales folder,
you'll find a file called "en.yml".

This file's intended use is for translating your app into other languages. This file acts as a data store for translations for Active Record's error_messages_for Helper and we are going to co-opt this feature to put in some more custom error messages (in English.)

Some messages, like for length, will have to remain in the model.

**config/locales/en.yml**
```yaml
en:
  hello: "Hello world"
  activerecord:
    attributes:
      tweet:
        message: "A Tweet"
      user:
        username: "A username"
    errors:
      models:
        tweet:
          attributes:
            message:
              blank: " is missing. Cat got your tongue?"
              too_long: " is only 140 characters. Everybody knows that."
        user:
          attributes:
            username:
              blank: " is missing. You need to pick one. And pick one wisely."
              taken: " must be unique. And someone already got to that one. Should've signed up sooner."
```

**BE CAREFUL!** *yaml* files do not accept tabs, just spaces!

Test out the validations again!


### Using current_user
We can use current_user to help us properly create a new tweet. *current_user.id* is the ID # we'll looking to populate our FK (tweet.user_id) with!

```html
  <div class="form-group">
    <%= f.text_field :message, placeholder: "What's happening?", class: "form-control" %>
  </div>
  <div class="field">
    <%= f.hidden_field :user_id, value: current_user.id %>
  </div>
  <div class="form-group">
    <%= f.submit "Tweet It!", class: "btn btn-primary" %>
  </div>
```
We use "hidden_field" to keep our associating process hidden from the user.

What we should see (or rather, shouldn't see) at **localhost:3000/tweets/new**:
An entry for user_id will be passed, just not shown. Use Chrome's "Inspect" to see that the code for that input field is indeed there!

It's time to test association. Let's modify the Show page to display the username behind this new tweet!

```html
<!-- tweets/show.html.erb -->
<p>
  <strong>Message:</strong>
</p>

<p>
  <strong>User:</strong>
  <%= @tweet.user.username %>
</p>

<%= link_to 'Edit', edit_tweet_path(@tweet) %> |
<%= link_to 'Back', tweets_path %>
```

This page could use a redesign... but that sounds like great material for homework!


### Following Other Users
One of the major functions of Twitter is that you "follow" other users and their tweets show up on your "feed".

Let's first create a controller that will contain views that house your feed and allow you "follow" another User.

Let's call this controller the "Epicenter" - both because it's going to be the focal point of our site, and because aren't you tired of calling this sort of thing the "Welcome" controller?

```sh
$ rails g controller Epicenter feed show_user now_following unfollow
```

	* The "feed" page will be similar to those "welcome/index" pages we've done in past projects - we'll make it the root page in our Routes file.

	* "show_user" will do just what it says: show individual user's profile and tweets (since we used Devise to create the User, we were given no "users/show" page).

	* "now_following" will be an action taking the selected User and adding their ID to the  current_user's "following" attribute (view page optional!).

	* "unfollow" will be similar to the above action, but removing a User from the current_user's "following" attribute (view page optional!).

Speaking of, we need to add this "following" attribute to the User.

```sh
$ rails g migration AddFollowingToUsers following:text

$ rails db:migrate
```

Why did I set the data-type as text? You can trick a text attribute into behaving like an array in via the model:

```ruby
# placed around line 7:
  serialize :following, Array
```

The "following" attribute is now an Array, and we can push other Users' IDs into it. Also remember! We need to add this new attribute to the sanitizers in our **application_controller.rb**.

```ruby
# application_controller.rb
  def configure_permitted_parameters
    devise_parameter_sanitizer.permit(:sign_up, keys: [:username, :name, :bio, :location, :avatar, :following])

    devise_parameter_sanitizer.permit(:account_update, keys: [:username, :name, :bio, :location, :avatar, :following])
  end
```

We just got a whole bunch of new views & actions, so let's head over to the Routes file...

```ruby
# routes.rb
Rails.application.routes.draw do
	root 'epicenter#feed'

	get 'show_user' => 'epicenter#show_user'

	get 'now_following' => 'epicenter#now_following'

	get 'unfollow' => 'epicenter#unfollow'

	resources :tweets
	devise_for :users
end
```

The 'feed' page is set as the Root, and we shorten some URLs.

The 'feed' page will be fairly simple: a greeting to the current_user, and then a loop through that tweets associated with the current_user's 'following' array.

```html
<!-- epicenter/feed.html.erb -->
<h1>@<%= current_user.username %>'s Twitter Feed</h1>
<p>Here is what your "friends" are saying:</p>


<div>
    <% @following_tweets.each do |tweet| %>
        <div>
	    <p>@<%= tweet.user.username %></p>
	    <p><%= tweet.message.html_safe %></p>
	</div>
    <% end %>
</div>
```

So this @following_tweets variable...it's an array...but where'd that come from? We're gonna make it in the Controller!

```ruby
# epicenter_controller.rb
	def feed
	    @following_tweets = []

	    Tweet.all.each do |tweet|
	      if current_user.following.include?(tweet.user_id) 
	        || current_user.id == tweet.user_id
	        @following_tweets.push(tweet)
	      end
	    end
	end
```

We include tweets from people we follow, and our own tweets, in our "following_tweets".

The Show User page shows the profile and tweets of a single User. We want to also include a way for the current_user to follow them. Since we're not taking any input from the user, we can just use a link (as opposed to a form)!

```html
<!-- show_user.html.erb -->
<h1>@<%= @user.username %>'s Profile & Tweets</h1>

<p><%= @user.name %></p>
<p><%= @user.location %></p>
<p><%= @user.bio %></p>

<p>
    <% if current_user.following.include?(@user.id) %>
        <%= link_to "Unfollow", unfollow_path(id: @user.id), class: "btn btn-danger" %>
    <% else %>
        <% if current_user.id != @user.id %>
	    <%= link_to "Follow", now_following_path(id: @user.id), class: "btn btn-primary" %>
	<% end %>
    <% end %>
</p>

<% @user.tweets.each do |tweet| %>
    <div>
        <p>@<%= @user.username %></p>
	<p><%= tweet.message %></p>
    </div>
<% end %>
```

We need to pass user_id, so we can add that to the current_user's Following array.

```ruby
# epicenter_controller.rb
	def show_user
	  @user = User.find(params[:id])
	end
```

Yeah...that's all we need in there! Thing is, to reach this page, we need to follow this link:

```html
<!-- tweets/show.html.erb  -->
<p>
	<strong>User:</strong>
	  <%= link_to @tweet.user.username, show_user_path(id: @tweet.user.id) %>
</p>
```

```html
<!-- tweets/index.html.erb -->
<td><%= link_to tweet.user.username, show_user_path(id: tweet.user.id) %></td>
```

In the controller, we'll add to the current_user's Following array, then, instead of continuing to the view page, let's go back to the page we were on (**redirect**).

```ruby
# epicenter_controller.rb
def now_following
  # We are adding the user.id of the user you want to 
  # follow to your following array.
  # and we want to make sure it's saved in there as an integer
  current_user.following.push(params[:id].to_i)
  current_user.save

  redirect_to show_user_path(id: params[:id])
end
```

You could delete the associated view if you wanted (now_following.html.erb).

We could test out this functionality now.

We need to have a filter that will prevent the "Follow" button from displaying if we're looking at our own profile page.

It's a pretty easy fix: Just add an If Statement!

```html
<!-- epicenter/show_user.html.erb -->
<% if current_user != @user.id %>
    <%= link_to "Follow", now_following_path(id: @user.id), class: "btn btn-primary" %>
<% end %>
```

The "unfollow" action will be nearly identical to the "now_following" action: deleting from the "following" attribute (instead of pushing to).

```ruby
# epicenter_controller.rb
def unfollow
  current_user.following.delete(params[:id].to_i)
  current_user.save

  redirect_to show_user_path(id: params[:id])  	
end
```

Once again, you could delete the associated view if you wanted (**unfollow.html.erb**).

