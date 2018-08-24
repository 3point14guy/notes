# Omniauth Gem
## Social Media Authentication

Using the Omniauth gem to sign-in via social media accounts.

### Useful Resources:###

- [Instagram Developer](https://www.instagram.com/developer/)
- [Omniauth Gem](https://github.com/omniauth/omniauth) 
- [GitHub For Developers](https://developer.github.com/)

---

1. Homework Review / Final Project Check-In
2. Omniauth Intro
3. Instagram Developer
4. Gem Time
5. User Model
6. Sessions Controller
7. Home Controller
8. Omniauth Initializer
9. View Set-Up
10. GitHub & Omniauth

---

### Omniauth Intro
The Omniauth gem allows your app's Users to sign in via their social media accounts. We can then access the User's name, image, and more!

## Become an Instagram Developer

The first step is to visit [https://www.instagram.com/developer/](https://www.instagram.com/developer/)

Once you're signed in (just your normal Instagram sign-in):

1. click on "Manage Clients" in the navigation bar.
2. On the next screen, click the  "Register A New Client" button.
3. Give your App a Name and a short description. 
4. Make the Website URL: "http://localhost:3000/"
5. Make the Valid Redirect URL: "http://localhost:3000/auth/instagram/callback" and press ENTER
6. At the bottom of the screen, click "Register".
7. Once the client is created, click the "Manage" button.  Make note of your Client ID and Client Secret Keys.

 


### Gem Time
In a new project, or an existing one without Devise, add these the FB Omniauth gem to your **Gemfile**:

```ruby
# Gemfile
gem 'omniauth-instagram'
gem 'figaro'
```

As before, we'll use Figaro to keep our API keys secret! And then, of course, in your Terminal run...
```sh
$ bundle install
$ figaro install
```


### User Model
Let's generate a model called "User"...
```sh
$ rails g model User provider:string uid:string name:string
```

We don't need all those extra pages a scaffold would set up.
We created a table in our database: so don't forget to...

```sh
$ rails db:migrate
```

## Complete the Model##

Fill in the **user.rb** model with the following code:

```ruby
# user.rb
class User < ActiveRecord::Base
  def self.create_with_omniauth(auth)
    create! do |user|
      user.provider = auth["provider"]
      user.uid = auth["uid"]
      user.name = auth["info"]["name"]
    end
  end
end
```

## Application Controller##

We'll need to add some code to our **application_controller.rb**:

```ruby
# application_controller.rb
class ApplicationController < ActionController::Base
  # Prevent CSRF attacks by raising an exception.
  # For APIs, you may want to use :null_session instead.
  protect_from_forgery with: :exception

  # add the code shown below:

  helper_method :current_user
 
  private
 
  def current_user
    @current_user ||= User.find(session[:user_id]) if session[:user_id]
  end
end
```


### Sessions Controller
Next, we need to create a new Controller.
This controller will have two actions - **create** and **destroy** - but we don't necessarily need views associated with those actions, and we'll be augmenting the routes, so it's up to you if you want to do:

```sh
$ rails g controller Sessions create destroy
```

Or you can create the file (**sessions_controller.rb**) manually in Sublime.

However you went about creating it, let's now fill in **sessions_controller.rb**:

```ruby
# sessions_controller.rb
class SessionsController < ApplicationController
  def create
    auth = request.env["omniauth.auth"]
    user = User.find_by_provider_and_uid(auth["provider"], 
    auth["uid"]) || User.create_with_omniauth(auth)
    session[:user_id] = user.id
    redirect_to root_url, :notice => "Signed in!"
  end
 
  def destroy
    session[:user_id] = nil
    redirect_to root_url, :notice => "Signed out!"
  end
end
```


### Home Controller
So far, we don't actually have a page to visit to test this project out. So let's just create a **Home** controller, with one page: **Index**.

```sh
$ rails g controller Home index
```

## Change Your Routes

We'll need some custom routes for the Omniauth-related actions, and then let's set that page we just created (home/index.html.erb)

```ruby
# routes.rb
Rails.application.routes.draw do
  root "home#index"
  get "/auth/:provider/callback" => "sessions#create"
  get "/signout" => "sessions#destroy", :as => :signout
end
```


### Omniauth Initializer
Within the **'config'** folder, and then within the **'initializers'** folder, create a new file:

```ruby
# config/initializers/omniauth.rb
OmniAuth.config.logger = Rails.logger

Rails.application.config.middleware.use OmniAuth::Builder do
  provider :instagram, ENV['INSTAGRAM_ID'], ENV['INSTAGRAM_SECRET'], scope:['basic']
end
```



Now let's set up those ENV variables in the file that Figaro helped us set-up (**application.yml**). 

## Keep It Secret##

In this file you will insert your APP ID and Secret we created way back in the beginning (@ [developers.facebook.com](https://developers.facebook.com/)).

```yaml
# config/application.yml

INSTAGRAM_ID: YOUR-CLIENT-ID
INSTAGRAM_SECRET: YOUR-CLIENT-SECRET-ID

#Add your keys without quotations.

```



## Back at Home##

Now, either on **home/index.html.erb**, or just the **application.html.erb** view, add Sign In/Sign Our code:

```html
<!-- home/index.html.erb -->
<h3>Welcome!</h3>
  <div id="user-widget">
	<% if current_user %>
	     Welcome <strong><%= current_user.name %></strong>!<br />
	    <%= image_tag current_user.image %><br />
		  
	    <%= link_to "Sign out", signout_path, id: "sign_out" %>
	<% else %>
	    <%= link_to "Sign in with Github", "/auth/github" %><br />
	     <%= link_to "Sign in with Instagram", "/auth/instagram" %>
		    
	<% end %>
    </div>
```

You'll see that once a User signs in through Instagram, we will display their name and Instagram profile pic.

And now it's time to fire up the server and test this out!

```shell
$ rails server
```




### ##There's an Oauth for That
Signing in with Instagram Omniauth is just the beginning.
There are Omniauth gems for:
	1. Twitter
	2. LinkedIn
	3. GitHub
	4. More!

Once you have one Omniauth sign-in set up, adding additional ones is really easy. Let's walk through adding just one more: **GitHub**

Once you see how to add this one, try out more on your own.

```ruby
# Gemfile
gem 'omniauth-instagram'
gem 'omniauth-github'
gem 'figaro'
```

```sh
$ bundle install
```

Remember when we created **omniauth.rb** in the **/config/initializers** folder? Let's head back to that file and add a line for GitHub:

```ruby
Rails.application.config.middleware.use OmniAuth::Builder do
  provider :github, ENV['GITHUB_KEY'], ENV['GITHUB_SECRET'], scope: "user:email,user:follow"
  provider :instagram, ENV['INSTAGRAM_ID'], ENV['INSTAGRAM_SECRET'], scope:['basic']
end
```

Like we got keys from developers.facebook.com, we'll need to get API keys from GitHub. So go to GitHub and click on your name on the right-hand side of the navbar, you'll be taken to your profile page. Then click on "Edit Profile". Then click on "Applications" (in the menu on the left).

Click on the "Developer Applications" tab, then click the "Register New Application" button. For the Homepage URL and Authorization callback URl, enter the values seen in the image in the slides.

	* Application Name
		* TTS Omniauth Example App
	* Homepage URL
		* http://localhost:3000
	* Application description
		* Showing TTS students how to use Omniauth login
	* Authorization callback URL
		* http://localhost:3000

Once you create the application, you'll be taken to a screen where your **CLIENT ID** and **CLIENT SECRET** are on the right-hand side. Grab those and paste them into **application.yml**!

```yaml
GITHUB_KEY: github_client_id
GITHUB_SECRET: github_client_secret

INSTAGRAM_ID: YOUR-CLIENT-ID
INSTAGRAM_SECRET: YOUR-CLIENT-SECRET-ID
```

In **home/index.html.erb**, put in a link to sign in with GitHub, changing the URL linked to as well. Also, let's put a conditional around the image_tag (we'll see why on the next slide):

```html
<!-- home/index.html.erb -->
<body>
    <h3>Welcome!</h3>
  	<div id="user-widget">
	    <% if current_user %>
	        Welcome <strong><%= current_user.name %></strong>!<br />
		<%= image_tag current_user.image %><br />
		<%= link_to "Sign out", signout_path, id: "sign_out" %>
	    <% else %>
		<%= link_to "Sign in with Github", "/auth/github" %><br />
		<%= link_to "Sign in with Instagram", "/auth/instagram" %>
	    <% end %>
    	</div>
    <%= yield %>
  </body>
</div>
```

Now test this one out!

Copyright © Tech Talent South. All rights reserved. 