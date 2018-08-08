# Twitter Project
## Part Two: Hashtags and Tweet Links

---

1. Homework Review
2. Hashtags
 * Model Generation
 * Association
 * Join Table
 * Creating Tags + TweetTags
 * Using the Helper
3. Tag Tweets Page
4. Links in Tweets
 * Adding Attribute
 * Model Methods:
  * Link Check
  * Apply Link

---

### Hashtags
A major aspect of Twitter that we haven't yet talked about is Hashtags. Essentially, hashtags let users know that they're talking about the same subject as other users.

So if your Tweet has #rails4life within the message, you should be able to click on that phrase (as a link), and see every other Tweet that contains that  hashtagged-phrase.

Hashtags will not only be phrases within Tweets, they will also be a Resource unto themselves:

```sh
$ rails g model Tag phrase:string
```

We don't need the whole scaffold structure for Tags. We will create custom pages within our Epicenter controller to view all Tweets with particular Tags.

So what is the relationship between Tags and Tweets?
One Tweet can have many Tags. One Tag can exist within many Tweets.

Hmm...
I think we have ourselves a good ol' fashioned **Many-to-Many Association**. So that means we're going to need a Join Table! Once again, no need for a scaffold. And all a Join Table takes as attributes is the Foreign Keys of the two tables we want to associate.

```sh
$ rails g model TweetTag tweet_id:integer tag_id:integer
$ rails db:migrate
```

Next, we want to set up the association keywords throughout the three models:

```ruby
# tweet.rb
class Tweet < ApplicationRecord
    belongs_to :user

    has_many :tweet_tags
    has_many :tags, through: :tweet_tags

    validates :message, presence: true
    validates :message, length: {maximum: 140}
end
```

```ruby
# tags.rb
class Tag < ApplicationRecord
    has_many :tweet_tags
    has_many :tweets, through: :tweet_tags
end
```

```ruby
# tweet_tags.rb
class TweetTag < ApplicationRecord
    belongs_to :tweet
    belongs_to :tag
end
```

We've established the association between Tweets and Tags, but how are the latter created? As each Tweet is created, we'll need to comb through - word by word - to find out if a Tag (hashtagged word or phrase) is even present.

```ruby
# tweets_controller.rb
def create
  @tweet = Tweet.create(tweet_params)

  message_arr = @tweet.message.split

  message_arr.each do |word|
    if word[0] == "#"
      #create a new instance of a Tag
    end
  end

  #create action continues down here
```

But it might not be so easy...
We don't want multiple instances of the same Tag. So we'll need to check to see if the Tag already exists before creating a new one.

```ruby
# tweets_controller.rb
def create
  @tweet = Tweet.create(tweet_params)

  message_arr = @tweet.message.split

  message_arr.each do |word|
    if word[0] == "#"
      if Tag.pluck(:phrase).include?(word.downcase)
        #save that Tag as a variable (to use in TweetTag creation)
      else
        #create a new instance of Tag
      end
    end
  end

  #create action continues down here
```

**.pluck()** is an ActiveRecord call that will return an array populated with the values of the attribute you specify.

Let's fill in the actual Active Record call for those comments seen in the last slide:

```ruby
# tweets_controller.rb
def create
  @tweet = Tweet.create(tweet_params)

  message_arr = @tweet.message.split

  message_arr.each do |word|
    if word[0] == "#"
      if Tag.pluck(:phrase).include?(word.downcase)
      	# if the Tag already exists, look it up:
        tag = Tag.find_by(phrase: word.downcase)
      else
      	# if the Tag doesn't exist, create it:
        tag = Tag.create(phrase: word.downcase)
      end
    end
  end

  #create action continues down here
```

Next we'll create a TweetTag, to facilitate the association between the Tag and Tweet.

```ruby
# tweets_controller.rb
def create
  @tweet = Tweet.create(tweet_params)

  message_arr = @tweet.message.split

  message_arr.each do |word|
    if word[0] == "#"
      if Tag.pluck(:phrase).include?(word.downcase)
        tag = Tag.find_by(phrase: word.downcase)
      else
        tag = Tag.create(phrase: word.downcase)
      end
      # Create the TweetTag
      tweet_tag = TweetTag.create(tweet_id: @tweet.id, tag_id: tag.id)
    end
  end

  #create action continues down here
```

Within the Tweet, the tagged word should serve as a link to a page that displays all Tweets with that Tag. We're going to need to modify our each loop to include an index to accomplish this:

