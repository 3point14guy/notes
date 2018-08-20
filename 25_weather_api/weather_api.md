# Weather Underground API
## Gather Weather Data

This will likely be the class' first foray into APIs (besides Stripe, which works differently). We will use Weather Underground's free API to gather weather data on cities across the globe.

Useful Resources:
[Wunderground API](https://www.wunderground.com/weather/api)
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
3. Wunderground: Sign Up & Keys
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


### Wunderground: Sign Up & Keys
Let's get you an API key!
Visit: [wunderground.com/weather/api](https://wunderground.com/weather/api)
Choose the Stratus Plan under Pricing & Developer Payment ($0), then hit "Purchase Key"...
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
gem 'bootstrap-sass'
gem 'httparty'
gem 'figaro'
gem 'jquery-rails' *** (only required if running rails 5.1 or newer)
```

**httparty** will help use parse through the data sent by the API
**figaro** will create a safe place to store our API key
And **bootstrap-sass**... well, you know what's that's for

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

wunderground_api_key: yourkeyhere
```

Why are we using this gem and creating this new file just for one line of code?
We want to keep our API keys secret. **application.yml** appears in your .gitignore file. It will not be pushed to GitHub.


### API Documentation
Before you make an API call, it's a good idea to see the data you'll be fetching, and how it's structured. So head over to the [Wunderground Docs](wunderground.com/weather/api/d/docs) and click on the "Show Response" link.

(visuals within slide deck)

This is the data that would be sent if you made an API call for San Francisco, CA. Keep scrolling down to see all the data!


### Test Action/View
Time to set-up our Test action:

```ruby
# welcome_controller.rb
class WelcomeController < ApplicationController
  
  def index
  	# nothing yet
	end
 
  def test
	  response = HTTParty.get("http://api.wunderground.com/api/#{ENV['wunderground_api_key']}/geolookup/conditions/q/AZ/Phoenix.json")
	  
	  @location = response['location']['city']
	  @temp_f = response['current_observation']['temp_f']
	  @temp_c = response['current_observation']['temp_c']
	  @weather_icon = response['current_observation']['icon_url']
	  @weather_words = response['current_observation']['weather'] 
	  @forecast_link = response['current_observation']['forecast_url']
	  @real_feel = response['current_observation']['feelslike_f']
  end
end
```

We pull a URL from the docs, then add our location, and you'll want to use the variable you set for your API key in application.yml. Note that in the above code, there is a line break that needs to be deleted (between "api/" and "#{ENV[...")

Note the two different ways we could save the data in this slide (@results Hash), and the previous (all separate instance variables).

```ruby
# welcome_controller.rb
class WelcomeController < ApplicationController
  
  def index
  end
 
  def test
    response = HTTParty.get("http://api.wunderground.com/api/
    #{ENV['wunderground_api_key']}/geolookup/conditions/q/TX/Dallas.json")
  
    @results = {}
    @results[:location] = response['location']['city']
    @results[:temp_f] = response['current_observation']['temp_f']
    @results[:temp_c] = response['current_observation']['temp_c']
    @results[:weather_icon] = response['current_observation']['icon_url']
    @results[:weather_words] = response['current_observation']['weather'] 
    @results[:forecast_link] = response['current_observation']['forecast_url']
    @results[:real_feel] = response['current_observation']['feelslike_f']
  end
end
```

So you'll need to bring that in, and then we can build test.html.erb (we'll base this on the separate instance variables version).

```html
<!-- views/welcome/test.html.erb -->
<div class = "row">
  <div class = "col-md-6 col-md-offset-3">
    <div class = "well">
      <h1>Welcome To <%= @location %>!</h1>
      <div class = "row">
        <div class = "col-md-7">
          <p><em>What's the weather like?</em></p>
          <p>
              Temperature is: <%= @temp_f %>° / <%= @temp_c %>° C 
          </p>
          <p>
              Feels like: <%= @real_feel %>° F
          </p>
        </div>    
        <div class = "col-md-5">
            <p>
            <%= @weather_words%> <%= image_tag @weather_icon%> 
            </p>
            <p>
            <%=link_to "Full Forecast", @forecast_link, target: "_blank" %>
            </p>
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

@import "bootstrap-sprockets";
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
  	<div class="well">
	    <h1>Look Up Your Local Forecast</h1>
	    <p>What's the weather like in your city?</p>
	    <div>
        <%= form_tag index_path do %>
		    	<%= text_field_tag :city, nil, placeholder: "City", class: "form-control" %><br />
		    	<%= select_tag :state, options_for_select(@states), prompt: "Please select" %><br />
			    <%= submit_tag "Check Weather", class: "btn btn-primary" %>
				<% end %>
	    </div>
		</div>
  </div>
</div>
```

Then a place to display the results...
<!-- views/welcome/index.html.erb -->
<!-- Our response from wunderground's API -->
<div class="row">
  <div class="col-md-6 col-md-offset-3">
    <div class="well">

<!-- Bring in the instance variables from our controller to display on our index page. -->
<!-- Check to make sure @location is not empty or nil and that the API did not return an error -->
		<% if @location != nil && @location != "" && (@error == "" || @error == nil) %>
	    <h1>Forecast for <%= @location %></h1>
	
	    <div class="row">
	      <div class="col-md-7">
		  		<p><em>What's the weather like?</em></p>
	        <p>
		    		Temperature is: <%= @temp_f %>° / <%= @temp_c %>° 
		  		</p>
		  		<p>
		    		Feels like: <%= @real_feel_f %>°
	    	  </p>
	      </div>
	      <div class="col-md-5">
		  		<p>
		    		<%= @weather_words %> <%= image_tag @weather_icon %>
		  		</p>
		  		<p>
		    		<%= link_to "Full Forecast", @forecast_link, target: "_blank" %>
		  		</p>
	      </div>
	     </div>
	    <% else %> 
	    	<p>Error: Please enter a valid request. <%= @error %> </p> 
			<% end %>
	  </div>
	</div>
</div>
```

Because we're using a form, we'll need to adjust our routes.

​```ruby
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

We need to build the action for the index view, using the params from the form in our API call:

​```ruby
# welcome_controller.rb
def index
  # Creates an array of states that our user can choose from on our index page
  @states = %w(HI AK CA OR WA ID UT NV AZ NM CO WY MT ND SD NB KS OK TX LA AR MO IA MN WI IL IN MI OH KY TN MS AL GA FL SC NC VA WV DE MD PA NY NJ CT RI MA VT NH ME DC).sort!

  # removes spaces from the 2-word city names and replaces the space with an underscore 
  if params[:city] != nil
  	# you may have to add this line:
  	# params[:city] = params[:city].dup
  	params[:city].gsub!(" ", "_")
  end

  #checks that the state and city params are not empty before calling the API
  if params[:state] != "" && params[:city] != "" && params[:state] != nil && params[:city] != nil
		 
		results = HTTParty.get("http://api.wunderground.com/api/#{Figaro.env.wunderground_api_key}/geolookup/conditions/q/#{params[:state]}/#{params[:city]}.json")
		  

    # if no error is returned from the call, we fill our instants variables with the result of the call
		if results['response']['error'] == nil || results['error'] == ""  	
	    @location = results['location']['city']
	    @temp_f = results['current_observation']['temp_f']
	    @temp_c = results['current_observation']['temp_c']
      @weather_icon = results['current_observation']['icon_url']
	    @weather_words = results['current_observation']['weather']
      @forecast_link = results['current_observation']['forecast_url']
	    @real_feel_f = results['current_observation']['feelslike_f']
	    @real_feel_c = results['current_observation']['feelslike_c']
	 	else
			# if there is an error, we report it to our user via the @error variable 	
	    @error = results['response']['error']['description']
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
			and the results area
-->
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