# Action Mailers
## Sending Mails Through Rails

Using Rails' built-in email sender.

Useful Resources:
[Action Mailer Basics](http://guides.rubyonrails.org/action_mailer_basics.html)
[Mailer Models](http://api.rubyonrails.org/classes/ActionMailer/Base.html)

---

1. Homework Review / Final Project Check-In
2. What's a Mailer?
3. Generate a Mailer
4. Email Views
5. Generate Users
6. Call the Mailer
7. Config
8. Figaro
9. Preview the Email
10. Security Settings
11. Email CSS
12. Production Set-Up

---

### Quick Notes

Due to Gmail security It's recommended to setup a dummy email and set permissions to less secure apps before you start this project. If you set permissions after the first attempt it may continue to block the email. 



### What's a Mailer?

Action Mailer allows you to send emails from your application using mailer classes and views.

Mailers work very similarly to controllers. They live in app/mailers, and have associated views.

Let's set up a new project. The user story will be: When a new user signs up, they immediately receive a welcome email.

``sh
$ rails new mail-test
```


### Generate a Mailer
Let's generate a mailer using the command below:
​```sh
$ rails generate mailer UserMailer
```

This gives us two .rb files in app/mailers and user_mailer folder under views. In Rails 5, we have two .erb files in views/layouts from `rails new`.

Where a controller generates content like HTML to send back to the client, a Mailer creates a message to be delivered via email. Let's add a **welcome_email** action to our **mailers/user_mailer.rb** file:

```ruby
class UserMailer < ApplicationMailer
  default from: 'notifications@example.com'
 
  def welcome_email(user)
    @user = user
    @url  = 'http://example.com/login'
    mail(to: @user.email, subject: 'Welcome to My Awesome Site')
  end
end
```


### Email Views
Now let's actually create the email in a view. We'll create a brand new file under **app/views/user_mailer** called **"welcome_email.html.erb"** (the name must exactly match the method we wrote in the mailer file).

```html
<!-- app/views/user_mailer/welcome_email.html.erb -->
<h1>Welcome to example.com, <%= @user.name %></h1>
<p>
  You have successfully signed up to example.com, 
  your username is: <%= @user.username %>.<br>
</p>
<p>
  To login to the site, just follow this link: <%= @url %>.
</p>
<p>Thanks for joining and have a great day!</p>
```

Why don't we need the html and body tags?
(There's a layout file for emails!)

Let's also create a text-only version for our friends who use email clients that don't accept HTML emails. This file will be called **"welcome_email.text.erb"**.

```text
Welcome to example.com, <%= @user.name %>
===============================================
 
You have successfully signed up to example.com,
your username is: <%= @user.username %>.
 
To login to the site, just follow this link: <%= @url %>.
 
Thanks for joining and have a great day!
```


### Generate Users
Let's scaffold a User with name, email and username fields.

```sh
$ rails g scaffold User name email username
$ rails db:migrate
```



### Call the Mailer
Then add the following line to **users_controller.rb** so that the email will be sent out upon user creation.

```ruby
def create
  @user = User.new(user_params)

  respond_to do |format|
    if @user.save
      # Tell the UserMailer to send a welcome email after save
      UserMailer.welcome_email(@user).deliver_now
```


### Config
In order to actually send an email through gmail we need to set up our SMTP settings in **config/environments/development.rb**

```ruby
  config.action_mailer.delivery_method = :smtp
  config.action_mailer.smtp_settings = {
    address:     'smtp.gmail.com',
    port:        587,
    domain:      'gmail.com',
    user_name:   "#{ENV['gmail_un']}@gmail.com",
    password:     "#{ENV['gmail_pw']}",
    # we want to use the Figaro gem to hide our UN & PW! 
    authentication:  'plain',
    enable_starttls_auto: true
  }

# NOTE: do NOT use an email acct w/ 2-step auth enabled
```

**SMTP** = **S**imple **M**ail **T**ransfer **P**rotocol


### Figaro
Add the Figaro gem to Gemfile, install...

```ruby
gem 'figaro'
```

```sh
$ bundle install
$ figaro install
```

...then assign the variables from development.rb in application.yml

```yaml
gmail_un: xxx # just your username, no @gmail.com
gmail_pw: yyy
```


### Preview the Email
Take the steps below to preview the email:

	* Add a user to localhost:3000/users

	* Write the following action in **test/mailers/previews/user_mailer_preview.rb**

	* Browse the page at the URL specified in the comment

```ruby
# Preview all emails at http://localhost:3000/rails/mailers/user_mailer

class UserMailerPreview < ActionMailer::Preview
  def user_mail_preview
    UserMailer.welcome_email(User.first)
  end
end
```

Go ahead and browse to localhost:3000/users and add a user (you!) with your personal email address. In a few seconds you should receive an email in your inbox:

Not getting an email? You need to kill / restart your server!
Also, a preview of the html will also appear in the Terminal.


### Security Settings
You may have to change security settings on your Gmail account.
1. Click your user icon on the top right
2. Click the "My Account" button
3. Choose "Sign-in & Security"
4. Towards the bottom of the page, set
   **Allow less secure apps: "ON"**

Note:
This may not be a setting you want for your personal email, so create a new email just for using in your apps' mailers!


### Email CSS
You can give your HTML emails (sorry, text) a little more zazz. In **views/layouts/mailer.html.erb**, remove the style-tags tags in the <head> and replace with the stylesheet and JavaScript tags from **application.html.erb**.

```html
<!DOCTYPE html>
<html>
  <head>
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
    <%= stylesheet_link_tag    'application', media: 'all', 'data-turbolinks-track': 'reload' %>
    <%= javascript_include_tag 'application', 'data-turbolinks-track': 'reload' %>
  </head>

  <body>
    <%= yield %>
  </body>
</html>
```
Then you're able to use your custom CSS, or Bootstrap if it's been implemented.


### Production Set-Up
To get this working on production (Heroku, AWS, etc.), you'll have to copy the config settings in **config/development.rb** to **config/production.rb**.