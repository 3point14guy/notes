# eCommerce App
## Day Two: Managing Cart & Users

Useful Resources:
[Cancancan Gem](https://github.com/CanCanCommunity/cancancan)

---

1. eCommerce Progress Report
2. Homework Review
3. Items by Category
4. Items by Brand
5. Product Loop Partial
6. Cart Resources
7. Line Items
8. Checkout
9. Cancancan

---

### eCommerce Progress Report
	* Generated the Product & Category resources
	* Set up the "backroom" area for employees to maintain our databases
	* Created a new controller called "Storefront", which contains three views: All Items, Items by Category, and Items by Brand
	* We built the All Items page, utilizing Bootstrap classes
	* We included a modal for "quick info" on a product

### Homework Review
After class you were asked to:
	* Add Devise to the App
	* Add a Navbar to the App

The two tasks were linked: the Navbar should have Sign In/Sign Out links, depending on whether a user was in session or not.

While we're on the subject of the Navbar...

Let's set up our links to the two other pages:
	* Items by Category
	* Items by Brand

These are really more than just two links - there will be a link per Category, and a link per Brand. Let's see how to set that up.

### Items by Category
First up, Links per Category:
```html
<!-- views/layouts/application.html.erb -->
      <div class="collapse navbar-collapse" id="navbarSupportedContent">
        <ul class="navbar-nav">
          <li class="nav-item dropdown">
            <a class="nav-link dropdown-toggle" href="#" id="navbarDropdown" role="button" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
              Shop by Category
            </a>
            <div class="dropdown-menu" aria-labelledby="navbarDropdown">
              <% @categories.each do |category| %>
                <%= link_to category.name, categorical_path(category_id: category.id), class: "dropdown-item" %>
                <div class="dropdown-divider"></div>              
              <% end %>
              <%= link_to 'All', root_path, class: "dropdown-item" %>
            </div>
          </li>
        </ul>
        <!-- brands will go here -->
      </div>
```

If we want an instance variable to be usable anywhere on your app, you need to define it in the... **ApplicationController**!

```ruby
# application_controller.rb
class ApplicationController < ActionController::Base
  protect_from_forgery with: :exception

  before_action :categories

  def categories
  	@categories = Category.order(:name)
  end

  # continues below w/ Devise stuff...
```

We create a method that makes an ActiveRecord call, and then we need to call the method, so we use "before_action".

In the **storefront_controller.rb**, we use the passed parameter (:category_id), to find products associated with that Category.

```ruby
# storefront_controller.rb
def items_by_category
  #to get to this page: catergorical_path(params[:category_id])

  @products = Product.where(category: params[:category_id])
  @category = Category.find(params[:category_id])
end
```
(We set up the **route** yesterday.)

For the **Items by Category** view, it's basically the same as the all_items page, but with a different header. (And the data within @products is different.)

```html
<!-- views/storefront/items_by_category.html.erb -->
<h1>All of our <%= @category.name %></h1>

<div class="row">
  <% @products.each do |item| %>
    <div class="col-md-4">
      <div class="card">
	<div class="card-img-top">
	  <% if item.image.url.nil? == false %>
	    <p><%= image_tag item.image.url, width: "100%" %></p>
	  <% end %>
	</div>
	<div class="card-body">
	  <h4><%= link_to item.name, item, class: "card-title" %></h4>
	  <p><%= number_to_currency item.price %></p>
	  <p>
	    <a href="#" data-toggle="modal" data-target="#modal_<%= item.id %>"><span class="fa fa-plus"></span> Quick Info</a>
	  </p>
	</div>
      </div>
    </div>
  <div class="modal fade" id="modal_<%= item.id %>" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true">
    <div class="modal-dialog" role="document">
      <div class="modal-content">
        <div class="modal-header">
	  <h5 class="modal-title" id="exampleModalLabel"><%= link_to item.name, item %></h5>
	  <button type="button" class="close" data-dismiss="modal" aria-label="Close">
	    <span aria-hidden="true">&times;</span>
	  </button>
        </div>
      <div class="modal-body">
        <% if item.image.url.nil? == false %>
	  <p><%= image_tag item.image.url, width: "100%" %></p>
	<% end %>
	<p><%= number_to_currency item.price %></p>						
	<p><%= item.description %></p>						
	<p><%= item.brand %></p>						
      </div>
      <div class="modal-footer">
	<p></p>
      </div>
    </div>
  </div>
</div>
<% end %>
</div>
```

Run the server and test that functionality out...

### Items by Brand
It'll be a little trickier to get the Brand links working.
We'll need to visit the application_controller.rb again:
```ruby
# application_controller.rb
before_action :categories, :brands

def categories
  @categories = Category.all
end

def brands
  @brands = Product.pluck(:brand).sort.uniq
end
```

	* **.pluck(:brand)** will build our array of just brand names (strings)
	* **.sort** will put them in alphabetical order
	* **.uniq** will take out any duplicates
	* Don't forget to add  ':brands'  to your before_action!

Now we can loop through @brands in the Navbar:
```html
<!-- application.html.erb -->
<!-- Brands -->
        <ul class="navbar-nav">
          <li class="nav-item dropdown">
            <a class="nav-link dropdown-toggle" href="#" id="navbarDropdown" role="button" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
              Shop by Brand
            </a>
            <div class="dropdown-menu" aria-labelledby="navbarDropdown">
              <% @brands.each do |brand| %>
                <%= link_to brand, branding_path(brand: brand), class: "dropdown-item" %>
                <div class="dropdown-divider"></div>              
              <% end %>
              <%= link_to 'All', root_path, class: "dropdown-item" %>
            </div>
          </li>
        </ul>
<!-- End Brands -->
```

Then it's to the controller:
```ruby
# storefront_controller.rb
  def items_by_brand
    @products = Product.where(brand: params[:brand])
    @brand = params[:brand]
  end
```

We collect products with the same brand. Plus, save the param as an instance variable.

Once again, the view is the same as the All Items page, but with a different heading, and while it's the same instance variable name, it holds different data...
```html
<!-- views/storefront/items_by_brand.html.erb -->
<h1>Here are our <%= @brand %> products</h1>

<div class="row">
  <% @products.each do |item| %>
    <div class="col-md-4">
      <div class="card">
        <div class="card-img-top">
          <% if item.image.url.nil? == false %>
            <p><%= image_tag item.image.url, width: "100%" %></p>
          <% end %>
        </div>
        <div class="card-body">
          <h4><%= link_to item.name, item, class: "card-title" %></h4>
          <p><%= number_to_currency item.price %></p>
          <p>
            <a href="#" data-toggle="modal" data-target="#modal_<%= item.id %>"><span class="fa fa-plus"></span> Quick Info</a>
          </p>
        </div>
      </div>
    </div>
<div class="modal fade" id="modal_<%= item.id %>" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true">
  <div class="modal-dialog" role="document">
    <div class="modal-content">
      <div class="modal-header">
	<h5 class="modal-title" id="exampleModalLabel"><%= link_to item.name, item %></h5>
	<button type="button" class="close" data-dismiss="modal" aria-label="Close">
	  <span aria-hidden="true">&times;</span>
	</button>
      </div>
      <div class="modal-body">
        <% if item.image.url.nil? == false %>
	  <p><%= image_tag item.image.url, width: "100%" %></p>
	<% end %>
	<p><%= number_to_currency item.price %></p>						
	<p><%= item.description %></p>						
	<p><%= item.brand %></p>						
      </div>
      <div class="modal-footer">
	<p></p>
      </div>
    </div>
  </div>
</div>
<% end %>
</div>
```

### Product Loop Partial
Before we move on, we sorta have a lot of extra code. Specifically, we've got the same block of code in three different view files: all_items, items_by_category, items_by_brand

All three are basically the same code, except for their headers. So what can we do about it?

Create a PARTIAL!

Let's create a partial in the Storefront views, called "Product Loop", and copy-and-paste the <div class="row"> with the loop through @products into it.

```html
<!-- views/storefront/_product_loop.html.erb -->
<div class="row">
	<% @products.each do |item| %>
		<div class="col-md-4">
			<div class="card">
				<div class="card-img-top">
					<% if item.image.url.nil? == false %>
						<p><%= image_tag item.image.url, width: "100%" %></p>
					<% end %>
				</div>
				<div class="card-body">
					<h4><%= link_to item.name, item, class: "card-title" %></h4>
					<p><%= number_to_currency item.price %></p>
					<p>
						<a href="#" data-toggle="modal" data-target="#modal_<%= item.id %>"><span class="fa fa-plus"></span> Quick Info</a>
					</p>
				</div>
			</div>
		</div>
	<div class="modal fade" id="modal_<%= item.id %>" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true">
		<div class="modal-dialog" role="document">
			<div class="modal-content">
				<div class="modal-header">
					<h5 class="modal-title" id="exampleModalLabel"><%= link_to item.name, item %></h5>
					<button type="button" class="close" data-dismiss="modal" aria-label="Close">
						<span aria-hidden="true">&times;</span>
					</button>
				</div>
				<div class="modal-body">
					<% if item.image.url.nil? == false %>
						<p><%= image_tag item.image.url, width: "100%" %></p>
					<% end %>
					<p><%= number_to_currency item.price %></p>						
					<p><%= item.description %></p>						
					<p><%= item.brand %></p>						
				</div>
				<div class="modal-footer">
					<p></p>
				</div>
			</div>
		</div>
	</div>
<% end %>
</div>
```

That code can then be removed from the three files and replaced with a **render** Ruby tag.

```ruby
<!-- all_items.html.erb -->

<h1>Welcome to Tech Talent Store</h1>
<p>Give us your money!</p>

<%= render 'productloop' %>
```

```ruby
<!-- items_by_category.html.erb -->

<h1>Our Entire Stock of <%= @category.name %></h1>

<%= render 'productloop' %>
```

```ruby
<!-- items_by_brand.html.erb -->

<h1>All of Our <%= @brand %> Products</h1>

<%= render 'productloop' %>
```


### Cart Resources
In the previous slide deck, we mentioned that our Cart would be powered by two Resources, let's create those models now:

*Instructor Note: once again, in the slides, the rails generate command is not explicity spelled out for the students.*

```sh
$ rails g model LineItem product_id:integer quantity:integer line_item_total:decimal order_id:integer
$ rails g model Order subtotal:decimal sales_tax:decimal grand_total:decimal user_id:integer order_items:text
$ rails db:migrate
```

And then we also want to create a separate controller
called Cart, with three views: add_to_cart, view_order, and checkout

```sh
$ rails g controller Cart add_to_cart view_order checkout
```

Then we set up the Associations...

```ruby
# line_item.rb
class LineItem < ApplicationRecord
  belongs_to :product
  belongs_to :order, optional: true
end
```

```ruby
# order.rb
class Order < ApplicationRecord
  has_many :line_items
  belongs_to :user, optional: true

  serialize :order_items, Hash
end
```
(Remember when we serialized in the Twitter app? You'll see what we're doing with it now a little later.)

```ruby
# user.rb
class User < ApplicationRecord
  # Include default devise modules. Others available are:
  # :confirmable, :lockable, :timeoutable and :omniauthable
  devise :database_authenticatable, :registerable,
         :recoverable, :rememberable, :trackable, :validatable

  has_many :orders
end
```

Next, we're tweaking the Cart-related views: changing add_to_cart to a "post" route, and shortening all the URLs

```ruby
# routes.rb
Rails.application.routes.draw do
  
  post 'add_to_cart' => 'cart#add_to_cart'

  get 'view_order' => 'cart#view_order'

  get 'checkout' => 'cart#checkout'

  # remaining routes and down here
```

Let's build the form that will kick-off the path to purchasing. We'll add this form to the footer of our modal:

```html
<!-- views/storefront/_product_loop.html.erb -->
<div class="modal-footer">
  <!-- Delete those "Save Changes" and "Close" buttons that were here -->
  <p>
  <%= form_tag add_to_cart_path do %>
    <%= hidden_field_tag :product_id, product.id %>
    <%= number_field_tag :quantity, nil, placeholder: "How many?" %>
    <%= submit_tag "Add to Cart", class: "btn btn-primary" %>
  <% end %>
  </p>
</div>
```

This will take us to the "Add_to_Cart" page, but not really... Let's see what we'll be doing in the controller action.


### Line Items
In the **cart_controller.rb**, within the **add_to_cart** action, we will us the data we passed from the show page to create a new LineItem.

```ruby
# cart_controller.rb
def add_to_cart
  line_item = LineItem.create(product_id: params[:product_id], quantity: params[:quantity])

  line_item.update(line_item_total: (line_item.quantity * line_item.product.price))

  redirect_back(fallback_location: root_path)
end
```

And notice at the end that we redirect *back* to whatever page we came from - so we never actually see the Add_to_Cart view (you could fill it in later with recommendations for more to purchase!)

The **view_order.html.erb** page will be the place to review the Line Items you've put in your "cart", and to confirm your purchase:

```html
<!-- views/cart/view_order.html.erb -->
<h2>My Cart</h2>

<div>
  <% @line_items.each do |line_item| %>
    <li><%= line_item.product.name %></li>
    <li><%= number_to_currency line_item.product.price %></li>
    <li>Qty: <%= line_item.quantity %></li>
    <li>Subtotal: <%= number_to_currency line_item.line_item_total %></li>
  <% end %>
</div>

<div>
  <%= link_to "Proceed to Checkout?", checkout_path, class: "btn btn-success" %>
</div>
```

In the corresponding action in the cart_controller.rb, we just have one lil' line:

```ruby
# cart_controller.rb
def view_order
    @line_items = LineItem.all
end
```

The next step is to explore what happens when we click the "Proceed to Checkout?" button on this page.

We also need a way to get to this page! Let's add a link in our Navbar:
```html
<!-- within layouts/application.html.erb -->
<!-- after our sign-in link -->
  <ul class="navbar-nav">
    <li class="cart"><%= link_to "", view_order_path, class: "fa fa-shopping-cart fa-2x" %></li>
  </ul>
<!-- Make sure the link is on the right-hand side of the navbar. -->
```


### Checkout
We didn't pass any params through the link, because, as it stands, the only Line Items in the table are yours (we'll address this problem tomorrow).

