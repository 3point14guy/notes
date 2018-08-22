# Pushing to Production
## Heroku + Amazon S3

Let's push a Rails app to Heroku, and then use Amazon Web Services' S3 to store user-uploaded images.

Useful Resources:
[Ruby on Heroku](https://devcenter.heroku.com/categories/ruby)
[Amazon Web Services](https://aws.amazon.com/)
[AWS S3 Docs](https://aws.amazon.com/s3/?p=tile)
[Heroku Paperclip Docs](https://devcenter.heroku.com/articles/paperclip-s3)

---

1. Homework Review / Final Project Check-In
2. Create a Project
3. Faker Gem
4. Production Gems
5. Push to GitHub
6. Push to Heroku
7. AWS S3
8. CarrierWave Implementation
9. Paperclip Implementation
10. Heroku Config Variables

---

### Create a Project
### Create a Project

1. Create a new rails app to test this out in.
2. In the Gemfile, add either the CarrierWave or Paperclip gem
3. Scaffold a User resource with a name, team_id, and avatar

```sh
$ rails new heroku-test
```

```ruby
#Gemfile

gem 'carrierwave'

# or

gem 'paperclip'
```
* if using CarrierWave: "avatar" is a string attribute, then add the AvatarUploader and mount on the User model

* if using Paperclip: "avatar" must be added after initial scaffolding using 
```sh
rails g paperclip user avatar 
```

```sh
$ rails g scaffold User name:string team_id:integer avatar:string
$ rails db:migrate
```


### Faker Gem
Let's try something new...
Using the faker gem, and seeding associated tables!

1. Generate a Team model (name, sport)
2. Enter association key phrases in the models
3. Install faker gem

[Faker](https://github.com/stympy/faker) is a handy gem for generating sample data in your application. It can generate names, email addresses, URLs, paragraphs of text, and a lot more.
```ruby
Faker::Name.name      #=> "Christophe Bartell"

Faker::Internet.email #=> "kirsten.greenholt@corkeryfisher.info"

Faker::Hacker.say_something_smart
  #=> "Try to compress the SQL interface, maybe it will program the back-end hard drive!
```

Let's use Faker to generate users and teams for our app by putting this code in db/seeds.rb and then running rails db:seed

```ruby
# db/seeds.rb
# clear out the existing teams and users
Team.destroy_all
User.destroy_all

# create 3 teams...
3.times do
  team = Team.create(
    name: Faker::Team.name,
    sport: Faker::Team.sport,
  )

  # ...with 5 users each
  5.times do
    team.users.create(
      name: Faker::Name.unique.name,
      # no need to seed team_id, it'll do it for you. MAGIC!
    )
  end
end
```

```sh
$ rails db:seed
```

### Production Gems

Heroku databases run on PostgreSQL, not SQLite. Don't worry, there won't be much noticable difference to you.

We need to remove all evidence of sqlite3 before pushing to GitHub first: 

```ruby
# Gemfile
# for our local server:
group :development do
    gem 'sqlite3'
end

# for Heroku:
group :production do
  gem 'pg'
  gem 'rails_12factor'
end
```

Comment out sqLite3 from your Gemfile: 

```ruby
#gem 'sqlite3'
```

Do a quick search in your gem file to make sure you have removed it. Next go to config/database.yml and change the following: 

```ruby
production:
  <<: *default
  database: db/production.sqlite3
```

To be this instead: 

``` ruby
production:
  <<: *default
  database: db/production.pg
```

```sh
$ bundle install
```


### Push to GitHub
No push to production is complete without a push to your code storage repo! And anyway, you're gonna have to do the whole git init...git commit part for Heroku anyway, might as well push to GitHub as well.

```sh
$ git init

$ git add .

$ git commit -m "added production gems"

$ git remote add origin ...

$ git push origin master
```


### Push to Heroku
If you've used Heroku before you're probably already logged in so you can skip right to the "heroku create" step.

```sh
$ heroku login
Enter your Heroku credentials.
Email: [your email here]
Password (typing will be hidden): [your pw here, but unseen!]
Authentication successful.

$ heroku create unique-project-name
Creating unique-project-name... done, stack is cedar-14
https://unique-project-name.herokuapp.com/ | 
https://git.heroku.com/unique-project-name.git
Git remote heroku added
```
If you don't put in a *unique-project-name*, Heroku will give one for you. You can change it later when you come up with some good.

Go ahead and push to Heroku. If your site has database dependencies you'll need to run the migrate command as well. Then test to see if your site deployed properly.

```sh
$ git push heroku master
$ heroku run rails db:migrate
$ heroku open
```

But there's no data! Don't panic. Run this:
```sh
$ heroku run rails db:seed
```

Many, many, many things can go wrong during deployment. If everything went off without a hitch: congrats! If not, here are a few other useful commands to troubleshoot.

```sh
$ heroku logs
$ heroku status
```

And there's also our good friend Google to help us find answers.


### AWS S3
If you have CarrierWave or Paperclip installed you'll have to get an account with AWS (Amazon Web Services) to host your images. Unfortunately Heroku won't host them for you.

Sign up for a FREE AWS Account (or just use your existing amazon.com account):
[Amazon Web Services](https://aws.amazon.com/)

(Paperclip solution follows after CW solution.)

Click on S3 once you're logged into AWS and create a bucket (blue button - top left). Name it something descriptive like "blog-photos." Some other guidelines: 

	* No capital letters (A-Z)
	* No periods (.)
	* No underscores (_)
	* - cannot appear at the beginning nor end of the bucket name
	* The bucket name must be less than or equal to 32 characters long
	* Be sure to create a bucket in the same region as your app

Your S3 credentials can be found on the Security Credentials section of the AWS menu. Select "Access Key" from the accordion, then "Create New Access Key". Copy your access key, secret access key and bucket name and paste someplace safe. 

We'll need them again in a few.


### CarrierWave Implementation
Add the fog gem to your gemfile. This will connect our CarrierWave uploader with S3. We'll also add figaro to hide our keys.

```ruby
# Gemfile
gem 'carrierwave'
gem 'fog'
gem 'figaro'
```

```sh
$ bundle install
$ figaro install
```

Set your variables in application.yml:
```yaml
AWS_ACCESS_KEY_ID: xxx
AWS_SECRET_ACCESS_KEY: yyy
S3_REGION: aaa
S3_BUCKET_NAME: bbb
```

Create a new file in config/initializers called carrierwave.rb
```ruby
# config/initiliazers/carrierwave.rb

CarrierWave.configure do |config|
  config.fog_credentials = {
    :provider              => 'AWS',
    :aws_access_key_id     => "#{ENV['AWS_ACCESS_KEY_ID']}",
    :aws_secret_access_key => "#{ENV['AWS_SECRET_ACCESS_KEY']}",
    :region                => "#{ENV['S3_REGION']}",
    :path_style            => true
  }

  config.fog_directory =  "#{ENV['S3_BUCKET_NAME']}"
  config.cache_dir     = "#{Rails.root}/tmp/uploads"   # For Heroku
end
```


### Paperclip Implementation
Add the aws-sdk gem to your gemfile. I had some trouble with versions above 2.0 so here I've added versions below 2.0.

```ruby
# Gemfile
group :production do
  gem 'pg'
  gem 'rails_12factor'
  gem 'aws-sdk', '~> 2.3'
end
```

```sh
$ bundle install
```

Here we'll configure our 3 environment variables. Paste this code EXACTLY. Do not add your actual key and bucket names.

```ruby
# config/environments/production.rb

config.paperclip_defaults = {
  :storage => :s3,
  :s3_protocol => 'https',
  :s3_credentials => {
    :bucket => ENV['S3_BUCKET_NAME'],
    :access_key_id => ENV['AWS_ACCESS_KEY_ID'],
    :secret_access_key => ENV['AWS_SECRET_ACCESS_KEY']
  }
}
```


### Heroku Config Variables
Now we can push our file changes to GitHub & Heroku.

```sh
$ git add .
$ git commit -m "added aws gem and configs"
$ git push origin master
$ git push heroku master
```

And finally set the vars inside Heroku directly using the command line. Paste your ACTUAL key and bucket names in here... replace xxx, yyy and abc, def.

```sh
$ heroku config:set AWS_ACCESS_KEY_ID=xxx AWS_SECRET_ACCESS_KEY=yyy

$ heroku config:set S3_BUCKET_NAME=abc S3_REGION=def
```

Or input them manually by visiting your Heroku dashboard, clicking the app, choosing the "Settings" tab, and then "Reveal Config Vars" (add more at the bottom).

You'll now see that AWS is hosting your photo on production! Use Chrome's Inspect to see the difference.

Example:

**Localhost:**
src="/system/blog_posts/photos/000/000/001/original/blogpic1.jpg?1425257686"

**Production:**
src="http://s3.amazonaws.com/blog-photos/blog_posts/photos/000/000/002/original/blogpic1.jpg?1425423241"