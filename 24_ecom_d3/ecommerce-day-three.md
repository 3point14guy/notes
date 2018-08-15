# eCommerce App
## Day Three: Stripe API & User Sessions

Useful Resources:
[Stripe Documentation](https://stripe.com/docs/checkout/rails)
[Rails Sessions](http://guides.rubyonrails.org/security.html)

---

1. Homework Review
2. eCommerce: Getting Paid
3. Stripe
4. Stripe Test App
 * Gemfile
 * Charges Controller
 * Initializer
 * Views
 * Testing
5. Integrating Stripe in App
 * Gemfile
 * Initialization
 * Order Complete
 * Modify Checkout
 * Testing
6. Sessions
7. Further Topic Suggestions

---

### eCommerce: Getting Paid
Yesterday we somewhat completed the order process in our eCommerce app. We just forgot one important thing: Getting paid.

But how do we collect that money, securely and with ease for our customers?

There's plenty of online payment platforms:
	* Square
	* Google Wallet
	* PayPal

But we're going to be using Stripe (their Rails documentation are quite good).


### Stripe
Stripe is a simple way to take online payments, that can be specifically formatted to work with a Rails app. They make it easy to take credit card payment on web apps – including re-occurring payments.

How does Stripe make money? They take 2.9% of each transaction.

Go to [stripe.com](https://stripe.com/) and Sign Up.

You'll be taken to the Dashboard. It should automatically be in Test mode, but just double check!

Stripe's official instructions on installation can be found on the upper right, under '[Documentation](https://stripe.com/docs/checkout/rails)' (book icon)


### Stripe Test App
Before we implement Stripe in our eCommerce app, let's test it out in its own separate app.

(Make sure you're out of the eCommerce app, then:)
```sh
$ rails new stripe-test
$ cd stripe-test
```

*Lesson for the Students:*
Don't be shy about creating Rails app you have no intention of pushing to production! They can be helpful ways of trying out new APIs, gems, and plug-ins you find before applying them to larger projects.

As we've done before with projects fueled by gems, we'll start in the Gemfile:

```ruby
# Gemfile
gem 'stripe', :git => 'https://github.com/stripe/stripe-ruby'

# for PC users:

gem 'certified'

# will help us avoid SSL errors
```

And, as always, we turn next to the Terminal...
```sh
$ bundle install
```

We'll generate a controller with two pages: **create** and **new**

```sh
$ rails g controller Charges create new
```

The **'new'** view will house a form...
where a new transaction will start
The **'create'** action will do all the work...
the transaction is actually created in there.
The **'create'** view has it easy...
Just display a "Thanks for shopping" message!

The routes for create and new were constructed when we created the controller. But Stripe requires a **"resources"** route (which we only get when we scaffold a resource).

```ruby
# routes.rb
Rails.application.routes.draw do
  get 'charges/create'

  get 'charges/new'

  # add the below line:

  resources :charges
end
```

Completing the controller:
(code comes from the Stripe docs)
```ruby
# charges_controller.rb
class ChargesController < ApplicationController

  def new
    # view houses a form,
    # nothing for us to do here!
  end

  def create
    # this action is what actually performs the transaction.
    # Amount in cents
    @amount = 500

    customer = Stripe::Customer.create(
      :email => 'example@stripe.com',
      :card => params[:stripeToken]
    )

    charge = Stripe::Charge.create(
      :customer => customer.id,
      :amount => @amount,
      :description => 'Rails Stripe customer',
      :currency => 'usd'
    )

    rescue Stripe::CardError => e
    flash[:error] = e.message
  end

end
```

Some gems require an initializer. Some create their own (Devise); some require you to create the file. We'll create this new file within the **config/initializers** directory.

```ruby
# config/initializers/stripe.rb
Rails.configuration.stripe = {

  :publishable_key => "PUBLISHABLE_KEY",

  :secret_key => "SECRET_KEY"

}



Stripe.api_key = Rails.configuration.stripe[:secret_key]
```
To get your **Publishable** and **Secret Keys**, you'll need to go back to your Stripe dashboard and click on **"API"** in the sidebar.

For the views:

```html
<!-- views/charges/new.html.erb -->
<div>
  <%= form_tag charges_path do %>
    
    <article>
      <label class="amount">
        <span>Amount: $5.00</span>
      </label>
    </article>
    
    <script src="https://checkout.stripe.com/checkout.js" 
    class="stripe-button" 
    data-key="<%=Rails.configuration.stripe[:publishable_key] %>" 
    data-description="A month's subscription" 
    data-amount="500"></script>
    
  <% end %>
</div>
```

```html
<!-- views/charges/create.html.erb -->
<h2>Thanks, you paid <strong>$5.00</strong>!</h2>
```

### Restart your Server

And now it's time to test! Run your server and try it out.
For testing purposes, Stripe suggests using the credit card # 4242 4242 4242 4242, with any expiration date, and any three-digit code for the CVC.
Once you click the blue "Pay $5.00" button, it will turn green with a check-mark, and then the Create page will load.

We can now return to our eCommerce app and confidently implement Stripe in there!


### Integrating Stripe in App
After going through the Stripe example slides, let's see if we can apply what we learned to this app. Let's start as we always do when working with a gem:

```ruby
# Gemfile
gem 'stripe', :git => 'https://github.com/stripe/stripe-ruby'
gem 'figaro'
gem 'certified' #for the PC users
```

In the Terminal:
``sh
$ bundle install
$ figaro install
```

We need to create the stripe.rb file in **config/initializers**:

​```ruby
# config/initializers/stripe.rb
Rails.configuration.stripe = {

  :publishable_key => "#{ENV['stripe_test_publishable_key']}",

  :secret_key => "#{ENV['stripe_test_secret_key']}"

}

Stripe.api_key = Rails.configuration.stripe[:secret_key]
```

And define the API key storage variable in the Figaro-produced YAML file:
```yaml
# config/application.yml
stripe_test_publishable_key: YOUR-PUBLISHABLE-KEY-HERE

stripe_test_secret_key: YOUR-SECRET-KEY-HERE
```

This time around we won't need a Charges controller...
We will work within our existing Cart controller. Create a new view called **order_complete.html.erb**, a new action in cart_controller.rb called **order_complete**, and add a new route:
```ruby
# within routes.rb
post 'order_complete' => 'cart#order_complete'
```

For the Order Complete action, most of the code we'll copy from the example app, but the first two lines are unique to this app.

```ruby
# cart_controller.rb
def order_complete
  @order = Order.find(params[:order_id])
  @amount = (@order.grand_total.to_f.round(2) * 100).to_i

  customer = Stripe::Customer.create(
    :email => current_user.email,
    :card => params[:stripeToken]
  )

  charge = Stripe::Charge.create(
    :customer => customer.id,
    :amount => @amount,
    :description => 'Rails Stripe customer',
    :currency => 'usd'
  )

  rescue Stripe::CardError => e
  flash[:error] = e.message
  redirect_to cart_path
end
```

About those first two lines...
```ruby
def order_complete
  @order = Order.find(params[:order_id])
  @amount = (@order.grand_total.to_f.round(2) * 100).to_i
```
We're going to need to modify our Stripe form in order to pass the params[:order_id] -- that'll be from the checkout.html.erb page. The next line is converting the Order's grand total into cents, and then into Integer form (remember, this is how Stripe requires the "amount").

Let's revise the ***checkout** page to include a break-down of the Order costs, and the form at the bottom.

```html
<!-- views/cart/checkout.html.erb -->
<h1>Thanks for Shopping With Us!</h1>
<p>Let's review your order:</p>


<% @order.order_items.each do |key, value| %>
  <ul>
    <li><%= Product.find(key).name %></li>
    <li>Qty: <%= value %></li>
  </ul>
<% end %>

<p>
  <strong>Subtotal: </strong><%= number_to_currency @order.subtotal %> 
</p>

<p>
  <strong>Sales Tax: </strong><%= number_to_currency @order.sales_tax %> 
</p>

<p>
  <strong>Order Total: </strong><%= number_to_currency @order.grand_total %> 
</p>

<div>
  <%= form_tag order_complete_path do %>
    <article>
      <label class="amount">
        <span><%= number_to_currency @order.grand_total %></span>
      </label>
    </article>
    <div>
      <%= hidden_field_tag :order_id, @order.id %>
    </div>

    <script src="http://checkout.stripe.com/checkout.js"
    class="stripe-button"
    data-key="<%= Rails.configuration.stripe[:publishable_key] %>"
    data-description="Order #<%= @order.id %>"
    data-amount="<%= (@order.grand_total.to_f.round(2) * 100) %>"></script>
  <% end %>
</div>
```

Here's a zoomed-in view of the form:
```html
<div>
  <%= form_tag order_complete_path do %>
    <article>
      <label class="amount">
        <span><%= number_to_currency @order.grand_total %></span>
      </label>
    </article>
    <div>
      <%= hidden_field_tag :order_id, @order.id %>
    </div>

    <script src="http://checkout.stripe.com/checkout.js"
    class="stripe-button"
    data-key="<%= Rails.configuration.stripe[:publishable_key] %>"
    data-description="Order #<%= @order.id %>"
    data-amount="<%= (@order.grand_total.to_f.round(2) * 100) %>"></script>
  <% end %>
</div>
```

Lastly, here's the layout of the Order Complete view:
```html
<!-- views/cart/order_complete.html.erb -->
<h2>Thank you for your order!</h2>

<p>
  Order #<%= @order.id %> will be shipping to you, 
  <%= @order.user.name %>, as soon as possible!
</p>
```

And no it's time to fire up the server and test it all out!
Remember to use credit card # 4242 4242 4242 4242 when testing.


### Sessions
We want to institute a way that line items are tied to specific site users, but not necessarily signed-in users. Otherwise, we would have to require people to sign-in to add to a cart, which is preferable to the current situation (there is only one cart, shared by all users), but there's another way...

```ruby
# application_controller.rb
  helper_method :current_order

  def current_order
    if !session[:order_id].nil?
      Order.find(session[:order_id])
    else
      Order.new
    end
  end
```

This **current_order** method is similar to what Devise sets up for us with **current_user**.

A session will be based upon an Order ID. Either there is no session currently (session[:order_id] will equal *nil*), or there is a session, tied to an Order ID, and we'll look up the order by that ID.

We'll create an Order earlier than before, utilizing the current_order method, and build our line_item through Order association.

```ruby
# cart_controller.rb
def add_to_cart
    @order = current_order

    line_item = @order.line_items.new(product_id: params[:product_id], quantity: params[:quantity])
    @order.save
    session[:order_id] = @order.id

    line_item.update(line_item_total: (line_item.product.price * line_item.quantity))

    redirect_back(fallback_location: root_path)
end
```

The session takes on the Order's id.

For the view_order page, we'll want to base the Line Items in that page on the **current_order**.

```ruby
# cart_controller.rb
def view_order
    @line_items = current_order.line_items
end
```

When we are finalizing the order in the checkout action, we'll need to call upon current_order again...

```ruby
# cart_controller.rb
def checkout
  line_items = current_order.line_items

  if line_items.length != 0
    current_order.update(user_id: current_user.id, subtotal: 0)

		@order = current_order

		line_items.each do |line_item|
	    line_item.product.update(quantity: (line_item.product.quantity - line_item.quantity))
	    @order.order_items[line_item.product_id] = line_item.quantity 
	    @order.subtotal += line_item.line_item_total
		end
		@order.save

		@order.update(sales_tax: (@order.subtotal * 0.08))
		@order.update(grand_total: (@order.sales_tax + @order.subtotal))

    @order.line_items.destroy_all

    session[:order_id] = nil
  end
end
```

At the end of action, the Line Items associated with this Order are deleted, and **current_order** session is set to nil.


### Further Topic Suggestions

	* Inventory Control: out-of-stock notices, blocking over-drafting stock.
	* Re-instate Add to Cart page w/ product recommendations.