```ruby
# tweets_controller.rb
def create
  @tweet = Tweet.create(tweet_params)

  message_arr = @tweet.message.split

  # .each with index iterator:
  message_arr.each_with_index do |word, index|
    if word[0] == "#"
      if Tag.pluck(:phrase).include?(word.downcase)
        tag = Tag.find_by(phrase: word.downcase)
      else
        tag = Tag.create(phrase: word.downcase)
      end
      tweet_tag = TweetTag.create(tweet_id: @tweet.id, tag_id: tag.id)
      # at the appropriate index, we will
      # replace the tagged phrase with
      # a link-ified version of the phrase:
      message_arr[index] = "<a href='/tag_tweets?id=#{tag.id}'>#{word}</a>"
    end
  end

  #create action continues down here
```

One last thing - we need to rejoin the message_arr as a string, and update the Tweet with this modified string as the message:

```ruby
# tweets_controller.rb
def create
  @tweet = Tweet.create(tweet_params)

  message_arr = @tweet.message.split

  message_arr.each_with_index do |word, index|
    if word[0] == "#"
      if Tag.pluck(:phrase).include?(word.downcase)
        tag = Tag.find_by(phrase: word.downcase)
      else
        tag = Tag.create(phrase: word.downcase)
      end
      tweet_tag = TweetTag.create(tweet_id: @tweet.id, tag_id: tag.id)
      message_arr[index] = "<a href='/tag_tweets?id=#{tag.id}'>#{word}</a>"
    end
  end

  # rejoin the message Array as a String
  @tweet.update(message: message_arr.join(" "))

  #create action continues down here
```

The "create" action is really packed now. Look at it:
```ruby
# tweets_controller.rb
def create
  @tweet = Tweet.create(tweet_params)

  message_arr = @tweet.message.split

  message_arr.each_with_index do |word, index|
    if word[0] == "#"
      if Tag.pluck(:phrase).include?(word.downcase)
        tag = Tag.find_by(phrase: word.downcase)
      else
        tag = Tag.create(phrase: word.downcase)
      end
      tweet_tag = TweetTag.create(tweet_id: @tweet.id, tag_id: tag.id)
      message_arr[index] = "<a href='/tag_tweets?id=#{tag.id}'>#{word}</a>"
    end
  end

  @tweet.update(message: message_arr.join(" "))

  respond_to do |format|
    if @tweet.save
      format.html { redirect_to root_path }
    else
      format.html { render :new }
      format.json { render json: @tweet.errors, status: :unprocessable_entity }
    end
  end
end
```

This is a situation that calls for the Helper!
Let's move all that code we just wrote into a method in the TweetsHelper:

```ruby
# tweets_helper.rb
module TweetsHelper
	def get_tagged(tweet)
		message_arr = tweet.message.split
		message_arr.each_with_index do |word, index|
			if word[0] == "#"
				if Tag.pluck(:phrase).include?(word.downcase)
				  tag = Tag.find_by(phrase: word.downcase)
				else
				  tag = Tag.create(phrase: word.downcase)
				end
				tweet_tag = TweetTag.create(tweet_id: tweet.id, tag_id: tag.id)
				message_arr[index] = "<a href='/tag_tweets?id=#{tag.id}'>#{word}</a>"
			end
		end

		tweet.message = message_arr.join(" ")
		return tweet
	end
end
```

A couple notes on changes made from moving over from the Controller:
	* Instead of looking for a tag in @tweet, it is simply "tweet".
	* At the end we "return tweet"

We can now redefine @tweet in the create action, by passing it through that new helper method.

```ruby
# tweets_controller.rb

def create
  @tweet = Tweet.create(tweet_params)

  @tweet = get_tagged(@tweet)

  #rest of create action continues down here...
```

However, we do need to make sure the helper is included at the top of the controller:

```ruby
# tweets_controller.rb
class TweetsController < ApplicationController
  before_action :set_tweet, only: [:show, :edit, :update, :destroy]

  before_action :authenticate_user!

  # This line is added:
  include TweetsHelper
  # to "include" all the code from 
  # the tweets_helper.rb file!

  # controller continues below...
```


### Tag Tweets Page
Before we test this out, we are expecting a link to a route we haven't defined yet, so unless we want an error, we better get to that...

```ruby
# routes.rb
get 'tag_tweets' => 'epicenter#tag_tweets'
```

And create an action:
```ruby
# epicenter_controller.rb
def tag_tweets
    @tag = Tag.find(params[:id])
end
```

And create a view:
```html
<!-- views/epicenter/tag_tweets.html.erb -->
<div>
    <h1><%= @tag.phrase %></h1>
</div>

<div class="row">
  <!-- a place for current user stats -->
  <div>
    <% @tag.tweets.order(created_at: :desc).each do |tweet| %>
      <p> 
	     <%= tweet.user.name %> <%= "@#{tweet.user.username}" %>
	    • <%= time_ago_in_words(tweet.created_at) %>
      </p>
			<p>
			    <%= tweet.message.html_safe %>
			</p>
    <% end %>
   </div>
</div>
```


