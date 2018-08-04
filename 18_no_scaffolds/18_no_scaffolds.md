# Rails: No Scaffolds
## Models & Controller Only

No, I don't want no Scaffolds. A scaffold is a generator that can't get no love from me. Hanging out the passenger side of his best friend's Terminal, trying to holler at me.

---

1. Homework Review / Final Project Check-In
2. Why Do We Scaffold?
3. Scaffold-Free App
4. Form Tag
 * Routes
 * Actions
 * Users "Index" View
5. For For
 * Routes
 * Actions
 * Products "Index" View
6. Demystified Scaffold

---

### Why Do We Scaffold?
In the course, when generating new files and directories for our Rails apps, we have generally focused on:
```sh
rails g scaffold
```

And we've talked at length about all that gives us.
We've also discussed other ways to get some, but not all (or at least, not all at once) of content you get from generating a scaffold:

```sh
rails g model

rails g controller
```

But we've rarely done a Rails app where we didn't generate a scaffold. But is it like that in the "real world"? You may hear Rails programmers who say they "never use scaffold". And you'll find some who do admit to it, but they'll probably give the caveat: "I was in a hurry."

So that's one reason to scaffold: Speed
"We gotta just get this app up, and fast!"

That would be one of our reasons in this class, and the other reason is: It's great for beginners!
	1. It's fast
	2. It give a lot from giving little
	3. It allows you to works backwards

Creating a scaffold has let us see what's possible in a Rails app, and then see if we can make it on our own, and even prefer making it on our own (like that first coder we asked).

So let's try out coding in the "real world"...
Let's create a Rails app with controllers, resource models, permitted parameters... but NO SCAFFOLDS!


### Scaffold-Free App
Build a new app! Name suggestions:
scaffold-free, no-scaffs, unscaffolded

Within the app, create two Models:
```sh
$ rails g model User name age:integer
$ rails g model Product name price:decimal quantity:integer
```

Then create a Controller:
```sh
$ rails g controller Welcome index users products
```

We're going to focus on modeling two aspects of the Scaffold:
1. Create an instance of Resource
2. View all instances of a Resource

We've done both of these before without a Scaffold, but we'll review them here, and then see how we can even better mimic a Scaffold without running the scaffold generator. So for review: let's revisit the Ruby Form Tag!


### Form Tag
We've used this one several times before. This Ruby tag creates a general form that is not tied to any specific Resource, but can be used to create one. We're gonna use it to take input on a new User.

```html
<!-- index.html.erb -->
<h1>Home Page</h1>

<div style="margin:50px;">
  <h3>Create a User</h3>
    <%= form_tag create_users_path do %>
			<%= text_field_tag :name, nil, placeholder: "Your Name" %><br />
			<%= number_field_tag :age, nil, placeholder: "Your Age" %><br />
      <%= submit_tag "Create Profile" %>
    <% end %>
</div>
```

Notice where the form_tag is pointed to: create_users_path
This isn't particularly important as far as naming - it could lead to any path - back to this same page, even. But it does mean:
	1. We're going to need a POST route.
	2. We're going to need a new action.
	3. We're going to need code to create a new User in that action.
	4. Do we need a new view?


#### Form Tag: Routes
```ruby
# routes.rb
Rails.application.routes.draw do
  root 'welcome#index'

  post 'create_users' => 'welcome#create_users'
  get 'users' => 'welcome#users'

  # more routes down here,
  # we'll get to those in a bit
end
```

We set our 'index' page as our route, we add a new POST route and we customized our 'users' route.


#### Form Tag: Actions
```ruby
# welcome_controller.rb
class WelcomeController < ApplicationController
  def index

  end

  def create_users
    user = User.create(name: params[:name], age: params[:age])
    redirect_to users_path
  end

  def users
    @users = User.all
  end

  # more stuff down here, we'll get to that
end
```

*Here's what we saw:*

**'index' action**
Nothing to add here! The form_tag works on its own, it does need any help from any code the action.

**'create_users' action**
Our new action! This one does what it's called! It creates a User, using Active Record. But there's no view, so we redirect to the Users action.

**'users' action**
Pretty standard stuff here. You'll recall this kind of action from all the times we scaffolded (aka, the "index" action)


#### User "Index" View
Now to round it out, and fill in the users.html.erb view:
```html
<!-- users.html.erb -->
<h1>All the Users</h1>

<table border="2">
  <tr>
    <th>Name</th>
		<th>Age</th>
  </tr>

  <% @users.each do |user| %>
    <tr>
      <td><%= user.name %></td>
    <td><%= user.age %></td>
		</tr>
  <% end %>
</table>

<p>
<%= link_to "Return to Index", root_path %>
</p>
```

