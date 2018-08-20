# Open Weather Map API
## Gather Weather Data

This will likely be the class' first foray into APIs (besides Stripe, which works differently). We will use Open Weather Map's free API to gather weather data on cities across the globe.

Useful Resources:
[Open Weather Map](https://home.openweathermap.org/)
[Figaro Gem](https://github.com/laserlemon/figaro)
[JSON Official Site](http://www.json.org/)
[W3Schools JSON Intro](https://www.w3schools.com/js/js_json_intro.asp)
[50 Most Useful APIs for Developers](http://www.computersciencezone.org/50-most-useful-apis-for-developers/)

---

1. Homework Review
2. API: An Introduction
* Client Side vs. Server Side
* JSON
* API Keys
3. Open Weather Map: Sign Up & Keys
4. Weather App
* Controller
* Gems
5. API Documentation
6. Test Action/View
7. Bootstrap Set-Up
8. Index Action/View
9. Rails Challenge
* Save-able locations

---

### Quick Notes

Newer Rails no longer carries the JQuery Dependency, for users running rails 5.1 or newer they will need to add the gem 'jquery-rails' into their gem file. They will also need to update the application.js file to add:

//= require jquery & //= require jquery_ujs

Without the additon of these files you will recieve errors in the console pertaining to jquery 



### API: An Introduction

**Application Programming Interface**
Allows an application like yours to make HTTP requests to another application that lives somewhere on the web!

We will be working with REST API. With this type of API your application will make a request to a URL, kind of like how a browser goes to a web address. The server will parse the URL and determine what data was requested. Then that server will return the requested data in the JSON format.

[Google Maps Example](http://maps.googleapis.com/maps/api/geocode/json?address=30318)

APIs are generally structured around a well-defined Client-Server relationship:

| Client                       | Server                                   |
| ---------------------------- | ---------------------------------------- |
| Sends a request              | Receives & parses the request            |
| Waits for the response       | Retrieves data from its sources          |
| Parses & relays the response | Compiles data & sends a response to the client |

**JSON**
*JavaScript Object Notation*

	* Collection of name/value pairs
	* JSON is easy to read 
	* Similar key/value relation like we see in hashes in Rails

#### API Keys
More often than not, an API provider will require you to register with them and acquire an API Key. The key will be a unique set a characters that should only be used by you (or your company). Through the key, the calls you make to the API have a unique stamp.

There are tons of APIs out there. Some grant access for free, some for a fee.


### Open Weather Map: Sign Up & Keys
Let's get you an API key!
Visit: [openweathermap.org/price](https://openweathermap.org/price)
Choose the Free Plan and then select "Get API Key and Start"...
You'll be taken to a page and asked to fill in several fields,
then taken to a page where you'll find your API key in the spaces that have been blacked out (in image in slide deck).

Now that we have our API key, let's create an app to utilize it!


### Weather App
New rails app, possible names:
	* weather_app
	* fair-weather-app
	* weathering-it

Once inside the app, we'll create a **Welcome** controller with two pages:
	* **TEST** - we'll make a call to the API using a hard-coded location
	* **INDEX** - we'll rely on user-input to provide us a location to use in the API call

```sh
$ rails g controller Welcome text index
```

We're going to use a trio of gems for this app:
```ruby
# Gemfile
gem 'bootstrap', '~>4.1.3'
gem 'httparty'
gem 'figaro'
gem 'jquery-rails' *** (only required if running rails 5.1 or newer)
```

**httparty** will help use parse through the data sent by the API
**figaro** will create a safe place to store our API key
And **bootstrap**... well, you know what's that's for

In addition to running "bundle install", you'll also need to run "figaro install", which will produce the file "application.yml" in your /config folder.
Store your API key in here:
```ruby
# Add configuration values here, as shown below.
#
# pusher_app_id: "2954"
# pusher_key: 7381a978f7dd7f9a1117
# pusher_secret: abdc3b896a0ffb85d373
# stripe_api_key: sk_test_2J0l093xOyW72XUYJHE4Dv2r
# stripe_publishable_key: pk_test_ro9jV5SNwGb1yYlQfzG17LHK
#
# production:
#   stripe_api_key: sk_live_EeHnL644i6zo4Iyq4v1KdV9H
#   stripe_publishable_key: pk_live_9lcthxpSIHbGwmdO941O1XVU

openweather_api_key: yourkeyhere
```

Why are we using this gem and creating this new file just for one line of code?
We want to keep our API keys secret. **application.yml** appears in your .gitignore file. It will not be pushed to GitHub.


### API Documentation
Before you make an API call, it's a good idea to see the data you'll be fetching, and how it's structured. So head over to the [openweathermap.org/current](https://openweathermap.org/current) and Select 'JSON' under the 'Parameters of API respond' from the right menu.

(visuals within slide deck)

This is the data that would be sent if you made an API call for Cairnes, Australia. Keep scrolling down to see all the data!


### Test Action/View
Time to set-up our Test action:

```ruby
# welcome_controller.rb
class WelcomeController < ApplicationController
  
  def index
  	# nothing yet
	end
 
  def test
    response = HTTParty.get("http://api.openweathermap.org/data/2.5/weather?zip=28204&units=imperial&appid=#{ENV['openweather_api_key']}")
    @location = response['name']
    @temp = response['main']['temp']
    @weather_icon = response['weather'][0]['icon']
    @weather_words = response['weather'][0]['description']
    @cloudiness = response['clouds']['all']
    @windiness = response['wind']['speed']
  end
end
```

We pull a URL from the docs, then add our location, and you'll want to use the variable you set for your API key in application.yml. 

Note the two different ways we could save the data in this slide (@results Hash), and the previous (all separate instance variables).

```ruby
# welcome_controller.rb
class WelcomeController < ApplicationController
  
  def index
  end
 
  def test
    response = HTTParty.get("http://api.openweathermap.org/data/2.5/weather?zip=
      75001&units=imperial&appid=#{ENV['openweather_api_key']}")
  
    @results = {}
    @results[:location] = response['name']
    @results[:temp] = response['main']['temp']
    @results[:weather_icon] = response['weather'][0]['icon']
    @results[:weather_words] = response['weather'][0]['description'] 
    @results[:windiness] = response['wind']['speed']
    @results[:cloudiness] = response['clouds']['all']
end
```

So you'll need to bring that in, and then we can build test.html.erb (we'll base this on the separate instance variables version).

```html
<!-- views/welcome/test.html.erb -->
<div class="row">
  <div class="col-md-6">
    <div class="card">
      <div class="card-header bg-success">
        <h1 class="card-title">Forecast for
          <%= @location %></h1>
        </div>
      <div class="row">
        <div class="card-body">
          <div class="col-md-7">
            <p>
              Temperature is:
              <%= @temp %>°F
            </p>
            <p>
              Cloud Cover:
              <%= @cloudiness %>%
            </p>
            <p>
              Wind:
              <%= @windiness %>
              mph
            </p>
          </div>
          <div class="col-md-5">
            <p>
              <%= @weather_words%>
              <img src="http://openweathermap.org/img/w/<%= @weather_icon%>.png">
            </p>
          </div>
        </div>
      </div>
    </div>
  </div>
</div>
```

Fire up the server and try it out!

### Bootstrap Set-Up
Wait... things don't look right.
That's right, we haven't finished setting up Bootstrap

**application.css** => **application.scss**

```css
# assets/application.scss

@import "bootstrap";
```

```javascript
// In application.js:

//= require jquery
//= require jquery_ujs
//= require bootstrap-sprockets
//= require turbolinks
//= require_tree .
```

*Students:* If you had already done this right after we added to our Gemfile, please raise your hand and the instructor will bring you your gold star.


### Index Action/View
Obviously this a very limited app if you have to hard-code in the location. Let's set up the **index** page to take a city and state from the user, and give them back that location's forecast!

First the form to take user input...
```html
<!-- views/welcome/index.html.erb -->
<div class="row">
	<div class="col-md-6 col-md-offset-1">
		<div class="card">
			<div class="card-header bg-warning">
				<h5>Look up your local forecast: </h5>
			</div>
			<div class="card-body">
				<div class="form-inline">
					<%= form_tag index_path do %>
					<%= text_field_tag :zipcode, nil, placeholder: "enter zipcode", class: "form-control-sm" %>
					<%= submit_tag "Check Weather", class: "btn btn-warning btn-sm" %>
					<% end %>
				</div>
			</div>
		</div>
	</div>
</div>
```

Then a place to display the results...
```
<!-- views/welcome/index.html.erb -->
<!-- Our response from openweathermap's API -->
<br>
<div class="row">
	<div class="col-md-3"></div>
	<div class="col-md-6">

<!-- Bring in the instance variables from our controller to display on our index page. -->
<!-- Check to make sure @location is not empty or nil and that the API did not return an error -->

		<% if @location != nil && @location != "" && (@error == "" || @error == nil) %>
		<div class="card">
			<div class="card-header bg-warning">
				<h5>Forecast for <%= @location %></h5>
			</div>
			<div class="card-body">
				<div class="col-sm-7">
					<p>The current temperature is <%= @temp %>&deg;F</p>
					<p>Cloud cover: <%= @cloudiness%>%</p>
					<p>Wind: <%= @windiness %> mph</p>
				</div>
				<div class="col-sm-5">
				 	<p>
	          <%= @weather_words%>
	          <img src="http://openweathermap.org/img/w/<%= @weather_icon%>.png">
	        </p>
	      </div>
			</div>
		</div>
		<% elsif @status == "404" %>
			<p>Error: <%= @error %>. Please try again.</p>
		<% else %>
		<% end %>
	</div>
</div>
```

Because we're using a form, we'll need to adjust our routes.

```ruby
# routes.rb
Rails.application.routes.draw do
  # the index page gets three routes:
  root 'welcome#index'
  get 'index' => 'welcome#index'
  # "get" routes for when we initially come to the page
  # (whether typing in manually, or coming to as the root)
  post 'index' => 'welcome#index'  
  # a "post route for when we come to the page"
  # after submitting the form

  # hey, while we're here, wanna change the 'test' route?
  get 'test' => 'welcome#test'
end
```

We need to build the action for the index view, using the params from the form in our API call:

```ruby
# welcome_controller.rb

def index
    # checks that the state and city params are not empty before calling the API
    if params[:zipcode] != '' && !params[:zipcode].nil?
      response = HTTParty.get("http://api.openweathermap.org/data/2.5/weather?zip=#{params[:zipcode]}&units=imperial&APPID=#{ENV['openweather_api_key']}")
      @status = response['cod']
      # if no error is returned from the call, we fill our instance variables with the result of the call
      if @status != '404' && response['message'] != 'city not found'
        @location = response['name']
        @temp = response['main']['temp']
        @weather_icon = response['weather'][0]['icon']
        @weather_words = response['weather'][0]['description']
        @cloudiness = response['clouds']['all']
        @windiness = response['wind']['speed']
      else
        # if there is an error, we report it to our user via the @error variable
        @error = response['message']
      end
    end
end
```

And now it's time to give the Index page a try!

*Instructor Note:
Try converging all the instance variables into one instance variable storing a Hash (as suggested above with the Test page). Remember to change the View, as well.*


### Rails Challenge
*Instructor Note: In-class activity if time allows, homework if not enough time*

Why not create a database table to save previous city look-ups?

Heads up: cities with two or more words in their name will need underscores instead of spaces in the API call.

```sh
$ rails g model Location city:string state:string
$ rails db:migrate
```

```ruby
# welcome_controller
# within the index action
# after the states array, add:

# check to see if entered city & state
# already exist as a saved Location
location_exists = false
Location.all.each do |location|
	if location.city == params[:city] && location.state == params[:state]
		location_exists = true
	end
end

# if they don't: create that Location!
if params[:city] != nil && params[:state] != nil && location_exists == false 	# !location_exists
	Location.create(city: params[:city], state: params[:state])
end

# collect all Locations in an Array
@locations = Location.order(:city)
```

```html

<!-- views/welcome/index.html.erb -->
<!-- add this code between the form area
			and the results area -->

<div class="row">
	<div class="col-md-12">
		<ul class="nav nav-pills">
			<% @locations.each do |location| %>
				<li role="presentation">
					<%= link_to "#{location.city}, #{location.state}", root_path(city: location.city, state: location.state) %>
				</li>
			<% end %>
		</ul>
	</div>
</div>
```