### Links in Tweets
Twitter has a policy that if you put a URL in your Tweet, they will only charge 23 characters, no matter the length of the actual URL. However, even if your URL is less than 23 characters, you still get charged for 23 characters.

Not cool, Twitter.

So let's be better than Twitter, and give Tweets a break when it comes to long URLs, but leave alone URLs shorter than 23 characters.

First, let's address a problem of shortening a URL: we need the whole URL to make a hyperlink work. So we can shorten the URL in the message, but we'll need to save the full URL for use later. Let's create a new attribute in which to save that full URL:

```sh
$ rails g migration AddLinkToTweets link:string
...
$ rails db:migrate
```

And don't forget to add to our permitted params:

```ruby
# at the bottom of tweets_controller.rb:
  def tweet_params
    params.require(:tweet).permit(:message, :user_id, :link)
  end
```

Next, we need to have a way to see if there's even a URL in a new (or edited) message. We can create a method in the model to check for that!

```ruby
# tweet.rb
class Tweet < ApplicationRecord

  # there's code up in here

  def link_check
    if self.message.include? "http://"
      arr = self.message.split
      
      # .map is another Array iterator (as see ".collect")
      # it will return in an new Array the outcome
      # of our code within the curly brackets,
      # which in this case is true or false
      index = arr.map { |x| x.include? "http://" }.index(true)
      # we then find which index has the value of "true"
      # this will be the index in "arr" that has the URL

      self.link = arr[index]

      if arr[index].length > 23
        arr[index] = "#{arr[index][0..20]}..."
      end

      self.message = arr.join(" ")
    end
  end
end

Link Check Step-by-Step:
	* We check to see if the message even includes a URL (starts w/ "http://")
	* We split the message into an Array
	* Using .map & .index we can find exactly where that URL is within the message
	* Then save that URL as our link attribute
	* Now we perform the URL shortening, but only if it's longer than 23 characters
	* Finally, we rejoin the message as a String

We need to do all this before we run the attribute through validation. Similar to the *before_filter* and *before_action* calls we have made in the models and controllers, we can use *before_validation* to call a method:

​```ruby
# tweet.rb
class Tweet < ApplicationRecord

  before_validation :link_check

  validates :message, length: { maximum: 140 }

    
  def link_check
      # contents hidden for this slide
  end
end
```

One last thing we need to consider: should this method be able to be called anywhere else (via Tweet.link_check)? In this case, the answer is probably: No!

So we can house the method in a "private" (or "protected") section, to keep it only usable in this file

```ruby
# tweet.rb
class Tweet < ApplicationRecord

  before_validation :link_check

  validates :message, length: { maximum: 140 }

  private

    def link_check
      # contents hidden for this slide
    end

  # no "end" needed
end
```

We can test this functionality out, but we'll find something that's gone un-addressed: how do we make the URL in the message an actual hyperlink?

This is the reason we saved the full link: so we could access it when we were left with a shortened (i.e., incomplete) version within the message. Now we need to apply that full URL to whatever is left in the message.

First, we need to identify the URL in the message... Hey! We just did that in link_check!

Let's build a new method, still in the private area of the model:

```ruby
# tweet.rb
	def apply_link
	  arr = self.message.split
	  index = arr.map { |x| x.include? "http://" }.index(true)
	  url = arr[index]

	end
```

Here we repeat the code from the beginning of link_check, but we save arr[index] is a new variable: **url**

Next, we'll use that url and self.link to build an anchor tag and replace arr[index] with this

```ruby
# tweet.rb
def apply_link
	arr = self.message.split
	index = arr.map { |x| x.include? "http://" }.index(true)
	url = arr[index]

	arr[index] = "<a href='#{self.link}' target='_blank'>#{url}</a>"

	self.message = arr.join(" ")

end
```

We are not building a Ruby link_to tag because it will not save correctly in a String. But HTML tags will save as text, and then we can use .html_safe on display.

Remember to mark anyplace that the message is being displayed with .html_safe

```html
<%= tweet.message.html_safe %>

<!-- or -->

<%= @tweet.message.html_safe %>

<!-- depending on which view you're on -->
```

Now it's time to try it out!


**Note:**
You will probably find some URLs not coorperating with your code. This likely because they are "https" sites.

We can go back and add OR (||) conditions to places in the code where we're just to see if "http://" is included or exists.

Example:
```ruby
index = arr.map { |x| x.include?("http://") || x.include?)"https://") }.index(true)
```

Also notice that you will likely have to start using *parentheses* with .include? if you're going to add an OR.