We also could've cut out the middle action...
```ruby
# in routes.rb
	post 'create_users' => 'welcome#users'
	get 'users' => 'welcome#users'
```

```ruby
# in welcome_controller.rb

# no "create_users" action

def users
	if params[:name] != nil
		user = User.create(name: params[:name], age: params[:age])
	end
	@users = User.all
end
```


### Form For
We've used the form_for tag many times - it's in a scaffolded _form file. But we've never made our own!

Let's create one to make Products:
```html
<!-- index.html.erb -->

<h1>Home Page</h1>

<!-- form_tag for Users -->

<div style="margin:50px;">
    <h3>Create a Product</h3>
    <%= form_for @product do |f| %>
	<%= f.text_field :name, placeholder: "Product Name" %><br />
	<%= f.number_field :price, placeholder: "Price", step: :any %><br />
	<%= f.number_field :quantity, placeholder: "Quantity" %><br />
	<%= f.submit %>
    <% end %>
</div>
```

Notice the differences:
```html
<%= form_for @product do |f| %>
  <%= f.text_field :name, placeholder: "Product Name" %><br />
  <%= f.number_field :price, placeholder: "Price", step: :any %><br />
  <%= f.number_field :quantity, placeholder: "Quantity" %><br />
  <%= f.submit %>
<% end %>
```
**form_for** works off an instance variable (representing the resource), not a route.
**form_for** uses an iterator, passing the variable 'f' throughout the inputs, form_tag is like, "Huh?"


#### Form For: Routes

```ruby
# routes.rb
Rails.application.routes.draw do
  root 'welcome#index'

  # those routes for User pages

  post 'products' => 'welcome#create_products'
  get 'products' => 'welcome#products'
end
```

Okay, mostly the same, but... Check out that POST route. Let's talk about that!

```ruby
post 'products' => 'welcome#create_products'
```

Why couldn't the URL be 'create_products' like we did w/ the User routes? Because form_for is looking for a URL which corresponds to this format:

post 'resources' => 'controller#action'

If you run `rails routes` on one of your apps with a scaffold you will see:
```sh
# from our eCommerce App:
Prefix       Verb      URI Pattern              Controller#Action
products     GET    /products(.:format)          products#index
             POST   /products(.:format)          products#create
new_product  GET    /products/new(.:format)      products#new
edit_product GET   /products/:id/edit(.:format)  products#edit
product      GET    /products/:id(.:format)      products#show
            PATCH   /products/:id(.:format)      products#update
             PUT    /products/:id(.:format)      products#update
           DELETE   /products/:id(.:format)      products#destroy
```
(When the prefix seems blank, it actually takes the one listed before)


#### Form For: Actions

```ruby
# welcome_controller.rb
class WelcomeController < ApplicationController
  def index
    @product = Product.new
  end

  # the user actions are here

  def create_products
    product = Product.create(product_params)
    redirect_to products_path
  end

  def products
    @products = Product.all
  end

  private

    def product_params
      params.require(:product).permit(:name, :price, :quantity)
    end
end
```

**'index' action**
form_for needed a little help: an instance variable carrying a new instance of a Product.

**'create_products' action**
Our new action! Similar to create_users, it creates a new instances of the Product resource, but with help from another method, housed at the bottom of the file

**'products' action**
Standard "index" page of all instances of a resource. No big deal here.

But what about that method at the bottom?
```ruby
private

  def product_params
    params.require(:product).permit(:name, :price, :quantity)
  end
```

We've seen that a million times before! But have you thought about what it does? 

**form_tag** just lets in any old parameters, **form_for** wants to you to be discerning...
So we use the product_params method to gather all the parameters we permit to come through.


#### Product "Index" View
And then the product.html.erb view, nothing fancy:

```html
<h1>All the Products</h1>

<table border="2">
    <tr>
        <th>Name</th>
	<th>Price</th>
        <th>Quantity</th>
    </tr>

    <% @products.each do |product| %>
        <tr>
            <td><%= product.name %></td>
            <td><%= number_to_currency product.price %></td>
	    <td><%= product.quantity %></td>
	</tr>
    <% end %>
</table>

<p>
<%= link_to "Return to Index", root_path %>
</p>
```


### Demystified Scaffold
Hopefully that should have cleared some things up about how the resource creation through the Scaffold works.
Everything else involved with the Scaffold should be pretty easy to mimic:
* Show Page
* Edit Page (especially now that you know how to use the form_for)
* Controller Methods