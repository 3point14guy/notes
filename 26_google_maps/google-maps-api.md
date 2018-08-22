# Google Maps API
## Adding Maps to Apps

We'll look at working with Google Map's JavaScript API and adding maps to a Rails app.

Useful Resources:
[Google Maps API](https://developers.google.com/maps/web/)
[Geocoder Gem](https://github.com/alexreisner/geocoder)
[Figaro Gem](https://github.com/laserlemon/figaro)

---

1. Homework Review / Final Project Check-In
2. API Review
3. Google Maps API
4. Start a Project
5. Add a Map
6. Turbolinks Removal
7. Adding to JavaScript
8. Add Coordinates to Destinations
9. Destination Map
10. Geocoder
11. Figaro
12. Multiple Markers

---

### Quick Notes 

Newer Rails no longer carries the JQuery Dependency, for users running rails 5.1 or newer they will need to add the gem 'jquery-rails' into their gem file. They will also need to update the application.js file to add:

//= require jquery & //= require jquery_ujs

Without the additon of these files you will recieve errors in the console pertaining to jquery 



— Form Genration for rails 5.1 has changed from <%= form_with(model: @location, local: true) do |f| %> to <%= form_with(model: @location, local: true) do |**Form**| %>. To get f.text or f.field …etc to work properly you will have to change |form| to |f|



### API Review

**Application Programming Interface**
A set of programming instructions and standards for accessing a Web-based software application.


### Google Maps API
Google Maps is simply a combination of HTML, CSS and Javascript working together. The map tiles are images that are loaded into the background.

The Maps API let's us create our own map and use Javascript to tell it how to behave.


### Start a Project
Let's create a new Rails project to try out this API!
Create a new app called "gmaps-app".
```sh
$ rails new gmaps-app
```

Scaffold a "Destination" resource with two attributes:
City & Country (strings).
```sh
$ rails g scaffold Destination city:string country:string
```

Generate a "Welcome" Controller with an "index" action.
```sh
rails g controller Welcome index
```

Set the "welcome/index" page as your root:
```ruby
# routes.rb
root 'welcome#index'
```

***For Users running rails 5.1 add this to your gem file

```ruby
gem 'jquery-rails'
```

In application.js add

//=require jquery & //= require jquery_ujs

###Add a Map

Here are the basic steps:
	1. Go to [https://developers.google.com/maps/web](https://developers.google.com/maps/web)
	2. Click the "Get a Key" button at the top
	3. Obtain an API Key
	4. Add the API Key to the script tag in the <head>
	5. Create a div for our map in the html & css
	6. Pass some coordinates to the javascript
	7. Customize our awesome map

#### Getting an API Key
	1. Have a Google account? Great! If not, create one.
	2. Visit the [Google API Console](https://code.google.com/apis/console)
	3. Click the Services link from the left-hand menu
	4. Activate the Google Maps JavaScript API v3 service
	5. Click the API Access link from the left-hand menu.

Let's go back to our developer docs, copy the script tag and plug our API key into the part that says API_KEY:
```html
<!-- application.html.erb -->
 <!-- Just before the closing body tag: -->
 <script type="text/javascript"
      src="https://maps.googleapis.com/maps/api/js?key=API_KEY">
 </script>
```

We'll need a place for our map to live so let's add a div to our welcome index page and give it an id.
```html
<!-- views/welcome/index.html.erb -->
<div id="map-canvas"></div>
```

Remember that divs kind of dumb; they don't come with any built-in properties. Let's write a style to give it an exact size. I'm actually ignoring the docs and putting this in an external stylesheet to be reusable (also it's more proper).
```css
// application.css
	#map-canvas {
	  width: 100%;
	  height: 500px;
	}
```

#### Coordinates
Coordinates are used to express latitude (south to north) and longitude (west to east) values. This data is fed to the JavaScript function.

We're going to go against the docs on this one and add the JavaScript to our **application.js** file for more extensibility. 
Since we're running a web application and not a one-page website we should write code that can be used on any page. We'll need to wrap this all in a document.ready function so the JS will run on page load.

```javascript
# application.js
$(document).ready(function (){

  function initialize() {
    var mapOptions = {
    center: { lat: -34.397, lng: 150.644 },
    zoom: 8
    };
      
    var map = new google.maps.Map(document.getElementById('map-canvas'),
              mapOptions);    
  }
    
  google.maps.event.addDomListener(window, 'load', initialize);
});
```

Okay... after all this we should be able to see a map on our index page. Fire up the server and have a look.

It's a map of Sydney though, because that's the sample Google provided for us. Let's change up the coordinates to center the map someplace else. Pick a city in your travel site, for example.

How do we find a city's coordinates? We could probably just Google it, right? Or... right clicking on Google Maps and selecting "What's here" will get you the coordinates of anywhere on Earth. Enter these numbers into your JavaScript file and see how your map changes.


###Turbolinks Removal
Do you have to refresh the page several times for the map to load? This is because of turbolinks. Turbolinks is rails gem that conflicts with our javascript. 

Funny, because its intended purpose is to load pages faster. Let's kill it.

	1. Remove the gem 'turbolinks' line from your Gemfile.
	2. Remove the **//= require turbolinks** from your **app/assets/javascripts/application.js**.
	3. Remove the two **"data-turbolinks-track" => true** hash key/value pairs from your **app/views/layouts/application.html.erb**.


### Adding to JavaScript
Let's play with the zoom levels. Low numbers correspond to larger swaths of land with lower resolution imagery. A zoom of 0 will show the entire earth. Higher numbers will narrow in on specific areas displayed in high resolution. The max zoom is 21.

Let's add this attribute to disable zoom on scroll because it's super annoying when we're trying to scroll down the page.

```javascript
var mapOptions = {
	center: { lat: -36.848738, lng: 174.752173},
	zoom: 15,
	scrollwheel: false  
}
```

We can also set the coordinates as a variable. This will make our lives easier when we get to the next topic.

```javascript
function initialize() {
  var myCoords = new google.maps.LatLng(33.784624, -84.422030);
  
  var mapOptions = {
    zoom: 15,
    center: myCoords,
    scrollwheel: false    
  }

  var map = new google.maps.Map(document.getElementById('map-canvas'), mapOptions);
}

google.maps.event.addDomListener(window, 'load', initialize);
```

We've all used markers to pinpoint a location on Google Maps. Let's drop a marker on our map in the same spot as our center coordinates.

Go back to the documentation. On the left nav select Drawing on the Map and then Simple markers.

Add a new marker instance to our initialize function:
```javascript
var marker = new google.maps.Marker({
  position: myCoords,
  map: map,
  title: 'Stronbox West'
});
```

Use the documentation and figure out how to change the marker from the universal pin to something different. Maybe the country flag? Or something silly like a dog paw print.

Hint: Do an image search and select 'icons'. Remember where to save your image and how to reference it?

We need to save our js file as js.erb and use an asset path method to reference our image. [Marker Docs](https://developers.google.com/maps/documentation/javascript/markers#icons)

```javascript
// application.js.erb
 var image = "<%= asset_path 'peru-icon.png'%>"
    var marker = new google.maps.Marker({
      position: myCoords,
      map: map,
      icon: image
  });
```

Say we want to provide some information to the user when they click the marker. Let's add 3 elements to our initialize function:
```javascript
var contentString = '<h2>Strongbox West</h2>' +
   '<p>Where all your dreams come true.</p>';

 var infowindow = new google.maps.InfoWindow({
   content: contentString
 });

 google.maps.event.addListener(marker, 'click', function() {
    infowindow.open(map,marker);
 });
```

 Let's make our pointers drop and bounce on page load! Simply add the animation attribute to our marker instance in the for loop.

 ```javascript
 var marker = new google.maps.Marker({
  position: myCoords,
  map: map,
  icon: image,
  animation: google.maps.Animation.DROP
})
 ```


### Add Coordinates to Destinations
This part should be a review. Let's add two columns to our destinations resource: **latitude** and **longitude**.

```sh
$ rails g migration AddCoordinatesToDestinations latitude:float longitude:float
$ rails db:migrate
```

Remember to add the variables to the permitted list at the bottom of the destinations controller.

```ruby
# destinations_controller.rb
def destination_params
  params.require(:destination).permit(:city, :country, :description, :latitude, :longitude)
end
```

Form input fields:
```html
<!-- views/destinations/_form.html.erb -->
  <div class="field form-group">
    <%= f.label :latitude %><br>
    <%= f.text_field :latitude, class: "form-control" %>
  </div>
   <div class="field form-group">
    <%= f.label :longitude %><br>
    <%= f.text_field :longitude, class: "form-control" %>
  </div>
```

And then add to our **Show** page:
```html
<!-- views/destinations/show.html.erb -->
<p><%= @destination.latitude %>, <%= @destination.longitude %></p>

<p><%= @destination.description %></p>
```

Go ahead and add some latitudes and longitudes to your destinations. I'll just daydream about this view...


### Destination Map
We'll need to use a different id name in our HTML and CSS as not to overwrite the map we created on our welcome index page.

```html
<!-- show.html.erb -->
<div id="destination-map"></div>
```

```css
// application.css
#map-canvas, #destination-map {
  width: 100%;
  height: 500px;
}
```

Since we're being good front-end developers and not including a script tag in our view we have to find a way to pass our destination data to the JavaScript file. Here's how:

```html
<!-- show.html.erb -->
<!-- at the bottom of the page: -->
<%= javascript_tag do %>
   var latitude = '<%= j @destination.latitude.to_s %>';
   var longitude = '<%= j @destination.longitude.to_s %>';
   var address = '<%= j @destination.city %>';
<% end %>
```

Let's copy the code from application.js and drop it in **destinations.js** (remove the .coffee extension). Then we'll replace hardcoded values with the variables we just created. You can also remove the `var image` and `icon: image` from the marker code.

```javascript
$(document).ready(function (){

	function initialize() {
    var myCoords = new google.maps.LatLng(latitude,longitude);

    var mapOptions = {
       zoom: 15,
       scrollwheel: false,   
       center: myCoords
    }

    var map = new google.maps.Map(document.getElementById('destination-map'), mapOptions);

    var marker = new google.maps.Marker({
      position: myCoords,
      map: map,
      title: address
    });
 
    var contentString = '<h2>'+ address + '</h2>';

    var infowindow = new google.maps.InfoWindow({
      content: contentString
  	});

  	google.maps.event.addListener(marker, 'click', function() {
    infowindow.open(map,marker);
  	});
	}

	google.maps.event.addDomListener(window, 'load', initialize);
});
```
You're now pulling data from the database!


### Geocoder
Typing in our own coordinates is such a bore. Thankfully there's the geocoder gem that will determine latitude and longitude for us based on address. Yay!

```ruby
# Gemfile
gem 'geocoder'
```

```ruby
# destination.rb
class Destination < ActiveRecord::Base
  geocoded_by :address
  after_validation :geocode

  def address
    "#{city}, #{country}"
  end
end
```

Take a few minutes to add or update destinations in your table. You can leave out Latitude and Longitude because they will be magically calculated for us!


### Figaro
When we push to Github we'll want to make sure our API key isn't public. So that means: it's time to add **Figaro** to our Gemfile!

```ruby
# Gemfile
gem 'figaro'
```

```sh
$ bundle install
$ figaro install
```

This creates a commented **config/application.yml** file and adds it to your .gitignore. Now we'll add our own configuration to this file. Now we'll add our API Key to the newly generated yml file and update our application layout to reference this value.

```yaml
# config/application.yml
google_api_key: AIzaSyAd2gc7vvmDVDdik3z-JAIME
```

```
<!-- views/layout/application.html.erb -->
<script src="https://maps.googleapis.com/maps/api/js?key=<%= ENV['google_api_key'] %>">
</script>
```

When you push your files to github you'll notice your application.yml file isn't in the repo. Hooray! No one can steal your API Key!

When pushing to Heroku you can use the following command to configure the API Key.

```sh
$ heroku config:set google_api_key=AIzaSyAd2gc7vi9l09s0k29
```


### Multiple Markers
OK... how about plotting many markers on the same page? It's a little tricky but totally doable.

First we create an array of arrays in our js file. The inner array contains title, coordinates & z-index items. Then we loop over all the sites and output our custom markers.

```javascript
$(document).ready(function (){

function initialize() {

  // Create an array of sites
	var sites = [
	  ['Machu Picchu', -13.163047, -72.544566, 4],
	  ['Temple of the 3 Windows', -13.163726, -72.544881, 5],
	  ['House of the Guardian to the Funerary Rock', -13.165398, -72.544624, 3],
	  ['Sacred Rock', -13.161846, -72.546104, 2],
	  ['Intihuatana', -13.163439, -72.546732, 1]
	];

	var mapOptions = {
	  zoom: 15,
	  scrollwheel: false,   
	  center: new google.maps.LatLng(-13.163081, -72.545200)
	}

  var map = new google.maps.Map(document.getElementById('map-canvas'), mapOptions);
  var image = "<%= asset_path 'peru-icon.png'%>"
	
	// Loop over all sites and display marker images
  var marker, i;

  for (i = 0; i < sites.length; i++) {  
    marker = new google.maps.Marker({
    position: new google.maps.LatLng(sites[i][1], sites[i][2]),
    map: map,
    icon: image
    });
  }
}

google.maps.event.addDomListener(window, 'load', initialize);

});
```