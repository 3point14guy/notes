# AJAX & Rails
## Server Side Updating

Learning to implement Asynchronous JavaScript and XML into a Rails app.

Useful Resources:
[W3Schools - AJAX Intro](https://www.w3schools.com/xml/ajax_intro.asp)
[MDN - AJAX Getting Started](https://developer.mozilla.org/en-US/docs/AJAX/Getting_Started)

---

1. Homework Review / Final Project Check-In
2. What is AJAX?
3. Intro App
* Making an AJAX call
4. To-Do List App
5. Creating New Task on Index Page
6. Updating Tasks on Index Page
7. Deleting Tasks on Index Page
8. App Beautification (Bootstrap)
9. Datetimepicker
10. Changing Data Type
11. Cleaning Up

---

### Quick Notes

Newer Rails no longer carries the JQuery Dependency, for users running rails 5.1 or newer they will need to add the gem 'jquery-rails' into their gem file. They will also need to update the application.js file to add:

//= require jquery & //= require jquery_ujs

Without the additon of these files you will recieve errors in the console pertaining to jquery 



### What is AJAX?

**AJAX** stands for 
	**A**synchronous 
	**J**avaScript 
	**A**nd 
	**X**ML

*ASYNCHRONOUS* means that the client can request new pieces of information from the server at *ANY TIME*. This means content on a page is updated without having to re-render the entire page, making it a “seamless” experience.


### Intro App
Let's try out AJAX in a smaller app first, and then we'll go for gold in a more complex app.

Create a new Rails project called **ajax-sample**:
```sh
$ rails new ajax-sample
$ rails g scaffold Post content:text
$ rails db:migrate
```

Create a method inside your Posts controller called "ajax":
```ruby
# posts_controller.rb
	def ajax
	end
```

Create the corresponding "ajax" view and route.
```ruby
# routes.rb
root 'posts#ajax'
```

```html
<!-- ajax.html.erb -->

<h1>Ajax Testing 1, 2, 3</h1>
```

Now let's expand the View file to include JavaScript calls on our JSON file (that one you've had in every project with a scaffold!).

```html
<!-- ajax.html.erb -->
<h1>Ajax Testing 1, 2, 3</h1>

<script type="text/javascript">
  function loadPosts(){
    $.getJSON("/posts.json", function(data){
      var html = "";
      $.each(data, function (index){
				html += "<b><i>" + data[index].content + "</i></b><br>";
			}); 
			// update our div
	  	$("#posts").html(html);
		});
  }
</script>

<div id="posts">
  <a href="javascript:loadPosts();">This is where blog posts go</a>
</div>
```

#### Explanation:
1. We created a JQuery function called “.getJSON”:
* FIRST ARGUMENT PASSED = “/posts.json”. This is the URL that we’ll be working with.
* SECOND ARGUMENT PASSED = anonymous function with the argument data
  * In this case, data is the JSON that comes back via the request.
2. The OUTPUT of this URL call is LOADED INTO DATA (i.e., JSON is loaded into data)
3. We created another JQuery function that uses the each method to loop through each of our posts. This is like a loop that we saw with Ruby.
* Here, we’re using the data variable that the JSON was stored into.
* Then, we’re pulling an attribute of the Post class with .content
4. Notice that we created a variable called html that would store all the information that we had outputted in the last function
* At first, it’s simply an empty string. However, after iterating through all of the posts in our database, it is populated by the content information
* This is achieved by calling the variable within our function:
  * html += "<b><i>" + data[index].content + "</i></b><br>";
5. Finally, we plug our variable “html” into the DIV element below using JQuery:
* We target the ID of the DIV tag with “#posts”.
* Then, we use the .html method to render our results into HTML by passing in the variable, html.

Notice that this DIV element contains a HYPERLINK reference that triggers the loadPosts(); JavaScript function that we created earlier.

Create a couple of posts at **local:3000/posts**.
Aftewards, navigate over to **localhost:3000/posts.json** to see your posts in JSON format. This is the data that is being loaded into our AJAX script!


### To-Do List App
Let's create a new app called "todo-list"
Scaffold one resource: Task
​with attributes:
	* description:string
	* deadline:datetime

****Add jquery-rails gem****

**Add some data.**
(i.e., creating some Tasks the boring old Rails way)


### Creating New Task on Index Page
Let's reconfigure our **"new"** link to load a hidden form.

```html
<!-- # index.html.erb -->

...

<%= link_to 'New Task', new_task_path, remote: true %>

...
```

Here we pass the **remote: true** option to disable the default Rails mechanism that would have otherwise navigated us to **/tasks/new**.

Next let’s set up our Task Controller and to create new tasks with AJAX.

The **before_action** filter on all_tasks creates the @tasks instance variable for us automatically. Because we no longer have any logic in our index action, we can remove it. Rails will automatically render the correct template, even without the presence of the action.

```ruby
# tasks_controller.rb
class TasksController < ApplicationController
  before_action :all_tasks, only: [:index, :create]
  before_action :set_task, only: [:edit, :update, :destroy]

  # index and show actions removed

  def new
    @task = Task.new
  end

  def create
    #@task = Task.new(task_params)
    @task  = Task.create(task_params)
  end

  private

    # NEW ACTION!
    def all_tasks
      @tasks = Task.all
    end

    def set_task
      @task = Task.find(params[:id])
    end

    def task_params
      params.require(:task).permit(:description, :deadline)
    end
end
```

Now, choose a place on the index page to hide your form by passing a style attribute with the following:

```html
<!-- index.html.erb -->
...
<div id="task-form" style="display:none;"></div>
...
```

This will hide our form when the page is initially loaded.

Next, create new.js.erb (this JavaScript file, and the others we'll soon see, can be housed within the "tasks" views folder).
```javascript
// new.js.erb
$('#task-form').html("<%= j (render 'form') %>");
$('#task-form').slideDown(350);
```

This is just an ERB template that generates JavaScript instead of the HTML we’re used to. It basically translates to: “Find the element with an id attribute of task-form and at that point render the html in the form partial.”
It takes the place of:
```html
<!-- new.html.erb -->

<%= render 'form' %>
```

Our ‘form’ is now being rendered in new.js.erb.  The remote: true option is what will execute an ajax POST request. So we need to hack out some lines, and add to the top line:

```html
<!-- _form.html.erb -->

<!-- Notice we took away that "if" statement. -->

<%= form_with(model: @task, remote: true) do |f| %>
  <div class="field">
    <%= f.label :description %><br>
    <%= f.text_field :description %>
  </div>
  <div class="field">
    <%= f.label :deadline %><br>
    <%= f.datetime_select :deadline %>
  </div>
  <div class="actions">
    <%= f.submit %>
  </div>
<% end %>
```

Next we'll render our task list on the index page.

```html
<!-- index.html.erb -->

<%= link_to 'New Task', new_task_path, remote: true %>

<div id="task-form" style="display:none;"></div>  

<h2>Tasks</h2>
<ul>
    <div id="tasks">    
        <%= render @tasks %>
    </div>
</ul>
```

And create a partial to house list items for each Task:
```html
<!-- _task.html.erb -->

<li>
  <%= task.description %>
  <%= task.deadline %>
</li>
```

And we update our task list and hide our form with create.js.erb:
```javascript
// create.js.erb
$('#tasks').html("<%= j (render @tasks) %>");
$('#task-form').slideUp(350);
```

Now we should be able to dynamically add tasks to our list!


### Updating Tasks on Index Page
Let’s revisit our Task Controller and set it up for updates:

```ruby
# tasks_controller.rb

class TasksController < ApplicationController
  before_action :all_tasks, only: [:index, :create, :update]
  before_action :set_task, only: [:edit, :update, :destroy]

  def update
     @task.update(task_params) 
  end
```

And add an edit link to the task list.

```html
<!-- _task.html.erb -->

<li>
  <%= task.description %>
  <%= task.deadline %>
  <%= link_to "Edit", edit_task_path(task), remote: true %>
</li>
```

Our edit.js.erb is the same as our new.js.erb. Our update.js.erb is the same as our create.js.erb

```javascript
// edit.js.erb
// same code as new.js.erb

$('#task-form').html("<%= j (render 'form') %>");
$('#task-form').slideDown(350);
```

```javascript
// update.js.erb
// same code as create.js.erb

$('#tasks').html("<%= j (render @tasks) %>");
$('#task-form').slideUp(350);
```

That's it! Try it out!


### Deleting Tasks on Index Page
Finally, we set up our controller for deleting (aka destroying):

```ruby
# tasks_controller.rb

class TasksController < ApplicationController
  before_action :all_tasks, only: [:index, :create, :update, :destroy]
  before_action :set_task, only: [:edit, :update, :destroy]

  def destroy
    @task.destroy
  end
```

When adding our delete link, two additional steps are required:

Add the link on the task partial:
```html
<!-- _task.html.erb -->

<li>
  <%= task.description %>
  <%= task.deadline %>
  <%= link_to "Edit", edit_task_path(task), remote: true %>
  <%= link_to "Delete", task, remote: true, method: :delete,
        data: { confirm: 'Are you sure?' } %>
</li>
```

And create destroy.js.erb:
```javascript
// destroy.js.erb
$('#tasks').html("<%= j (render @tasks) %>");
```

You should now be able to add, update and delete tasks all without leaving the comfort of your index page!


### App Beautification (Bootstrap) and Datetimepicker
If we're getting fancy, that means it's time for Bootstrap!

Let's add another gem that helps us display a better UI for choosing dates.

```ruby
# Gemfile
gem 'bootstrap', '~>4.1.3'
gem 'font-awesome-rails'
gem 'momentjs-rails'
gem 'bootstrap4-datetime-picker-rails' 
```

```sh
$ bundle install
```

 Then restart your server to make the files available through the pipeline.

We have to change our application.css to application.scss Then add the @import lines to import Sass files.

```css
/* application.scss */

@import "bootstrap";
@import "tempusdominus-bootstrap-4.css";
@import "font-awesome";

```

Don't forget to delete the contents of your scaffolds.css file!

Now we have to require Bootstrap and Datetimepicker Javascripts in app/assets/javascripts/application.js

```javascript
// application.js

//= require jquery
//= require jquery_ujs
//= require turbolinks
//= require bootstrap-sprockets
//= require moment
//= require tempusdominus-bootstrap-4.js
//= require_tree .
```

Now that we have Bootstrap alive and kickin' let's add some structure to our site.

```html
<!-- application.html.erb -->

<body>
    <div class="container">
        <%= yield %>
    </div>
</body>
```


Let's add a partial that applies the datetimepicker method provided by the gem. We'll target an id called "datetimepicker1" (this partial will be created within the **tasks** views folder)

```javascript
// _datetimepicker.js

$(document).ready(function() {
  $('#datetimepicker1').datetimepicker();
});
```

We now have to render to the datepicker partial in our new and edit .js files.

```javascript
// new.js.erb and
// edit.js.erb

$('#task-form').html("<%= j (render 'form') %>");
$('#task-form').slideDown(350);

<%= render 'datetimepicker' %>
```

Now we'll add the datepicker id to our form so JavaScript knows where to work it's magic.

```html
<!-- _form.html.erb -->

<!-- <%= form_with(model: @task, remote: true) do |form| %>

  <div class="field">
    <%= form.label :description %>
    <%= form.text_field :description, id: :task_description %>
  </div> -->

  <div class="form-group">
    <div class="input-group date"  data-target-input="nearest">
    <%= form.text_field :deadline, id: "datetimepicker1", class: "form-control datetimepicker-input", "data-target" => "#datetimepicker1" %>
      <div class="input-group-append" data-target="#datetimepicker1" data-toggle="datetimepicker">
        <div class="input-group-text"><i class="fa fa-calendar"></i></div>
      </div>
    </div>
  </div>

<!--   <div class="actions">
    <%= form.submit %>
  </div>
<% end %> -->
```

Test it out... Not working?
Datetimepicker works on JavaScript, so if we're having problems with it, we need to visit the Inspect console!

The problem is this:
Datetimepicker requires being within Bootstrap's grid system. We already have "container" set up. We need to put our code within row and column divs, too.

Let's grid-up all of index.html.erb:
```html
<!-- index.html.erb -->
<div class="row">
  <div class="col-md-2">
    <%= link_to 'New Task', new_task_path, remote: true %>
    <div id="task-form" style="display:none;"></div>
  </div>
  <div class="col-md-1">
  <div class="col-md-6">
    <h2>Tasks</h2>
    <ul>
      <div id="tasks">
        <%= render @tasks %>
      </div>
    </ul>
  </div>
</div>
```

Working now? Well...sort of...

#### BUG ALERT!
Datetimepicker is giving us the wrong date format! It's giving us the day as the month, and the month as the day - so we can't even pick a day higher than 12 (or it's passing nothing at all!).

We need to change the data type of "deadline" to String, then Datetimepicker will place nice.


### Changing Data Type
Let's create a new migration in the Terminal:

```sh
$ rails g migration ChangeDataTypeForDeadline
```

Let's go find that in our db/migrate folder, and modify the code within:

```ruby
class ChangeDataTypeForDeadline < ActiveRecord::Migration
    def self.up
    change_table :tasks do |t|
      t.change :deadline, :string
    end
  end
  def self.down
    change_table :tasks do |t|
      t.change :deadline, :datetime
    end
  end
end
```

Then you need to
```sh
$ rails db:migrate
```

And voilà! Our "deadline" will now be saved as a string,
and more importantly: it will be formatted correctly. However, you may need to go and either update or just delete older tasks.


### Cleaning Up
Now we can focus back on Bootstrap'ing our various pages. We'll start with the form partial:

```html
<!-- _form.html.erb -->
<!-- <%= form_with(model: @task, remote: true) do |form| %> -->
  
  <div class="form-group">
    <%= form.text_field :description, placeholder: "Task description", class: "form-control" %>
  </div>

<!--   <div class="form-group">
    <div class="input-group date"  data-target-input="nearest">
    <%= form.text_field :deadline, id: "datetimepicker1", class: "form-control datetimepicker-input", "data-target" => "#datetimepicker1" %>
      <div class="input-group-append" data-target="#datetimepicker1" data-toggle="datetimepicker">
        <div class="input-group-text"><i class="fa fa-calendar"></i></div>
      </div>
    </div>
  </div> -->

  <div class="actions form-group">
    <%= form.submit class: "btn btn-light" %>
  </div>
<% end %>
```


```html
<!-- index.html.erb -->
<div class="row">
	<div class="col-md-3">
		<%= link_to new_task_path, remote: true do %>
    	<button class="btn btn-light"><i class="fa fa-plus"></i> Add Task </button>
		<% end %>

		<div id="task-form" style="display:none;"></div> 
	</div>

	<div class="col-md-8"> 
		<h2>JP's Tasks</h2>
		<div id="tasks">
			<div class="row">
  			<%= render @tasks %>
			</div>
		</div>
	</div>

</div>
```

Next it's on to the Task list (the partial):

```html
<!-- _task.html.erb -->

<div class="col-md-12">
  <div class="well">
    <h3><%= task.description %></h3>
    <p><%= task.deadline %></p>
    <p class="pull-right">
      <%= link_to "Edit", edit_task_path(task), remote: true %>	  
      <%= link_to "Delete", task, remote: true, method: :delete, data: { confirm: 'Are you sure?' } %>
    </p>
  </div>
</div>
```

Here's some optional styling:
```css
// application.scss

body {
  background: rgb(197, 221, 224);
}

h2 {
  color: lightslategrey;
  font-weight:bold;
  text-transform: uppercase;
}

#task-form {
  margin:10px 0;
  background: lightslategrey;
  padding: 10px 10px 5px;
}
```

#### Fancy Deleting
What if we wanted to delete an item simply by clicking on it?

We need some styling:

```css
// application.scss

a {
  text-decoration: none;
}

a:hover {
  background: none;
  text-decoration:line-through;
  color: #336699;
}
```

And then transform the description into a destroy link (and take away the "destroy" from the bottom).

```html
<!-- _task.html.erb -->
<div class="col-md-12">
  <div class="card">
    <div card-body>
      <h5 class="card-title"><%= link_to task.description, task, method: :delete, remote: true, data: { confirm: 'Are you sure?' } %></h5>
      <p class="card-text"><%= task.deadline %></p>
      <p class="float-right">
	<%= link_to "Edit", edit_task_path(task), remote: true, class: "btn btn-sm btn-light" %>
      </p>
    </div>
  </div>
</div>
```
