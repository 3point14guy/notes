# Bootstrap
## Pre-Built CSS & JS Framework
Let's take a look at using Bootstrap's library of CSS and JavaScript classes and id's. Though it's good to have thorough Front End skills under your belt, if you need to get an app out to production quick, Bootstrap can help cut down the time you may spend on UI/UX.

## Useful Resources
[Bootstrap's Website](http://getbootstrap.com/)
[Font Awesome (Bootstrap-based Icons)](http://fontawesome.io/)

---

1. Concept Review
	* Basic HTML Structure
	* Divs and Spans
	* Box Model
2. What is Bootstrap?
3. Create a New Project
	* New File
	* Bootstrap Basic Template
	* Bootstrap CDN links
4. Jumbotron
5. Grid System
6. Panels
7. Wells
8. Buttons
9. Footer
10. Glyphicons
11. Images
12. Navbar
13. Media Object
14. Divs and the Grid System (again!)
15. Overriding Bootstrap

---

## Concept Review
***slide 1***

Basic layout of an HTML page:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Divs and Spans</title>
    <link rel="stylesheet" href="assets/stylesheets/styles.css">
  </head>
  <body>
    <h1>My Awesome Site</h1>
    <p>You'll find some really cool stuff here.</p>
  </body>
</html>
```

### Review: Div and Span
***slide 2***

Divs and spans are HTML tags that are commonly used with CSS. 
* Divs are called "block" elements (for large chunks of code) and
* Spans are "inline" (for small bits of code).

```html
<div id="content">
    <h1>My Awesome Site</h1>
    <p>You'll find some really cool stuff here.</p>
</div>
```

```css
#content {
  width: 600px;
  background: #fff;
}

.crazy-color {
  color: fuchsia;
}
```

## Review: Box Model
***slide 3***

Each HTML element can be considered a box. The box model is essentially a box that wraps around HTML elements. It consists of: margins, borders, padding, and the actual content.

## Bootstrap responsive image
***slide 4***

*No content*

## What is Bootstrap?
***slide 5***

Bootstrap is an HTML, CSS & JS framework. It was built by developers at Twitter. It is mobile-ready and built with responsive design in mind.

Basically, it saves you lots of time! You can use their pre-built classes and ids to give your site a sleek look without having to write a whole lot of your own CSS and JavaScript.

#### Built w/ Bootstrap Examples:
[Timely](https://timelyapp.com/)
[Cluboid](www.cluboid.com/)
[Osmo](https://www.playosmo.com/en/)
If you think your class can handle some strong language and joking about the thing we're using:
[Every F'ing Bootstrap Site Ever](http://adventurega.me/bootstrap/)

## Create a New Folder
***slide 6***

Create a new folder in where you are collecting your Front End projects. Title it **"Bootstrap"**.

## Let's Get Started
***slide 7***

1. Inside create, a new file called **"index.html"**.
2. In the browser, go to [Bootstrap's Basic Template](http://getbootstrap.com/). 
3. Select v3.3.7
4. Click "Getting Started" at the top.
5. Copy-and-paste the template (use the **Copy** button on the top-right!) into **index.html**, and get rid of any extra stuff (comments, etc.)
6. Test it out.

## Here's What We Got
***slide 8***

*Just shows what file should look like*

## Let's Keep Going
***slide 9***

Bootstrap isn't coming through yet, but we want to see the difference between normal and Bootstrap. Next, let's bring in the Bootstrap library through CDN (**Content Delivery Network**) links:

1. Find CDN links at the top of Bootstrap's "Getting Started" page.
2. Copy the CSS link and paste over the dummy <link> tag in **index.html**
3. Copy the JS link and paste over the dummy <script> tag in **index.html**. Be careful not to delete or paste-over the <script> tag bringing in jQuery! Bootstrap is written in jQuery and needs that link!
4. Test again!

## Simple Test
***slide 10***

*Shows index.html with CDN link instead of local links*

## Use the Inspector
***slide 11***

* Hover over the "Hello, world!"
* Right-click to "Inspect"
* Notice the styles being applied
* Click the "Sources" tab
* Notice the Bootstrap folder (cloud)

## Bootstrap-ify
***slide 12***

Let's add to our **index.html** page a couple of common Bootstrap components, then we will look at them in more detail.

We're going to wrap everything in a `<div>` with the class of "jumbotron". Inside the `<div>` we'll keep the `<h1>`, add a `<p>`, and add an `<a>` tag that takes the classes of "btn" and "btn-primary" and "btn-lg".

```html
<div class="jumbotron">
	<h1>My Awesome Site</h1>
	<p>Bask in it's AWESOMENESS.</p>
	<a class="btn btn-primary btn-lg">Click this!</a>
</div>
```

Test out its responsiveness: change the size of the browser from big to small to big again.

## Grid Talk 1
***slide 13***

### Bootstrap Grid System
These are building blocks of a Bootstrap website. They provide us with responsive page layouts using rows & columns. This lets us not have to worry about floating in CSS! 

With Bootstrap, pages can be laid out in columns. Column size must add up to twelve (12). See the examples in Bootstrap's [documentation](http://getbootstrap.com/docs/3.3/css/#grid).

## Grid Talk 2
***slide 14***

Columns work within a row, and rows work within a container.
So we'll start with a `<div>` with "container" class, then a `<div>` with "row" class, then our `<div>`'s with "col-" classes.

Let's add a full-width column with Lorem Ipsum filler text.

You can either use Sublime's lorem-autofill (type in "lorem" and then press *tab*). Or choose an [Ipsum Generator](http://mashable.com/2013/07/11/lorem-ipsum/#wkre_euxBiqp) of your liking.

```html
<div class="container">
  <div class="row">
    <div class="col-md-12">
      <p>
      	Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor 
        incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis 
        nostrud exercitation ullamco laboris nisi ut aliquip exea commodo consequat.
      </p>
      <p>
      	Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu 
        fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in 
        culpa qui officia deserunt mollit anim id est laborum.
      </p>
    </div>
  </div>
</div>
```

Show how to make uneven columns:

```html
<div class="container">
  <div class="row">
    <div class="col-lg-8">
      <p>
      	Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor 
        incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis 
        nostrud exercitation ullamco laboris nisi ut aliquip exea commodo consequat.
      </p>
    </div>
    <div class="col-lg-4">
      <p>
      	Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu 
        fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in 
        culpa qui officia deserunt mollit anim id est laborum.
      </p>
    </div>
  </div>
</div>
```

Show how to make responsive columns:

Add classes for smaller screen size col-10 and col-2 to divs above

## Grid Challenge
***slide 15*** **[SHOW](http://techtalentsouth.slides.com/techtalentsouth/ftci-bootstrap?token=JpCRR0qg#/0/15)**
*Give students 5 minutes to complete. Ask to raise hands or Slack you when complete and check their work.*

* Add a new row with 3 columns
* Hint: use class="col-md-4"

## Grid Solution
***slide 16*** **[SHOW](http://techtalentsouth.slides.com/techtalentsouth/ftci-bootstrap?token=JpCRR0qg#/0/16)**

Possible Solution:
```html
<div class="row">
  <div class="col-md-4">
    <p>Authentic typewriter you probably haven't heard of them normcores paleo.</p>
  </div>
  <div class="col-md-4">
      <p>Authentic typewriter you probably haven't heard of them normcore Ugh mi</p>
  </div>
  <div class="col-md-4">
    <p>Authentic typewriter you probably haven't heard of them normcore</p>
  </div>
</div>
```

## Contained in Style
***slide 17***

The grid system only provide this x- and y- layout system for your page. It does not have any border or background properties (unless you write your own). We'll look now at two Bootstrap containers that do have these properties built-in (and can be used easily within the grid system).

## Panels!
***slide 18***

[Panels](https://getbootstrap.com/docs/3.3/components/#panels)
Panels are made up of several classes attributed to nested <div> tags. There is the "panel" class, plus color choice in "panel-". Then the "panel-body" <div> would hold your main content. You can also add a "panel-heading" (above "panel-body"), and add a "panel-footer" (below "panel-body").

```html
<div class="panel panel-default">
  <div class="panel-heading">
      My Article Title
  </div> <!-- closing of panel heading -->
  <div class="panel-body">
    Lorem ipsum dolor sit amet, consectetur adipisicing elit, 
    sed do eiusmod tempor incididunt<super>*</super> ut labore 
    et dolore magna aliqua. Ut enim ad minim veniam.
  </div> <!-- closing of panel body -->
  <div class="panel-footer">
    <super>*</super>No, not really.
  </div> <!-- closing of panel footer -->
</div> <!-- closing of panel class -->
```

## Panel Challenge
***slide 19*** **[SHOW](http://techtalentsouth.slides.com/techtalentsouth/ftci-bootstrap?token=JpCRR0qg#/0/19)**
*Give students 5-10 minutes to complete. Ask to raise hands or Slack you when complete and check their work.*

* Make a new row with three columns
* Create a panel in each row w/ heading, body & footer
* Make them colorful!
* Let me know when you're finished
* Share with class

## Panel Solution
***slide 20*** **[SHOW](http://techtalentsouth.slides.com/techtalentsouth/ftci-bootstrap?token=JpCRR0qg#/0/20)**

Possible Solution:
```html
<div class="row">
	<div class="col-md-4">
		<div class="panel panel-primary">
			<div class="panel-heading panel-primary">Heading 1</div>
			<div class="panel-body">
				<p>Authentic typewriter you probably haven't heard of them normcore, sustainable hoodie Thundercats heirloom squid craft beer Schlitz. Ugh biodiesel Pitchfork mixtape High Life. Put a bird on it selvage bicycle rights paleo.</p>
			</div>
		</div> 
	</div>
	<div class="col-md-4">
		<div class="panel panel-success">
			<div class="panel-heading">Heading 2</div>
			<div class="panel-body">
				<p>Authentic typewriter you probably haven't heard of them normcore, sustainable hoodie Thundercats heirloom squid craft beer Schlitz. Ugh biodiesel Pitchfork mixtape High Life. Put a bird on it selvage bicycle rights paleo.</p>
			</div>
		</div>
	</div>
	<div class="col-md-4">
		<div class="panel panel-warning">
			<div class="panel-heading">Heading 3</div>
			<div class="panel-body">
				<p>Authentic typewriter you probably haven't heard of them normcore, sustainable hoodie Thundercats heirloom squid craft beer Schlitz. Ugh biodiesel Pitchfork mixtape High Life. Put a bird on it selvage bicycle rights paleo.</p>
			</div>
		</div>
	</div>
</div>
```

## Well, Well, Well 1
***slide 21***
[Bootstrap Wells](http://getbootstrap.com/docs/3.3/components/#wells)
The "well" class is like the panel's minimalist cousin. All you get is a grey box with a grey border surrounding your content. But, it's much more easier to implement.

```html
<div class="well">
  <p>
		Ten years ago a crack commando unit was sent to prison 
		by a military court for a crime they didn’t commit. 
		These men promptly escaped from a maximum security stockade to the Los Angeles underground. Today, still wanted by the government, they survive as soldiers of fortune. 
  </p>
</div>
```

## Well, Well, Well 2
***slide 22***

There's also some size variation for wells.

```html
<div class="well well-lg">
  <p>
    Kinda looks the same as just the standard well, really.
  </p>
</div>
<div class="well well-sm">
  <p>
    I'm a little well div, short and stout. Here's my grey
    background, here's my grey border.
  </p>
</div>
```

## Well Challenge
***slide 23- this slide has solution on it, wait to*** **[SHOW](http://techtalentsouth.slides.com/techtalentsouth/ftci-bootstrap?token=JpCRR0qg#/0/23)** 

*Give students 5 minutes to complete. Ask to raise hands or Slack you when complete and check their work.*

* Build a new row, this time with just 2 columns
* Surround content in each column in a <div> with a "well" class

Possible Solution:
```html
<div class="row">
  <div class="col-md-6">
    <div class="well">
      <p>
        Gumbo beet greens corn soko endive gumbo gourd. Parsley shallot courgette tatsoi pea sprouts fava bean collard greens dandelion okra wakame tomato. Dandelion cucumber earthnut pea peanut soko zucchini.
      </p>
    </div>
  </div>
  <div class="col-md-6">
    <div class="well">
      <p>
        Celery quandong swiss chard chicory earthnut pea potato. Salsify taro catsear garlic gram celery bitterleaf wattle seed collard greens nori. Grape wattle seed kombu beetroot horseradish carrot squash brussels sprout chard.
      </p>
    </div>
  </div>
</div>
```

## Buttons 1
***slide 24***

[Bootstrap Buttons](http://getbootstrap.com/docs/3.3/css/#btn-groups)

You can add Bootstrap button ("btn") classes to nearly any HTML tag (though results may vary). Generally, though, we add to <a> or <button> tags.

Add a button to your 3rd column:
```html
<a href="#" class="btn btn-success">Sign Up Now!</a>
```
## Buttons 2
***slide 25***

The "btn" classes will not alone create a button. You must also include the style/color of the button.

The choices are:
*"btn-default" (white)
*"btn-primary" (dark blue)
*"btn-info" (light blue)
*"btn-warning" (orange)
*"btn-danger" (red)
*"btn-success" (green)

You can also change the size of the button with:
*"btn-lg" (large)
*"btn-sm" (small)
*"btn-xs" (really small!)

## Add a Footer
***slide 26 - This slide has the solution, wait to*** **[SHOW](http://techtalentsouth.slides.com/techtalentsouth/ftci-bootstrap?token=JpCRR0qg#/0/26)**

Bootstrap doesn't have a prebuilt footer component like some of the other components we've used today, but with our new found grid knowledge, some `.nav` classes, and the `<footer>` tag, we can easily build our own.

*Add a footer
*Use a 3 column layout (again)
*Include the copyright symbol in the right col , navigation in the center col, and follow me in the right col.

```html
<footer>
  <div class="container">
    <div class="row">
      <div class="col-md-4">© 2014</div>
      <div class="col-md-4">
        <ul class="nav nav-pills">
          <li><a href="#">Contact Us </a></li>
          <li><a href="#">Get Support</a></li>
          <li><a href="#">Privacy Policy</a></li>
        </ul>
      </div>
      <div class="col-md-4">
        <p class="text-right"><a href="#">Follow Us</a></p>
      </div>
    </div>
  </div>
</footer>
```
Note that copyright symbol could just be copy-n-pasted in, or you can use the proper Number (&#169;) or Entity (&copy;).
W3Schools has a good [list of Symbols for HTML](https://www.w3schools.com/html/html_symbols.asp).

## Add a Glyph(icon)
***slide 27***

[Bootstrap Glyphicons](http://getbootstrap.com/docs/3.3/components/#glyphicons)

Glyphicons are just Bootstrap's fancy name for "icons" - those little pictures that often precede or procede text, or maybe replace text altogether, as a representation of what is meant (e.g., having an envelope icon instead of the phrase "Email us").

Glyphicons come through as classes. Like the Bootstrap buttons, these are dependent on two classes: "glyphicon" and "glyphicon-", where the hyphen is proceded by a certain glyphicon name (see the docs).

You can house these classes in either a <span> tag (traditional choice), or an <i> tag (as some developers have recently taken to doing, as italics was moved to the <em> tag).
	
New Accessibility guidelines state that websites must be accessible to blind people.  There is a tag attribute and a Bootstrap class we'll use here to make a complete experience for them. `aria-hidden="true"` will keep screen readers from announcing unuseful information when there is text present.

Example, within our Footer:
```html
<li>
  <a href="#">
    Contact Us <span class="glyphicon glyphicon-envelope" aria-hidden="true"></span>
  </a>
</li>
```

Where icons are used without text we add another `<span>` that gives a screen reader a written interpretation of the icon.

```html
<li>
  <a href="#">
    <span class="glyphicon glyphicon-envelope" aria-hidden="true"></span>
    <span class="sr-only">Email</span>
  </a>
</li>
```

[Font Awesome](http://fontawesome.io/) is another place to get icons (and with a much greater selection to choose from). You go have to request a CDN link, however - it is no longer available right on their site.


## Images
***slide 28***
[Bootstrap Images](http://getbootstrap.com/docs/3.3/css/#images)

Bootstrap has some sweet classes for your images.

The "img-responsive" class makes your images change its size relative to the browser size.
```html
<img src="assets/images/llamas.jpg" class="img-responsive" alt="llamas!" />
```

There are also classes that can change the overall shape of your image:
```html
<img src="assets/images/llamas.jpg" alt="llama rounded" class="img-rounded">
<img src="assets/images/llamas.jpg" alt="llama circle!" class="img-circle">
<img src="assets/images/llamas.jpg" alt="llamas w/ nice border" class="img-thumbnail">
```

* "img-rounded" - adds a border-radius
* "img-circle" - image is now circle-shaped!
* "img-thumbnail" - gives a rounded border


## Throw in a Navbar
***slide 29***

[Bootstrap Navbar](http://getbootstrap.com/docs/3.3/components/#navbar)

A *navbar* is that container you see at the top of many websites, listing contents of the page or links to other pages. All the social media sites have one. Bootstrap makes them easy!

Taken straight from the docs:
```html
<nav class="navbar navbar-default" role="navigation">
  <div class="container-fluid">
    <!-- Brand and toggle get grouped for better mobile display -->
    <div class="navbar-header">
      <button type="button" class="navbar-toggle collapsed" data-toggle="collapse" data-target="#bs-example-navbar-collapse-1">
        <span class="sr-only">Toggle navigation</span>
        <span class="icon-bar"></span>
        <span class="icon-bar"></span>
        <span class="icon-bar"></span>
      </button>
      <a class="navbar-brand" href="#">Brand</a>
    </div>
    <!-- Collect the nav links, forms, and other content for toggling -->
    <div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">
      <ul class="nav navbar-nav">
        <li class="active"><a href="#">Link</a></li>
        <li><a href="#">Link</a></li>
        <li class="dropdown">
          <a href="#" class="dropdown-toggle" data-toggle="dropdown">Dropdown <span class="caret"></span></a>
          <ul class="dropdown-menu" role="menu">
            <li><a href="#">Action</a></li>
            <li><a href="#">Another action</a></li>
            <li><a href="#">Something else here</a></li>
            <li class="divider"></li>
            <li><a href="#">Separated link</a></li>
            <li class="divider"></li>
            <li><a href="#">One more separated link</a></li>
          </ul>
        </li>
      </ul>
      <form class="navbar-form navbar-left" role="search">
        <div class="form-group">
          <input type="text" class="form-control" placeholder="Search">
        </div>
        <button type="submit" class="btn btn-default">Submit</button>
      </form>
      <ul class="nav navbar-nav navbar-right">
        <li><a href="#">Link</a></li>
        <li class="dropdown">
          <a href="#" class="dropdown-toggle" data-toggle="dropdown">Dropdown <span class="caret"></span></a>
          <ul class="dropdown-menu" role="menu">
            <li><a href="#">Action</a></li>
            <li><a href="#">Another action</a></li>
            <li><a href="#">Something else here</a></li>
            <li class="divider"></li>
            <li><a href="#">Separated link</a></li>
          </ul>
        </li>
      </ul>
    </div><!-- /.navbar-collapse -->
  </div><!-- /.container-fluid -->
</nav>
```
## Navbar Features:
***slide 30***

*Brand (usually link to home/index page)
*Dropdowns
*Active Class (denotes which page you're currently on)
*Inline Form (we would need RoR or JS to power)
*Placeholder (text within input fields)

You can pick and choose from their main example, you probably won't need every aspect of a Bootstrap navbar. But don't delete the *navbar-header* <div> or remove "navbar-collapse" from its <div>. These power the responsive design of the navbar, which will gradually put items (from the right) into what is called a "hamburger menu", as the browser size decreases. (Try it out!)

If Dropdowns are not working, check to make sure jQuery and Bootstrap JS CDN links are written properly.

## Add a Media Object
***slide 31***

[Bootstrap Media Object](http://getbootstrap.com/docs/3.3/components/#media)

Media Object classe are for building various types of components (like blog comments, Tweets, etc.) that feature a left- or right-aligned image alongside textual content.

Add a media object into your second column panel:
```html
<div class="media">
  <a class="pull-left" href="#">
    <img class="media-object" src="assets/images/daschund.jpg" alt="Doxie">
  </a>
  <div class="media-body">
    <h4 class="media-heading">Mini Dachshunds are my favorite</h4>
    <p>
        Authentic typewriter you probably haven't heard of them normcore, 
        sustainable hoodie Thundercats heirloom squid craft beer Schlitz. 
        Ugh biodiesel Pitchfork mixtape High Life. Put a bird on it selvage 
        bicycle rights paleo.
    </p>
  </div>
</div>
```


## Embed a Video!
***slide 32***

[Bootstrap Responsive Embed](http://getbootstrap.com/docs/3.3/components/#responsive-embed)

These classes are MAGIC because they allow browsers to determine video or slideshow dimensions based on the width of their containing block by creating an intrinsic ratio that will properly scale on any device.

We're also using `<iframes>`, which allow us to embed an HTML document, inside another HTML doc.  It is used to insert content from other sources, like Ads, for instance. The `.embed` classes activate some Javascript to make it work.

Find a video on YouTube and add it to the first column panel:
```html
<div class="embed-responsive embed-responsive-16by9">
  <iframe src="http://www.youtube.com/embed/jR4lLJu_-wE" class="embed-responsive-item">
  </iframe>
</div>
```

## Div-ception 1
***slide 33*** **[show](http://techtalentsouth.slides.com/techtalentsouth/ftci-bootstrap?token=JpCRR0qg#/0/33)**

Exhibit meme.

## Div-ception 2
***slide 34*** 

Using Bootstrap to development is a shortcut, but that doesn't mean it can't seem incredibly complicated. You tend to have `<div>` within `<div>` within `<div>` to get the right layout.
We saw an example of this when working with **Panels**.

## Challenge: Deep Div-ing 1
***slide 35***  **[SHOW](http://techtalentsouth.slides.com/techtalentsouth/ftci-bootstrap?token=JpCRR0qg#/0/35)
**

The challenge is to create a layout similar to the one shown in the slide. 

## Challenge: Deep Div-ing 2
***slide 36*** **[SHOW](http://techtalentsouth.slides.com/techtalentsouth/ftci-bootstrap?token=JpCRR0qg#/0/36)**

Possible Solution:
```html
<div class="container">
  <div class="row">
    <div class="col-md-3">
      <h1>Left Sidebar</h1>
      <p>
  			Cut the cheese st. agur blue cheese fromage. Taleggio caerphilly bocconcini caerphilly cheese and wine feta brie feta. Dolcelatte monterey jack melted cheese dolcelatte cream cheese croque monsieur brie cheese triangles. Hard cheese the big cheese squirty cheese everyone loves bavarian bergkase cow caerphilly gouda. Stinking 
        bishop cheese slices bavarian bergkase dolcelatte.
      </p>
    </div>
    <div class="col-md-6">
      <div class="row">
  	<div class="col-md-4">
  	  <h3>Column 1</h3>
  	  <p>
  	    Queso rubber cheese melted cheese. Halloumi cheesy grin smelly cheese fondue brie taleggio dolcelatte red leicester.
          </p>
  	</div>
  	<div class="col-md-4">
  	  <h3>Column 2</h3>
  	    <p>
  	      Mascarpone dolcelatte cheese and biscuits camembert de normandie fondue fromage frais cheese on toast pepper jack. Goat.
            </p>
        </div>
  	<div class="col-md-4">
  	  <h3>Column 3</h3>
  	  <p>
  	    Halloumi melted cheese bocconcini. Manchego danish fontina cream cheese when the cheese comes out everybody's happy melted cheese.
          </p>
  	</div>
      </div>
    </div>
    <div class="col-md-3">
      <h1>Right Sidebar</h1>
      <p>
  			Cottage cheese caerphilly jarlsberg. Monterey jack rubber cheese port-salut cheeseburger cut the cheese manchego mascarpone smelly cheese. Chalk and cheese caerphilly queso cheese and biscuits cheese and biscuits cheddar parmesan everyone loves. Port-salut squirty cheese emmental the big cheese mascarpone red leicester melted cheese taleggio. Hard cheese cheese triangles cheese and wine.
      </p>
    </div>
  </div>
</div>
```


## Additional Features
***slide 37***

If time permits, have students research and present these features.  Otherwise just highlight them quickly without code.

* [Pagination](http://getbootstrap.com/components/#pagination)
* [Badges](http://getbootstrap.com/components/#badges)
* [Alerts](http://getbootstrap.com/components/#alerts)
* [Progress Bar](http://getbootstrap.com/components/#progress)
* [Carousel](http://getbootstrap.com/javascript/#carousel)


## Overriding Bootstrap Styles 1
***slide 38***

* Add an external style sheet called "styles.css" to the "assets/stylesheets" folder.
* Link this stylesheet to **index.html** - make sure this link is *after* your Bootstrap CDN link (page is read top-to-bottom).
* Use the Inspector tool to determine the class you'd like to override. 
* Write styles that take priority over Bootstrap's styles.

## Overriding Bootstrap Styles 2
***slide 39***

Let's say we want to change the color of `<h1>`s inside the "jumbotron". Inspecting that element tells us that the color in this Bootstrap style is inherited from a parent element.

To change this style we can define it in our own stylesheet. Inspecting again shows the Bootstrap style crossed out with our own taking priority.

```css
.jumbotron h1 {
  color: orange;
}
```

**Why does this work?**

## Bootstrap Themes
***slide 40***

NEED INSTRUCTIONS HERE FOR IMPLEMENTING FROM ONE OF THESE

[Bootswatch](http://bootswatch.com/)
[Wrap Bootstrap](https://wrapbootstrap.com/)
[Start Bootstrap](https://startbootstrap.com/)

## Review Questions
***no slide ***

* What is Bootstrap? *A front-end framework with predefined CSS rules and JS code.*
* What is the grid system in bootstrap? *The page is divided into 12 columns and as many rows as needed. Each column can be subdivided in the same way*
* What are the three main classes that make up the grid system? *.container, .row, .col*
How many columns are in a row? *12*
How do we specify the width of column? *using the .col-"screen-size value"-"(1-12)" class format.*
How does the xs, sm, med, lg, xl sizing work when on the same element? *Specifies column width at varying screen sizes."
What class do use to make buttons in bootstrap? *.btn and .btn-"color"*
What class do we use to make nice text boxes?
<div> .card
<div> .card-body
<p> .card-tex



## Activity + Homework
***slide 41***

(or in-class activity, if time allows)

* Add Bootstrap to your personal site
* Add a navbar, header and footer
* Flow each page of your content into a 2 or 3 column grid
* Add an external stylesheet and write a few styles
* If there's time: add a theme!

#### Div-ception Challenge II: Div-ing Deeper
* Try to mimic Twitter's layout (the feed page)
* Just fake the content
* Use custom CSS to size the user pic
