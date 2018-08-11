

# eCommerce App

## Day One: Initial Set-Up

We will review scaffolding, association and Bootstrap layout. We'll introduce some new Gems, some new Bootstrap, and Seeding.

Useful Resources:
[Database Seeding](http://edgeguides.rubyonrails.org/active_record_migrations.html#migrations-and-seed-data)
[Bootstrap Modals](http://getbootstrap.com/javascript/#modals)
---

1. Homework Review
2. App Checklist
3. eCommerce Resources
4. Gem Time
5. Database Seeding
6. Image Uploader
7. Product Form
8. Storefront Controller
9. Bootstrap Modal

---

### App Checklist

Here's what we're going to do over the next three days:

1. Let's start with a customer-free store (as shady as that sounds...)
2. We'll have products that are shown all together, or shown by category
3. You can buy one or more of each product, and we'll add them to a cart.
4. Then we'll tie a user to that cart ("check out").
5. At the end we'll learn how to implement a payment process.


### eCommerce Resources

What will our resources be for this app?

**Product**
	* Name
	* Price
	* Quantity
	* Description
	* Brand
	* category_id?

**Category**
	* Name

What kind of Association should we use?
	* One-to-Many
	* Many-to-Many

Depending on what we're selling it could go either way. If we were selling lots of various stuff (like Amazon), we would probably go with a Many-to-Many (many products within the same category, products can belong to many categories). But for the class, we'll keep it simple, selling just Electronics, necessitating only a One-to-Many Association.

That takes care of the inventory side of things. When it comes to shopping-and-purchasing, we'll have Cart resources. We won't have a Cart resource, exactly, but two separate resources: one to store an item in as you shop, the other gather those items together.

**LineItem**
	* product_id
	* quantity
	* line_item_total
	* order_id

**Order**
	* sub_total
	* sales_tax
	* grand_total
	* customer_id
		* how we'll eventually tie a user (customer) to what they want to buy

Let's scaffold the Products resource:

*Note to instructor: the students should be able to run a `rails g` command on their own, so in the notes we'll just layout resource names and attributes and data-types.* 
```sh
$ rails g scaffold Product name:string price:decimal
quantity:integer description:text brand:string rating:integer category_id:integer
```

By adding "category_id:integer" to this scaffold, we have decided to go the route of a One-to-Many Association. Let's set up the Category scaffold, and then complete the Association process.

For the Category resource, let's just build a model, rather than a scaffold, for this resource. We'll find out a new way to populate this table in a couple slides.

```sh
$ rails g model Category name:string
$ rails db:migrate
```

Code the association keywords into the two models:

```ruby
# product.rb
class Product < ApplicationRecord
  belongs_to :category
end
```

```ruby
# category.rb
class Category < ApplicationRecord
  has_many :products
end
```

#### Backroom/Storefront
We will use the scaffolded Views as our "backroom", a place for employees (admin users) can create and update products and categories.

We'll create new views a little later (via a new controller) to act as our "storefront" - views for our customers to peruse products (all or by category) and purchase.


### Gem Time
We've let scaffolding do most of the work for us here, but it's time for us to take control and start customizing.

```ruby
# Gemfile

gem 'jquery-rails'
gem 'bootstrap', '~>4.1.3'
gem 'font-awesome-rails'

gem 'devise'
gem 'paperclip'  # or gem 'carrierwave'
gem 'cancancan'

gem 'hirb'       # or 'pry-rails'; for rails c
gem 'better_errors', group: :development
```

```sh
$ bundle install
```

Start with Bootstrap (we'll set up the rest later).  Set it up (remember the four steps?), change the Product resource index view to use the Bootstrap table, and wrap your yield in a container.

```html
<!-- views/products/index.html.erb -->
<table class="table">
```

```html
<!-- views/layouts/application.html.erb -->
<div class="container">
  <%= yield %>
</div>
```

An explanation of some of the new gems:

```ruby
gem 'cancancan'
```
This authorization gem allows us to give certain abilities to particular Users.


```ruby
gem 'hirb'  # or 'pry-rails'; for rails c
```Reformats the look of rails console; "hirb" requires you to enter Hirb.enable each time you enter the console


```ruby
gem 'better_errors', group: :development
```
This gem will replace the default red error screen with a more helpful error screen. Note that it should only be implemented in Development.


### Database Seeding
Now, let's take care of Categories.  Categories don't change often and are usually consistent.  Wouldn't it be nice if we could pre-populate them so we don't have to do it manually?

```ruby
# db/seeds.rb
# This file should contain all the record creation needed to seed the database with its default values.
# The data can then be loaded with the rails db:seed command (or created alongside the database with db:setup).
#
# Examples:
#
#   movies = Movie.create([{ name: 'Star Wars' }, { name: 'Lord of the Rings' }])
#   Character.create(name: 'Luke', movie: movies.first)

categories = Category.create([{name: "Computers"}, {name: "Smart Phones"}, 
  {name: "Televisions"}, {name: "Game Consoles"}, {name: "Video Games"},
  {name: "Appliances"}, {name: "Other"}])
```

In the Terminal run:
```sh
$ rails db:seed
```

Check out `rails c` to see that we now have a populated Category table.


### Image Uploader

	#### Paperclip Implementation

	​```sh
	$ rails g paperclip product image
	​```
	
	​```ruby
	# product.rb
	class Product < ActiveRecord::Base
	  has_attached_file :image, styles: { medium: "300x300>", thumb: "100x100>" }, default_url: "/images/:style/missing.png"
	  validates_attachment_content_type :image, content_type: /\Aimage\/.*\Z/
	
	  belongs_to :category
	end
	​```
	
	#### CarrierWave Implementation
	
	​```sh
	$ rails g uploader Image
	$ rails g migration AddImageToProducts image:string
	$ rails db:migrate
	​```
	
	​```ruby
	# product.rb
	class Product < ActiveRecord::Base
  	mount_uploader :image, ImageUploader

  	belongs_to :category
	end
	​```
	
	​```ruby
	# config/application.rb
	config.autoload_paths += %W(#{config.root}/app/uploaders)
	​```

For either image uploader:
	​```ruby
	#products_controller.rb
		def product_params
			params.require(:product).permit(:name, :price, :quantity, :description, :brand, :rating, :category_id, :image)
		end
	​```


### Product Form
In the Product form partial, we'll...
* Remove the div for taking a rating (we'll do that later)
* Put in a field for taking an uploaded image
* Tweak the field that takes the category_id so that it's a drop-down menu to select a category
* Change the "fields" to "form-groups" and add ```class: "form-control"```(Bootstrap)
* The submit button is still ugly...need someone to take charge of that...

```html
<!-- views/products/_form.html.erb -->
<div class="form-group">
  <%= f.select :category_id, @categories.collect { |c| [ c.name, c.id ]}, {include_blank: "Please select a category" }, class: "form-control" %>
</div>

<strong>Upload Product Image: </strong>
<div class="custom-file">
  <%= form.label :image, class: "custom-file-label" %>
  <%= f.file_field :image, class: "custom-file-input %>
</div>
				   
<%= button_to "Submit", action: :create, controller: :products, class: "btn btn-light" %>
<!-- or -->
<%= button_to "Submit", product, method: :post, class: "btn btn-light" %>										      
<!-- <% end %> -->
```

To power that drop-down menu of Categories, we'll need to add two lines of code to our Products controller

```ruby
# products_controller.rb
def new
  @product = Product.new
  @categories = Category.all
end

def edit
  @categories = Category.all
end
```

Why do we add @categories to new and edit? 

Now you can go through and make a couple products for us to sell!

**Formatting hint:**
Hint: put **number_to_currency** in front of the product.price to format the price into proper currency form ($, commas, decimals)
```html
<td><%= number_to_currency product.price %></td>
```


### Storefront Controller
Let's give the customers three ways to view products:
* All Products
* Filter by Category
* Filter by Brand (a Product attribute)

Create a new controller:
```sh
$ rails g controller Storefront all_items items_by_category items_by_brand
```

Let's change our routes:
```ruby
# routes.rb
  root 'storefront#all_items'

  get 'categorical' => 'storefront#items_by_category' 

  get 'branding' => 'storefront#items_by_brand'
```

Since it will be front-facing, let's set up the **all_items** View like we've set up index pages in past apps (i.e., divs with Bootstrap classes).

Make sure to update your Storefront controller so that you can display the all items page properly

```ruby
  def all_items
    @products = Product.all
  end
```

```html
<!-- views/storefront/all_items.html.erb -->
<h1>Welcome to Tech Talent Store</h1>
<p>Give us your money!</p>

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
						<a href="#"><span class="fa fa-plus"></span> Quick Info</a>
					</p>
				</div>
			</div>
		</div>
	<% end %>
</div>
```

	* The "+ quick info" link will bring up a modal with product highlights. 
	* The Rating system is commented out until we set that up for the Users.

Run the server and test it out. You may need to override height on the "card" class to get everything looking good.


### Bootstrap Modal
Let's test a demo modal on our page before fitting it into our product loop.

Go to: [Bootstrap Modals](http://getbootstrap.com/javascript/#modals)

Copy the code from the "Live Demo" section, and paste it in the bottom of your **all_items.html.erb** file.

```html
<!-- all_items.html.erb -->
<!-- at the bottom of the page -->

<!-- Button trigger modal -->
<button type="button" class="btn btn-primary" >
  Launch demo modal
</button>

<!-- Modal -->
<div class="modal fade" id="exampleModal" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true">
  <div class="modal-dialog" role="document">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title" id="exampleModalLabel">Modal title</h5>
        <button type="button" class="close" data-dismiss="modal" aria-label="Close">
          <span aria-hidden="true">&times;</span>
        </button>
      </div>
      <div class="modal-body">
        ...
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
        <button type="button" class="btn btn-primary">Save changes</button>
      </div>
    </div>
  </div>
</div>
```

Refresh that page and test it out...
	* Clicking on the "Launch demo modal" button will bring up the modal you see here.
* If it did not work, there's a JavaScript error.
 * Look in the Inspect console, or try removing Turbolinks.

Let's use that modal code to create our own "quick info" modal. First, take the functionality from the demo button and move it to the anchor tags around "+ quick info".

In addition, add "_<%= product.id %>" to the data-target, this will let us have a unique element ID each time we loop through.

```html
<!-- all_items.html.erb -->
<p>
  <a href="#" data-toggle="modal" data-target="#modal_<%= product.id %>">
    <span class="fa fa-plus"></span> Quick Info
  </a>
</p>
```

Next move the modal code into a space between the <% end %> tag and the last </div> to close above that:
```html
<!-- all_items.html.erb -->


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
        <p>made by <%= item.brand %></p>
      </div>
      <div class="modal-footer">
        <p>
          <!-- empty for now -->
        </p>
      </div>
    </div>
  </div>
</div>
	 
	* At the top of this code you'll find id="exampleModal"
	* add "_<%= product.id %>" to that so it corresponds to the data-target in the anchor tag above.

Refresh and your modal should be working for each product!
```
