# Twitter Project
## Part Three: jQuery Review + Wrap-Up

---

1. Homework Review
2. jQuery Review
	* Following vs. Unfollow
3. More Pages!
	* All Users
	* Followers/Following
	* Partials
4. Tweet Timestamp

---

### jQuery Review: Following vs. Unfollow
One idea to implement some jQuery:
Currently we have a red "Unfollow" button on a user profile if our current user is already following them.

Isn't the button just so negative?
...the "danger"-style button, the word "unfollow"...

And before you say, "Well, just change the color of the button to green. Green is happy!" Here's a better idea:

	If you're following that user, there's a blue button that says so, but when you hover over it:

Let's use some jQuery to get it done!

Remember, we installed a jQuery gem - so there's no need to use the link we did when we were working in a purely Front End environment. (Some of our Bootstrap wouldn't be working if that wasn't the case.)

First, change the button's resting state from "Unfollow"/"btn-danger" to "Following"/"btn-primary", and give it an ID for jQuery to identify it ("unfollow-btn").

```html
<!-- views/epicenter/show_user.html.erb -->
<p>
  <% if current_user.following.include?(@user.id) %>
  	<!-- This line here: -->
    <%= link_to "Following", unfollow_path(id: @user.id), 
        class: "btn btn-primary", id: "unfollow-btn" %>
  <% else %>
    <% if current_user.id != @user.id %>
      <%= link_to "Follow", now_following_path(id: @user.id), 
          class: "btn btn-primary" %>
    <% end %>
  <% end %>
</p>
```

Then we can add some jQuery to change the button's wording and color when we hover over it. This can be placed in application.js or epicenter.js (changed from .coffee).

```javascript
$(document).on('turbolinks:load', function(){
  $('#unfollow_btn').hover(function(){
  	// when we hover over
    $(this).removeClass('btn-primary');
    $(this).addClass('btn-danger');
    $(this).html("Unfollow");
  }, function(){
  	// when we hover off
    $(this).html("Following");
    $(this).removeClass('btn-danger');
    $(this).addClass('btn-primary');
  });
})
```

	* When the cursor is not hovering over the button
		* Blue "Following" button
	* When the cursor is hovering over the button
		* Red "Unfollow" button


### More Pages: All Users, Followers, and Following
How do you decide who to follow? There's got to be an easier way than just looking at all the Tweets and then on to a Show User page!
 
What if we had a page that cataloged all the Users, with a little bit of info on them, and then the Follow/Following/Unfollow button?

If you remember from the Blog Project, when we add a new page we need (basically) three things: Route, Action, and View
 
Let's start with the Route:

```ruby
# routes.rb
Rails.application.routes.draw do
  
  root 'epicenter#feed'

  get 'all_users' => 'epicenter#all_users'

  # more routes down here...
```

Next we'll create the Action: we've already decided by the way we defined the route that the action will be in the Epicenter controller.

```ruby
# epicenter_controller.rb
	def all_users
    @users = User.all
    # or:
    # User.order(:username)
    # User.order(:name)
    # or whatever order you'd
    # like to put them in
  end
end

Then it's time to mock up a View page:

```html
<!-- views/epicenter/all_users.html.erb -->
<div class="row">
  <% @users.each do |user| %>
    <div class="col-md-3">
      <div class="well user-list-wells">
        <div class="row">
          <div class="col-md-6">
            <%= image_tag user.avatar.url, class: "user-pic-md" %>
          </div>
          <div class="col-md-6">
            <p>
              <% if current_user.following.include?(user.id) %>
                <%= link_to "Following", unfollow_path(id: user.id), class: "btn btn-primary", id: "unfollow_btn" %>
              <% else %>
                <% if current_user.id != user.id %>
                  <%= link_to "Follow", now_following_path(id: user.id), class: "btn btn-primary" %>
                <% end %>
              <% end %>
            </p>
          </div>
        </div>
        <div class="row">
          <%= link_to show_user_path(name: user.name) do %>
            <h3><%= user.name %></h3>
            <p>@<%= user.username %></p>
          <% end %>
          <p>
            <%= user.bio %>
          </p>
        </div>
      </div>
    </div>
  <% end %>
</div>
```

Pretty standard stuff there...except take a look at the second "row" <div>:

```html
<div class="row">
  <%= link_to show_user_path(name: user.name) do %>
    <h3><%= user.name %></h3>
    <p>@<%= user.username %></p>
  <% end %>
  <p>
    <%= user.bio %>
  </p>
</div>
```

What's going on with that "link_to"?
That's how you can have multiple HTML elements within the same hyperlink, using the Ruby link_to tag!

Finally, we need a link to get to this page. The Navbar seems like a good place for that:

```html
<!-- views/layouts/application.html.erb -->
<!-- navbar begins above -->
<div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">
  <ul class="nav navbar-nav">
    <li><%= link_to "Compose New Tweet", new_tweet_path %></li>
    <li><%= link_to "All Users", all_users_path %></li>
  </ul>
<!-- navbar continues below -->
```

Two more similar pages we can build are lists of those Users whom you are following, and those who are following you.

Let's start again with the routes:
```ruby
# somewhere within routes.rb

  get 'following' => 'epicenter#following'

  get 'followers' => 'epicenter#followers'