In the checkout action in the **cart_controller.rb**, we'll build a new Order using the LineItems currently in the table, and we'll associate the Order with a current_user (which means we'll need to have them sign-in once they click that button).

At the top of the cart_controller.rb we're going to set a filter which allows browsing of all the views except "checkout":

```ruby
# cart_controller.rb
class CartController < ApplicationController

  before_action :authenticate_user!, except: [:add_to_cart, :view_order]

  # the Cart controller continues below...
```

So when they click the "Proceed to Checkout?" button, they will be brought to the Devise sign-in page.

Here's what the **checkout** action will look like:
```ruby
# cart_controller.rb
def checkout
  line_items = LineItem.all
  @order = Order.create(user_id: current_user.id, subtotal: 0)

  line_items.each do |line_item|
    line_item.product.update(quantity: (line_item.product.quantity - line_item.quantity))
    @order.order_items[line_item.product_id] = line_item.quantity 
    @order.subtotal += line_item.line_item_total
  end
  @order.save

  @order.update(sales_tax: (@order.subtotal * 0.08))
  @order.update(grand_total: (@order.sales_tax + @order.subtotal))

  line_items.destroy_all
end
```

So what just happened there?
	* Collected all the Line Items
	* We created a new Order
		* Fed it the current_user ID & set subtotal to 0
	* Deduct the quantity ordered from the Product's inventory
	* Build our order_items Hash
	* Calculate the Order subtotal
	* Save the Order!
	* Then calculate sales tax and grand_total (+ saving, using .update()
	* Delete the Line Items currently in the table (so that they won't appear if you return)

With the order confirmed, we give a little overview of what they ordered, using the keys in the order_items Hash to find Product info.

```html
<!-- views/cart/checkout.html.erb -->
<h2>Thanks for shopping with us!</h2>
<p>Let's review your order:</p>

<% @order.order_items.each do |k,v| %>
  <ul>
    <li><%= Product.find(k).name %>, <%= v %></li>
  </ul>
<% end %>

<p>
  <strong>Order Total:</strong> <%= number_to_currency @order.grand_total %>
</p>
```
(Note: using Active Record call in a view is not ideal - we can address this later.)

Go and test out this functionality!


### Cancancan
Way back in the beginning we added a gem called CanCanCan. This gem will help us keep certain areas open to some users and off-limits to others.

But first... let's clean up our navbar so we can logout and create new users, and show who is currently logged in.

```html
<!-- within layouts/application.html.erb -->
<ul class="nav navbar-nav navbar-right">
  <li><%= link_to "View Cart", view_order_path %></li> <!-- already here -->
  <!-- add this code: -->
  <% if user_signed_in? %>
    <li class="dropdown">
      <a href="#" class="dropdown-toggle" data-toggle="dropdown" role="button" aria-haspopup="true" aria-expanded="false">
      Hello, <%= current_user.email %> <span class="caret"></span></a>
      <ul class="dropdown-menu">
        <li><%= link_to "Edit Profile", edit_user_registration_path %></li>
        <li><%= link_to "Sign Out", destroy_user_session_path, method: :delete %></li>
      </ul>
    </li>
  <% else %>
    <li><%= link_to "Sign In", new_user_session_path %></li>
  <% end %>
</ul>
```

Running this in our terminal will create an ability model, with a few examples in it:
```sh
$ rails g cancan:ability
```

Let's add **role** to our user and don't forget to add it to the sanitizers in the **application_controller**.
```sh
$ rails g migration AddRoleToUser role:string
$ rails db:migrate
```
Don't forget to add role to your devise sanitizers (**application controller**). Remember, without this the role will not be saved.

```ruby
# application_controller.rb
before_action :configure_permitted_parameters, if: :devise_controller?

def configure_permitted_parameters
   devise_parameter_sanitizer.permit(:sign_up, keys: [:role])
   devise_parameter_sanitizer.permit(:account_update, keys[:role])
end
```

If the code above gives you an error, try this:
```ruby
before_action :configure_permitted_parameters, if: :devise_controller?

def configure_permitted_parameters

  devise_parameter_sanitizer.permit(:sign_up) { |u| u.permit(:email, :password, 
     :password_confirmation, :role) }

  devise_parameter_sanitizer.permit(:account_update) { |u| u.permit(:email, :password, 
     :password_confirmation, :current_password, :role) }
end
```

In our **products/index.html.erb** file, let's make a conditional on the New, Edit and Destroy links. This will make the links disappear to users without that privilege.

```html
<% if can? :update, product %>
  <td><%= link_to 'Edit', edit_product_path(product) %></td>
  <td><%= link_to 'Destroy', product, method: :delete, data: { confirm: 'Are you sure?' } %></td>
<% end %>
```

```html
<% if can? :create, Product %>
  <%= link_to 'New Product', new_product_path %>
<% end %>
```

#### The *can* method
The *can* method is used to define permissions and requires two arguments. The first one is the action you're setting the permission for, the second one is the object you're setting it on.

You can pass *:manage* to represent any action and :all to represent any object.

```ruby
can :manage, Product  # user can perform any action on the Product
can :read, :all       # user can read any object
can :manage, :all     # user can perform any action on any object
```

Common actions are *:read, :create, :update* and *:destroy* but it can be anything. See the [Defining Abilities](https://github.com/CanCanCommunity/cancancan/wiki/defining-abilities#the-can-method) in the documentation for more.

In our **products controller**, we need a line for authentication:
```ruby
# products_controller.rb
before_action :authenticate_user!, except: [:show]
```

This line opens up only our **show** page to non-users.

The way Cancancan sets permissions is via abilities (what the user can do) and roles (what role the user plays).

Roles are defined in the User model:
```ruby
# user.rb
def admin?
    role == "admin"
end
```

Abilities are defined in the Ability model:
```ruby
# ability.rb
if user.admin?
    can :manage, Product
end
```

In this example, we've defined an admin role, and granted it permission (or ability) to *:manage* (meaning, "full permissions") the Product resource.

We have a slight problem.  How do we set someone as an admin?  
Enter... our hero: rails c.
Create a new user (in the UI) with admin in the email (so you remember) Then let's find that user using rails console and set their role to admin.

```sh
:002 > User.pluck(:id, :email, :role)
   (0.2ms)  SELECT "users"."id", "users"."email", "users"."role" FROM "users"
 => [[1, "rzapata@gmail.com", nil], [2, "admin@gmail.com", nil], [3, "guest@gmail.com", nil]]
:003 > u = User.find(2)
:004 > u.update(role: "admin")
:005 > u #Make sure it saved.
```

An admin is born!
Later, we may want to create a new page that lets current admin(s) assign additional users as admins.


#### Challege One
Because we've locked up our products controller, our guests can't see the show page for a product. Let's fix this. (Hint: Think back to if and elsif.)

```ruby
# ability.rb
if user.admin?
    can :manage, Product
elsif user.guest?
    can :show, Product
end
```

```ruby
# user.rb
def guest?
  role == 'guest'
end
```

#### Challenge Two
Constantly having to go through and setting roles for our users is a pain. Let's go ahead and make it so every user is automatically a guest. Hint: Make sure they can't see the field.

```html
<!-- views/devise/registrations/new -->
<div class="form-group">
    <%= f.hidden_field :role, value: "guest" %>
</div>
```
