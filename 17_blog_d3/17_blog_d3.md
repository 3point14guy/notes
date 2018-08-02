# Blog Project
## Part Three: More Gems, More Pages, Wrap-Up

---

1. Homework Review
2. HTML Safe
3. Pagination
 * Kaminari
4. Pages to Add
5. New Page: User's Posts
 * Action
 * Route
 * View
 * Link
6. Custom URL

---

### Intro
We've got the bulk of our app set-up at this point. Association, check. Authentication, check. Posting, commenting, signing in and out. But it still feels a little thin (index/show/new/edit...that's it?), and there's some tools we could utilize to make it better.

### HTML Safe
What if our users want to use HTML formatting when they're writing? There's a method for that!

```html
<!-- in posts/show.html.erb,
    as just one example.
-->
<p>
  <%= @post.blog_entry.html_safe %>
</p>
```

This method can be used on any String or Text datatype attribute. The blogger uses HTML tags to format their entry... .html_safe ensures that HTML tags are read:


### Pagination
For when you're not into the whole continuous scroll thing, it's time to paginate! There are several pagination gems out there, but we'll use [Kaminari](https://github.com/kaminari/kaminari) today.

```ruby
# In the Gemfile:
gem 'kaminari'
```
And then bundle install

Let's paginate our Posts index page. First, we need to tell the Post model how many entries we want on each page.

```ruby
# post.rb
class Post < ApplicationRecord
  has_many :comments
  belongs_to :user

  paginates_per 3
end
```

Next, add the page parameter to corresponding ActiveRecord call in **posts_controller.rb**:
```ruby
def index
  @posts = Post.all.page(params[:page])
end
```

Finally, on the view (index.html.erb), at the bottom of the page, call the Paginate helper:
```html
<!-- the rest of the file is up here -->
      </div>
    </div>
  </div>
<% end %>

<%= paginate @posts %>
```

Now on our index page, we just see three entires, plus this list in the bottom left!


### Pages to Add
We've improved the pages we already had, but what about adding more pages?

So far, we've only gotten new pages when we've created new controllers or scaffolds. But adding a new pages to an existing controller is not only possible...

...it's easy: just takes three steps!


### New Page: User's Posts
Let's add a page that shows only a single user's posts.

We'll start with Step 1: Create a new action in the controller

```ruby
# posts_controller.rb
class PostsController < ApplicationController
  before_action :set_post, only: [:show, :edit, :update, :destroy]

  def user_posts
    # what should go in here?
  end

  #file continues with index action
```

We'll come back to what code should be within the action.

Then it's on to Step 2: Create a route

```ruby
# routes.rb
Rails.application.routes.draw do
  devise_for :users
  resources :comments
  resources :posts

  root 'posts#index'

  get 'users_posts' => 'posts#users_posts'

  # routes file continues...
```

We've got a custom URL pointing to the controller/action pair.

And then Step 3: Create a new view

```html
<!-- app/views/posts/users_posts.html.erb -->

<h1>All Posts by <%= @user.username %></h1>

<% @user.posts.each do |post| %>
  <div class="row">
    <div class="col-md-8 col-md-offset-2">
      <div class="well">
        <h3><%= post.title %></h3>
        <p><em>posted on <%= local_time(post.created_at, "%m/%d/%Y at %l:%M%P") %></em></p>
        <p>
          <%= post.blog_entry.html_safe %>
        </p>
        <p>
          <%= link_to 'Show', post, class: "btn btn-info" %> 
          <% if current_user.id == post.user_id %>
	    <%= link_to 'Edit', edit_post_path(post), class: "btn btn-warning" %> 
	    <%= link_to 'Destroy', post, method: :delete, data: { confirm: 'Are you sure?' }, class: "btn btn-danger" %>
	  <% end %>
        </p>
      </div>
    </div>
  </div>
<% end %>
```

Make sure you're creating the view in the appropriate folder (posts), and that it is named the same as the action.

Now that we've established in the view what instance variable we're going to work with, we need to return to the action in the controller (from Step 2), and define that variable.

```ruby
# posts_controller.rb
class PostsController < ApplicationController
  before_action :set_post, only: [:show, :edit, :update, :destroy]

  def user_posts
    @user = User.find(params[:id])
  end

  #file continues with index action
```

But wait... where does params[:id] come from? Okay... so maybe there's a fourth step: how do we get to this page?!

So the fourth step of our three step process is: Create a link to the page! Let's put this link anywhere we see a user's username:

```html
<!-- views/posts/index.html.erb -->
<!-- within the @posts loop -->
<h3><%= post.title %></h3>
<p>by <em><%= link_to post.user.username, user_posts_path(id: post.user_id) %></em></p>
```

```html
<!-- views/posts/show.html.erb -->
<!-- within the @post.comments loop -->
<p>
  <strong><%= link_to comment.user.username, user_posts_path(id: comment.user_id) %> said...</strong>
</p>
```


### Custom URL
One last thing in adding our new page (which is not a required -- so not counting as Step 5) is improving our URL.

Right now looks something like this:

	localhost:3000/user_posts?id=2

What if we didn't want to give away a user's ID #, but instead have the URL display their username?

We're going to need to change a couple things...
First up, the route:

```ruby
# routes.rb
Rails.application.routes.draw do
  devise_for :users
  resources :comments
  resources :posts

  root 'posts#index'

  get '/:name' => 'posts#users_posts', as: :user_posts

  # routes file continues...
```

Our URL is now the ":name" variable (to be passed through the link), and showing up on the far right is our alias, this will let us continue using user_posts_path in our links.

Next, let's visit those links and change the variable/value pairs:

```html
<!-- views/posts/index.html.erb -->
<!-- within the @posts loop -->
<h3><%= post.title %></h3>
<p>by <em><%= link_to post.user.username, 
user_posts_path(name: post.user.username) %></em></p>
```

```html
<!-- views/posts/show.html.erb -->
<!-- within the @post.comments loop -->
<p>
  <strong><%= link_to comment.user.username, 
user_posts_path(name: comment.user.username) %> said...</strong>
</p>
```

Lastly, we'll need to change in action how we look up the user (since we have the "username" instead of the "id").

```ruby
# posts_controller.rb
class PostsController < ApplicationController
  before_action :set_post, only: [:show, :edit, :update, :destroy]

  def user_posts
    @user = User.find_by(username: params[:name])
  end

  #file continues with index action
```

And now the URL looks like this:

	localhost:3000/user-mcusername

Slight problem: in this project, we have not regulated "username" as a unique attribute of Users (i.e., multiple users can sign up with the same username). But! We will see in our next project how to accomplish this!