# Blog Project
## Part Two: Authentication

Useful Resources
[Active Record Associations](http://guides.rubyonrails.org/association_basics.html)
[Devise Gem](https://github.com/plataformatec/devise)
[Gravatar](http://en.gravatar.com/)

---

1. Homework Review
2. Bootstrap Form Classes
3. Authentication
4. Devise Gem
 * User Resource
 * Sign In/Up/Out
5. User Replaces Author
6. User Association
7. Database Reset
8. Add Attribute to Users
9. Devise Views
10. User Images: Choices
11. Gravatar
12. Image Uploaders
    * Paperclip
    * CarrierWave

---

### Bootstrap Form Classes
Hopefully you did your homework and re-designed your index and show pages (using the grid and panels or wells, for example). What you may not have touched is your forms.
But Bootstrap has CSS classes for those, too!

#This does not work anymore!!!!!
There are a pair of classes to be used in forms:
*form-group* and *form-control*

```html
<!-- in comments/_form.html.erb -->
<div class="form-group">
  <%= f.text_field :author, placeholder: "Written By...", class: "form-control" %>
</div>

<div class="form-group">
  <%= f.text_area :comment_entry, placeholder: "Tell Me Something...", class: "form-control" %>
</div>

<div class="form-group">
  <%= f.hidden_field :post_id, value: @post.id %>
</div>

<div class="form-group">
  <%= f.submit "Submit", class: "btn btn-primary" %>
</div>
```

```html
<!-- in posts/_form.html.erb -->
<div class="form-group">
  <%= f.text_field :title, placeholder: "Blog Entry Title", class: "form-control" %>
</div>

<div class="form-group">
  <%= f.text_field :author, placeholder: "Your Name Here", class: "form-control" %>
</div>

<div class="form-group">
  <%= f.text_area :blog_entry, placeholder: "Tell Us Something...", class: "form-control" %>
</div>

<div class="actions">
  <%= f.submit "Submit", class: "btn btn-primary" %>
</div>
```
#Do this instead!

```gem "bootstrap_form", ">= 4.0.0.alpha1"```

```bundle install```

``` *= require rails_bootstrap_forms```

In form views, change ```form_with``` to ```bootstrap_form_with```

You can also add ```class: "btn btn-success"``` to ```<%= form.submit %>``` so that the new line of code looks like:
```<%= form.submit class: "btn btn-success"%>```


### Authentication
Authentication in web apps refers to the function of letting only certain users into certain parts of your website. The various components of authentication include:
	* Registration (sign-up)
	* Log In
	* Log Out
	* Change Password 
	* Forgot Password

We could set this up all on our own, but that would be a whole lot of work! Why not look to see if another enterprising Ruby coder has created authentication code for Rails.
Hey...there's a gem called "Devise" takes care of all of this! We only have to write a minimal amount of code!


### Devise Gem
We've worked with a Gem before - Bootstrap! - now we can another to our wheelhouse:

```ruby
# In your Gemfile:

gem 'devise'

# doesn't particularly matter where
# in your Gemfile - just not inside
# of a "group" block)
```

And then run "bundle install", of course!

But we're not done just yet...
Some gems are ready to go after a bundle install. Others need you to do a little more...

In the Terminal:
```sh
$ rails g devise:install
```

Next, we need to tell Devise where, in our database, to store all this fun authentication stuff: email, user_name, password.

```sh
$ rails g devise User
```

This Generate command is most similar to `rails g model`, but we are using Devise to create a model with particular attributes.

Don't forget to migrate!

You can now go in your app's code and find a model (**user.rb**), that will look like this:
```ruby
class User < ApplicationRecord
  # Include default devise modules. Others available are:
  # :confirmable, :lockable, :timeoutable and :omniauthable
  devise :database_authenticatable, :registerable,
         :recoverable, :rememberable, :trackable, :validatable
end
```

"User" is basic resource name used with Devise. You could use any other name (perhaps "Blogger" in this case), however: there are other gems that work in tandem with Devise, and they require the term "User" to be used.

Our first step in truly requiring authentication to use our app is to put a filter into our application_controller.rb:

```ruby
class ApplicationController < ActionController::Base
  protect_from_forgery with: :exception

  # this line below!
  before_action :authenticate_user!
end
```

Now if anyone wants to view any page, they'll need to sign up, and sign in, first! If you try to access your root page now, you'll be met with a sign-in form. And you can follow the "Sign up" link to a page where you can create a new account, and then get access to our app.

One thing isn't given, though: a way to sign out. You want to have more than User when you test - as you want to create an environment similar to what it will be like in production (where, presumably and hopefully, there will be more than one user).

Let's create a button that will allow us to sign out of the current User session. We'll place it within the container div on the **application.html.erb** page:

```html
<div class="container">
  <div class="pull-right">
    <% if user_signed_in? %>
      <%= link_to "Sign Out", destroy_user_session_path, method: :delete, class: "btn btn-danger" %>
    <% end %>
  </div>
...
```

We put the code within a <div> with a "pull-right" Bootstrap class, which just floats the div to the right of the container <div>


### User Replaces Author
When creating a new post or comment, why have the User continuously enter their name? Through Association we can assign ownership of a post or comment to the User who is currently signed in. Devise has even given us a special variable for that:

`<%= current_user %>`
This variable is available on every page of the app, and no @-symbol is required!

We can use this to welcome users to our site!
```html
Welcome <%= current_user.email %>!
```


### User Association

We will need to add a column onto the Posts and Comments tables, and set up Association keywords in the models.

```sh
$ rails g migration AddUserIdToPosts user_id:integer

$ rails g migration AddUserIdToComments user_id:integer
```
Remember to migrate!

Now, put the association keywords in the models:

```ruby
# post.rb
class Post < ApplicationRecord
	has_many :comments
	belongs_to :user
end
```

```ruby
# comment.rb
class Comment < ApplicationRecord
	belongs_to :post
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

  has_many :posts
  has_many :comments
end
```

We next need to add "user_id" to our permitted params in the Posts and Comments controllers

```ruby
# posts_controller.rb
    def post_params
      params.require(:post).permit(:title, :author, :blog_entry, :user_id)
    end
```

```ruby
# comments_controller.rb
    def comment_params
      params.require(:comment).permit(:author, :comment_entry, :post_id, :user_id)
    end
```

Now we have to edit the forms to take the current_user's  ID - note that we want to do this behind the scenes, unseen, in the shadows...
We will no longer ask the user for an author name, and use a *hidden field* to populate the user_id

```html
<!-- posts/_form.html.erb -->
<div class="form-group">
  <%= f.text_field :title, placeholder: "Blog Entry Title", class: "form-control" %>
</div>

<div class="form-group">
  <%= f.hidden_field :user_id, value: current_user.id %>
</div>

<div class="form-group">
  <%= f.text_area :blog_entry, placeholder: "Tell Us Something...", class: "form-control" %>
</div>

<div class="actions">
  <%= f.submit "Submit", class: "btn btn-primary" %>
</div>
```

```html
<!-- comments/_form.html.erb -->
<div class="form-group">
  <%= f.text_field :user_id, value: current_user.id %>
</div>

<div class="form-group">
  <%= f.text_area :comment_entry, placeholder: "Tell Me Something...", class: "form-control"  %>
</div>

<div class="form-group">
  <%= f.hidden_field :post_id, value: @post.id %>
</div>

<div class="form-group">
  <%= f.submit "Submit", class: "btn btn-primary" %>
</div>
```

We need to change those places where we mentioned the "author" - we will eventually set up the User with a name, but for now, display their email address.

```html
<!-- posts/show.html.erb -->
<p>
  <!-- instead of @post.author -->
  by <%= @post.user.email %>
</p>
```

```html
<!-- posts/index.html.erb -->
<p>by <%= post.user.email %></p>
```


### Database Reset

Oops! Getting an error? That's because our data doesn't include user_ids on the existing blog posts or comments. We JUST set up that functionality. Let's clear our database and start fresh so everything is linked up properly.

In the Terminal:
```sh
$ rails db:reset
```

Also restart your server. Now you'll have to add Users, Blog Posts and Comments all over again. (A small price to pay for a working app.)


### Add Attribute to Users
Your bloggers have spoken! They want their posts to be credited to a username, not an email address. We can oblige.Start in the Terminal/Command Prompt:

```sh
$ rails g migration AddUsernameToUsers username:string

$ rails db:migrate
```


### Devise Views
Remember that the `rails g migration` only adds the attribute to the table. It does not add anything else. So we need to add places for the user to create a username. But where are those files? They just haven't been made available to us yet! Run this command in the Terminal:
```sh
$ rails g devise:views
```

This will give us access to the Sign-In & Sign-Up html.erb pages, and a whole lot more!


### Backing to Adding an Attribute to Users
Normally, when adding to a Resource's params, we'd visit its controller - but User has no controller, so we'll have to work in the **application_controller.rb**:

```ruby
  before_action :configure_permitted_parameters, if: :devise_controller?

  protected

  def configure_permitted_parameters
    devise_parameter_sanitizer.permit(:sign_up, keys: [:username])

    devise_parameter_sanitizer.permit(:account_update, keys: [:username])
  end
```

For the "keys", you only need to call out the attributes you have added (so, not email and password).

It's your choice if you want the User to sign-in (**devise/sessions/new.html.erb**) using their username or email - let's now focus on giving them the ability to create a username at registration:

**devise/registrations/new.html.erb**
```html
<div>
  <%= f.label :username %><br/>
  <%= f.text_field :username %> 
</div>
```
**devise/registrations/edit.html.erb**

(for this snippet, they're identical):
```html
<div>
  <%= f.label :username %><br/>
  <%= f.text_field :username %> 
</div>
```

Just as we made a button to end our user session, let's make a button to bring us to Devise's User edit page:

```html
<div class="pull-right">
  <% if user_signed_in? %>
   <%= link_to "Edit Profile", edit_user_registration_path, class: "btn btn-primary" %> 
   <%= link_to "Sign Out", destroy_user_session_path, method: :delete, class: "btn btn-danger" %>
  <% end %>
</div>
```

Now go back into the *index* & *show* pages for Posts and show usernames instead of email addresses!

```html
<!-- posts/show.html.erb -->
<p>
  <!-- instead of @post.user.email -->
  by <%= @post.user.username %>
</p>
```

```html
<!-- posts/index.html.erb -->
<!-- instead of post.user.email -->
<p>by <%= post.user.username %></p>
```


### User Images: Choices
Finally, let's talk about getting a profile picture for our users. We have a couple ways we could go with this. 

We could use a file uploader gem to let the users choose their profile pic. The best options are for that:
[Paperclip](https://github.com/thoughtbot/paperclip) (doesn't work so great on PC)
[CarrierWave](https://github.com/carrierwaveuploader/carrierwave)

Or we can use Gravatar
([gravatarify](https://github.com/lwe/gravatarify) gem)
Let's talk about this one first...


### Gravatar
[Gravatar](http://en.gravatar.com/) lets sites display user-provided pictures from a central database. With Gravatar, you end up with the same profile pic for all of your online accounts.

The downside: it does depend on users having signed up for the service (but it is free)

The upside: you don't have to store their uploaded profile pic in your database (+ the gem is easy to use!).

If a user has signed up with Gravatar, we can use the gravatarify gem to easily pull their picture

```ruby
# in the Gemfile:

gem 'gravatarify', '~> 3.0.0'
```
Then run bundle install.

Then we can call for the Gravatar pic. Let's put it with a welcome message next to the Sign Out/Edit Profile buttons.

```html
<div class="container">
  <div class="pull-right">
    <% if user_signed_in? %>
      Welcome, <%= current_user.username %>! 
      <!-- Here it is: -->
      <%= gravatar_tag current_user.email, size: 20 %>
      <!-- -->
      <%= link_to "Edit Profile", edit_user_registration_path, class: "btn btn-primary" %> 
      <%= link_to "Sign Out", destroy_user_session_path, method: :delete, 
      class: "btn btn-danger" %>
    <% end %>
  </div>
  <%= yield %>
</div>
```

Remember, the User you sign-up will need to sign-up with the email Gravatar recognizes (so you need to use a *real* email, not just a fake email).


### Image Uploaders
So if your users aren't into giving Gravatar access to their accounts, we can allow them to upload an image,
using a file uploader gem.

| Paperclip            |            CarrierWave            |
| -------------------- | :-------------------------------: |
| Uses ImageMagick     |         Uses an uploader          |
| Works great on Macs, |        Works great on both        |
| not so great on PCs  | Can utilize ImageMagick if wanted |

We'll use CarrierWave, so no one's left out.
Check out Paperclip's [GitHub Repo](https://github.com/thoughtbot/paperclip) for easy instructions.

#### Quick Note

Gravatar may overwrite any images uploaded with carrier wave.

#### CarrierWave
As with any gem, the first step is to add to our Gemfile:
```ruby
gem 'carrierwave', '~> 1.0'
```
Then run *bundle install*.

Next step is to generate an uploader:
```sh
$ rails generate uploader Avatar
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
    devise_parameter_sanitizer.permit(:sign_up, keys: [:username, :avatar])

    devise_parameter_sanitizer.permit(:account_update, keys: [:username, :avatar])
  end
```

Then we need to "mount" the uploader on the User model:
```ruby
class User < ApplicationRecord
  # Include default devise modules. Others available are:
  # :confirmable, :lockable, :timeoutable and :omniauthable
  devise :database_authenticatable, :registerable,
         :recoverable, :rememberable, :trackable, :validatable

  # here it is!
  mount_uploader :avatar, AvatarUploader

  has_many :posts
  has_many :comments
end
```

Finally, we need to add a configuration in **config/application.rb**:
```ruby
config.autoload_paths += %W(#{config.root}/app/uploaders)
```
(It may work without this step for some people, but was a solution for CW not working for others.)

That takes care of the back end, now we can set-up the front end. First, where to upload:

```html
<!-- 
  add to both devise/registrations/new.html.erb
  and /edit.html.erb
-->

<div class="form-group">
  <strong>Upload a Profile Pic:</strong>
  <%= f.file_field :avatar %>
</div>
```

Then we can display the image like this (depending on the location):
```html
<%= image_tag current_user.avatar.url %>
<!-- or -->
<%= image_tag post.user.avatar.url %>
```
