

# MARTA API

## Accessing a Transit API

MARTA (Atlanta Metro Transit) has a free API. Even if we're not all in Atlanta, it's an easy API to use, and requires no key, so it's great for beginners.

Useful Resources:
[MARTA Developer Resources](http://www.itsmarta.com/app-developer-resources.aspx)
[Google Maps API](https://developers.google.com/maps/web/)
[Geocoder Gem](https://github.com/alexreisner/geocoder)
[Figaro Gem](https://github.com/laserlemon/figaro)

---

1. Homework Review / Final Project Check-In
2. API Review
3. MARTA API
* Real-Time Bus Tracker
4. MARTA App
* Front End
* Back End
5. Adding Google Maps 

---

### Quick Notes

Newer Rails no longer carries the JQuery Dependency, for users running rails 5.1 or newer they will need to add the gem 'jquery-rails' into their gem file. They will also need to update the application.js file to add:

//= require jquery & //= require jquery_ujs

Without the additon of these files you will recieve errors in the console pertaining to jquery 



— Form Genration for rails 5.1 has changed from <%= form_with(model: @location, local: true) do |f| %> to <%= form_with(model: @location, local: true) do |**Form**| %>. To get f.text or f.field …etc to work properly you will have to change |form| to |f|



### API Review

**Application Programming Interface**

Allows an application like yours to make HTTP requests to another application that lives somewhere on the web!

Our application will make a request to a URL. The server will parse the URL and determine what data was requested. Then that server will return the requested data in the JSON format. Then we convert JSON to Ruby!

APIs are generally structured around a well-defined Client-Server relationship:

| Client                       | Server                                   |
| ---------------------------- | ---------------------------------------- |
| Sends a request              | Receives & parses the request            |
| Waits for the response       | Retrieves data from its sources          |
| Parses & relays the response | Compiles data & sends a response to the client |


### MARTA API
The API we will look at today pulls data from the Metro Atlanta Rapid Transit Authority (MARTA).

Why Atlanta's transit system API?
It was TTS' first campus, and it's a remarkably easy API to access and read.

See this list for other available transit system APIs:
[http://www.gtfs-data-exchange.com/agencies/bylocation](http://www.gtfs-data-exchange.com/agencies/bylocation)

The [MARTA Bus Real-time Data RESTful Web Service](http://www.itsmarta.com/app-developer-resources.aspx) provides information about the real-time location and schedule adherence for active buses on the MARTA system. Opened to developers in October 2012, the feed works in conjunction with schedule data provided in MARTA's GTFS feed.

If you visit MARTA Bus Real-time Data RESTful Web Service
you'll notice the two bottom tabs under App Developer Resources. We're going to work with the Bus RESTful API - which requires no key! To use the Rail RESTful API, you do have to apply for a key (sent via e-mail).

Through the API we are allowed access to data concerning all buses currently on the road wherever the MARTA (bus) system runs.

This is a sample of the data we are pulling in:
```javascript
{"ADHERENCE":"0","BLOCKID":"89","BLOCK_ABBR":"12-3","DIRECTION":"Southbound","LATITUDE":"33.7907773","LONGITUDE":"-84.4432624","MSGTIME":"4\/10\/2017 7:46:47 PM","ROUTE":"12","STOPID":"904800","TIMEPOINT":"Midtown Station","TRIPID":"5444050","VEHICLE":"1465"}
```

Look at all the data we get to work with:
	* ADHERENCE: how late/early the bus is (in minutes)
	* DIRECTION: which cardinal direction is the bus headed
	* LATITUDE/LONGITUDE: exact coordinates of the bus!
	* ROUTE: the route number, obvi.
	* TIMEPOINT: the bus' next stop.
	* VEHICLE: the ID number of that particular bus.

We can create a Rails app that will take an address (or approximate) from the user, and let them know if there is a bus anywhere nearby.

We'll be using the [Geocoder](https://github.com/alexreisner/geocoder) gem to pinpoint the User's longitude/latitude.


### MARTA App
Let's create a new project, with one resource: Location

```sh
$ rails new marta_near_me

$ cd marta_near_me

$ rails g scaffold Location address:string city:string latitude:float longitude:float

$ rails db:migrate
```

We'll be using the Geocoder gem, which requires latitude and longitude attributes, both as floats.

Let's make this app all about the New and Show pages. Locations/New will be our root page.

```ruby
routes.rb
Rails.application.routes.draw do

  root 'locations#new'

  resources :locations

  # For details on the DSL available within this file, 
  # see http://guides.rubyonrails.org/routing.html
end
```

On to the views...

```html
<!-- views/locations/new.html.erb -->
<h1>Is a Bus Nearby?</h1>

<p>
  Give us your address or approximate location
  (<em>I'm at the corner of...</em>), and we'll
  let you know if there's a bus in vicinity!
</p>

<%= render 'form' %>
```

```html
<!-- views/locations/_form.html.erb -->
<!-- lines 1 & 2 need to be updated to @location -->
<!-- lines 3-12 remain the same;
	but in the fields area we'll
	be more specific on our labels
	for the address field, and we'll
	delete the fields for latitude
	and longitude - Geocoder will
	take care of those for us!
-->

  <div class="field">
  	<%= f.text_field :address, 
  	  placeholder: "Address or Approx. Location" %>
  </div>

  <div class="field">
  	<%= f.text_field :city, placeholder: "City" %>
  </div>

  <div class="actions">
  	<%= f.submit %>
  </div>
<% end %>
```

If there are buses nearby, we'll have them saved in an array and display their vital info for the User.

```html
<!-- views/locations/show.html.erb -->
<p id="notice"><%= notice %></p>

<h2>You Are Currently Standing At...</h2>
<p>
  <%= @location.my_location %>
</p>

<h2>The Closest Buses Are...</h2>

<% if @bus_count == 0 %>
  <p>
    ...not really that close. Better order an Uber. Or Lyft. 
    Or call a friend. Or just give up and go back inside and 
    watch some TV. Going places is overrated.
    </p>
<% end %>

<% @nearby_buses.each do |bus| %>
  <p>
    <strong>Route:</strong> <%= bus["ROUTE"] %><br />
    <strong>Vehicle:</strong> <%= bus["VEHICLE"] %><br />
    <strong>Next Stop:</strong> <%= bus["TIMEPOINT"] %>
  </p>
<% end %>

<%= link_to "Actually, I'm at...", edit_location_path(@location) %>
```

We've got the Front End complete (maybe it could use some styling). Let's power this app in the Back End. Our next steps will be to visit these files:
	1. Gemfile
	2. location.rb
	3. locations_controller.rb
	4. locations_helper.rb

We'll start by adding the gem to our Gemfile. Along with Geocoder, let's add the httparty gem to help us out with API data parsing...
```ruby
gem 'geocoder'
gem 'httparty'
gem 'jquery-rails' ***(required for users running rails 5.1 or newer)
```

And then run bundle in Terminal...
```sh
$ bundle install
```

Next, we'll set up Geocoder's lat/long magic in the Location model.

```ruby
# location.rb
class Location < ApplicationRecord
  geocoded_by :my_location
  after_validation :geocode

  def my_location
    "#{address}, #{city}, GA"
  end
end
```

Next we'll visit the Locations controller. We're particularly interested in the show action.

```ruby
# locations_controller.rb
def show
  @buses = HTTParty.get('http://developer.itsmarta.com/BRDRestService/RestBusRealTimeService/GetAllBus')

  @bus_count = 0

  @nearby_buses = []

  @buses.each do |bus|
    if nearby(@location.longitude, @location.latitude, 
      bus["LONGITUDE"].to_f, bus["LATITUDE"].to_f)
      @bus_count += 1
      @nearby_buses.push(bus)
    end
  end
end

Let's visit the Application Helper now (you could also do this in the Location Helper) and define the methods we used in the Controller.

​```ruby
# application_helper.rb
module ApplicationHelper

  def nearby(lng1, lat1, lng2, lat2)
    if (lng1-lng2).abs <= 0.01 && (lat1-lat2).abs <= 0.01
      return true
    else
      return false
    end
  end
	
end
```

We'll need to make sure the Locations controller is even looking at the Application Helper.

```ruby
# locations_controller.rb
class LocationsController < ApplicationController
  before_action :set_location, only: [:show, :edit, :update, :destroy]

  include ApplicationHelper
```

Test it out!


### Adding Google Maps
Let's get Figaro in there so our API keys will be kept private. And add Bootstrap while we're at it.

```ruby
# Gemfile
gem 'figaro'
gem 'bootstrap-sass'
gem 'font-awesome-rails'
```

And running bundle in Terminal...
```sh
$ bundle install
$ figaro install
```

Running "install figaro" will have created a new file called **application.yml** in your config folder. Put your Gmaps API key in there.

```yaml
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

google_api_key: yourkeyhere
```

Let's add the JS link for Gmaps in the head, If you load it in the body, maps will ocassionally throw an error stating that google is not defined in your maps. 

```html
<head>  
<!-- views/layouts/application.html.erb -->
    <!-- yeah, there's a bunch of stuff up here. -->
      <script type="text/javascript" 
    src="https://maps.googleapis.com/maps/api/js?key=<%= Figaro.env.google_api_key %>">
    </script>
  </head>
    <div class="container">
      <%= yield %>
    </div>
  </body>
</html>
```

Then we're going to put JS code into the show page to show a map with each bus result.

```html
<!-- show.html.erb -->
<% @nearby_buses.each_with_index do |bus, index| %>
  <p>
    <strong>Route:</strong> <%= bus["ROUTE"] %><br />
    <strong>Vehicle:</strong> <%= bus["VEHICLE"] %><br />
    <strong>Next Stop:</strong> <%= bus["TIMEPOINT"] %>
  </p>

<script>
  $(document).ready(function (){
    function initialize(){
      var userCoords = new google.maps.LatLng(<%= @location.latitude %>, <%= @location.longitude %>);
      var busCoords = new google.maps.LatLng(<%= bus["LATITUDE"] %>, <%= bus["LONGITUDE"] %>);

      var mapOptions = {
	center: userCoords,
	zoom: 13,
	scrollwheel: false
      };
		      
      var map = new google.maps.Map(document.getElementById('map-canvas<%= index %>'), mapOptions);

      var coords = [userCoords, busCoords];
        
      var contentString = "<img src='http://www.motorbussociety.org/conventn/01spr/MARTA%203163.jpg' style='width:50px;height:50px;' />";

      var marker = new google.maps.Marker({
        position: coords[i],
	map: map,
	animation: google.maps.Animation.DROP
      });

      marker.info = new google.maps.InfoWindow({
        content: contentString
      });

      google.maps.event.addListener(marker, 'click', function() {
        marker.info.open(map, marker);
      });
    }
  }
  google.maps.event.addDomListener(window, 'load', initialize);
  });
</script>
  <div class="bus-map" id="map-canvas<%= index %>"></div>
<% end %>
```

This is just a view of the loop through the @nearby_buses array. First thing to notice is that we changed the each loop to an each_with_index loop.

We set the center of the map as your current location, set the zoom, set the map variable, and then save a bus pic as a variable (used on next page's code).

Then we place the bus pic in the info window, set an animated marker, finish up the maps JS, and then set up the <div> where it's displayed (notice that it has a unique ID based on the 'index').

Now try it out!