```

Next, it's on to the controller to create the actions. We need to make these actions generic, so that we could be viewing the following or followers of any User (not just current_user).

```ruby
# epicenter_controller.rb

  def following
    @user = User.find(params[:id])
    @users = []

    User.all.each do |user|
      if @user.following.include?(user.id)
        @users.push(user)
      end
    end
  end

  def followers
    @user = User.find(params[:id])
    @users = []

    User.all.each do |user|
      if user.following.include?(@user.id)
        @users.push(user)
      end
    end
  end
```

Notice the slight difference in the actions?
Also notice that we're using @users in both actions!

Now it's time to code the View pages.
But hold up -- these views are going to be incredibly similar -- they'll even share most of the same code! The same code found in all_users.html.erb, in fact! If we have the same HTML/Ruby code used in multiple places...
...what could we use to keep things DRY?

#### PARTIALS!

We can take all the code from all_users.html.erb and make it a new partial file... **_user_list.html.erb**

```html
<!-- views/epicenter/_user_list.html.erb -->
<div class="row">
  <% @users.each do |user| %>
    <div class="col-md-3">
      <div class="well user-list-wells">
        <div class="row">
          <div class="col-md-6">
            <%= image_tag user.avatar.url, class: "user-pic-md" %>
          </div>
          <div class="col-md-6">
            <p>
              <% if current_user.following.include?(user.id) %>
                <%= link_to "Following", unfollow_path(id: user.id), class: "btn btn-primary", id: "unfollow_btn" %>
              <% else %>
                <% if current_user.id != user.id %>
                  <%= link_to "Follow", now_following_path(id: user.id), class: "btn btn-primary" %>
                <% end %>
              <% end %>
            </p>
          </div>
        </div>
        <div class="row">
          <%= link_to show_user_path(name: user.name) do %>
            <h3><%= user.name %></h3>
            <p>@<%= user.username %></p>
          <% end %>
 	<p><%= user.bio %></p><br>
          <div class="container">
            <p><%= link_to "Followers", followers_path(id: user.id), class: "btn btn-primary" %>
            <%= link_to "Following", following_path(id: user.id), class: "btn btn-primary"%></p>
	  </div>
        </div>
      </div>
    </div>
  <% end %>
</div>
```

Remember: partials need no route or action!

Then render that partial in the three other files!

```html
<!-- views/epicenter/all_users.html.erb -->
<%= render 'user_list' %>
```

```html
<!-- views/epicenter/followers.html.erb -->
<h2><%= @user.name %>'s Followers</h2>
<%= render 'user_list' %>
```

```html
<!-- views/epicenter/following.html.erb -->
<h2><%= @user.name %> is following:</h2>
<%= render 'user_list' %>
```


### Tweet Timestamp
One last thing we can do to make our app more Twitter-like, is to put a timestamp on each tweet. But notice something about the timestamps on Twitter: it's not the exact Time and/or Date that the Tweet was posted, but rather how long ago it was posted. Let's look at modeling that layout.

The first part is pretty easy:
How to say how long ago a Tweet was posted. There's a Rails method for that: time_ago_in_word()

```html
<!-- views/epicenter/feed.html.erb -->
<% @following_tweets.sort { |x,y| y <=> x }.each do |tweet| %>
  #<div class="well">
    #<p>
      <%#= image_tag tweet.user.avatar.url, class: "user-pic-nav" %> 

      <%= link_to "@#{tweet.user.username}", show_user_path(id: tweet.user_id) %> 
      • <%= time_ago_in_words(tweet.created_at) %>
    </p>
    #<p><%= tweet.message.html_safe %></p>
  #</div>
#<% end %>
```

For the bullet, you can either copy-n-paste one in, or use the ASCII code &#8226, or use the HTML character entity &code;
We also are sorting the results to be most recent first with the help of the spaceship operator.

Using time_ago_in_words() will give us a timestamps like this:
	* less than a minute
	* about 1 hour
But notice that Twitter's timestamp doesn't include "about". How can we get rid of it on ours?

It's time to return to everyone's favorite file: **config/locales/en.yml**

```yaml
en:
  hello: "Hello world"
  datetime:
    distance_in_words:
      x_minutes:
        one: "1m"
        other: "%{count}m"
      about_x_hours:
        one: "1h"
        other: "%{count}h"
  # stuff we did in the first
  # set of slides is still down here
```

We're only going to use this method for minutes and hours. We'll handle anything older than a day a little differently.

For any tweet older than 24 hours, Twitter uses the timestamp of Month Day (abbreviated like: Nov 22). time_ago_in_words() and YAML won't help us here. We have to return to our old friend **.strftime()**. But only if certain conditions apply...

Whether within Rails or not, Ruby has can tell you the current time with **Time.now**. We can use this to determine if tweet.created_at was more than 24 hours ago. We will have two Ruby tags, only one will print out:

```html
<!-- views/epicenter/feed.html.erb -->
• 
<%= time_ago_in_words(tweet.created_at) if Time.now - tweet.created_at < 86400 %> 
<%= tweet.created_at.strftime('%b%e') if Time.now - tweet.created_at > 86400 %> 
```

When subtracting Time data type objects, Ruby will return the difference in seconds. 

**24 hours = 86400 seconds**

Also notice the Ruby code: we're using the one-line If statement!

Try it out, and we should now have Twitter-like timestamps. Remember to implement this on any other page where Tweets are displayed (*views/epicenter/show_user.html.erb*, *views/tweets/index.html.erb*).
