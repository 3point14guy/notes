# HTML: The Basics  

Let's take a dive into HTML (Hyper Text Markup Language). HTML is the markup language that provides us a structured way to create web pages. When we use the tags available to us through HTML we can control how almost everything is presented to the user via their web browser. Your web browswer it what interprets the tags to present what the end user sees.

## Useful Resources
[HTML Elemenent/Tag Reference](https://developer.mozilla.org/en*US/docs/Web/HTML/Element)  
[HTML 5 Reference](https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/HTML5)  
[Forms in HTML](https://developer.mozilla.org/en*US/docs/Web/Guide/HTML/Forms_in_HTML)  
[Pixlr * Online photoshop clone](https://pixlr.com/editor/)

---

1. HTML and its Tags
2. Basic HTML page structure
3. Site Organization
4. Some common tags explained
    * Headings
    * Paragraphs
    * Lists
    * Links
    * Images
    * Tables
    * Divs
    * Spans
5. Exercises
    * [Simple Travel Site](exercises/simple_travel_site/)
6. Homework

---

## Hyper Text Markup Language HTML5  
***slide 1***

HTML is a markup language interpreted by the browser. It is different from a programming language. It uses specific tags or elements to tell the browser how things should look when presented visually to the user. Lets take a look at some concepts behind how these tags work before we dive into actual tags.

## HTML and Its' Tags 1  
***slide 2***

***Not all tags in these examples are real HTML tags. They are being used to explain the concepts.***

* Tags are surrounded by angle brackets. Notice the closing tag has the `/` character. This lets the browser know that this is the matching closing tag. Tags are almost always followed up with matching closing tags. Just like we might wrap a book title in quotes, we wrap our content in tags.
```html
  <tag>some content</tag>
```


## HTML and Its' Tags 2  
***slide 3***

* Tags can also be nested inside each other.
```html
  <pizza>
    <topping>pineapple</topping>
    <topping>ham</topping>
    <topping>cheese</topping>
  </pizza>
```


## HTML and Its' Tags 3  
***slide 4***

* Tags use something called attributes to add additional functionality or describe behavior. 
* The attribute types can change depending on the element.


*Some attributes related to styling have been deprecated over time. It's best practices to do styling through CSS (Cascading Style Sheets).*
```html
  <tag attribute="value">some content</tag>
```


## HTML and Its' Tags 4  
***slide 5***

* Some tags are self closing. These tags are usually not intended to wrap content. The `<img>` tag is a common self closing tag. As you can see there is still a `/` at the end of the tag. Notice the use of attributes as well.
```html
  <img src="path/to/image/file.jpg" alt="image description" />
```


##  HTML Basic Layout   
***slide 6***

It this section we will take a look at the structure of an html file. We will discuss the basic tags need to have a well formed page for display on the web.
From this point on we will be speaking in context of HTML5.

**HTML basic example**
```html
  <!DOCTYPE html>
  <html>
  <head>
    <title></title>
  </head>
  <body>

  </body>
  </html>
```

**Lets take a look at some of these tags/elements**

***PRO TIP***  
*https://developer.mozilla.org/en-US/docs/Web/HTML/Element/YOUR_TAG_HERE - Replacing YOUR_TAG_HERE with something like 'head', 'title', or any other tag/element you can think of should bring you to documentation on that specific tag/element*

## Doctype
***slide 7***

`<!DOCTYPE html>` informs the browser which version of HTML will be used in this document. In this case we are delacring that we will be using HTML5 Doctype is speacial in that its not really considered a tag but a declaration. What you really need to understand about this tag/declaration is that it should be present and the first line in your HTML file.

## HTML  
***slide 8***

`<html>` is the tag that represents the root (top-level element) of an HTML document, so it is also referred to as the root element. All other elements must be descendants of this element. That being said, notice the mathing closing `</html>` tag on the last line.

## Head  
***slide 9***

`<head>` is that tag that contains descriptive information about your page like the pages title `<title>`. This is also where would include things like `<meta>` and style sheets. We will get into more of that stuff later in the curriculum. Notice the `<title>` tag has a closing tag, `</title>`, just like the `<html>` tag. I hope you are noticing the pattern.  
[Recomended Reading](https://developer.mozilla.org/en-US/docs/Learn/HTML/Introduction_to_HTML/The_head_metadata_in_HTML)

**title**  
***no slide***

`<title>` is the tag that defines the title of the document, shown in a browser's title bar or on the page's tab. It can only contain text, and any contained tags are ignored. It is followed by the closing `</title>` tag.

**link**  
***no slide***

The `<link>` tag creates connections with external resources.

**meta**  
***no slide***

The `<meta>` tag gives data about our website to browsers and crawlers.  The values of `name` attributes declare such things as the character set the browser should use, a description of the website, the author and how to scale the website.

## Body  
***slide 10***

`<body>` represents the content of an HTML document. There can be only one `<body>` element in an HTML document. This is where the things you want people to see would go. I am going to stop pointing out the closing tags. I think you got it by now.

## Get Organized  
***slide 11*** 

There are many opionions on how the files you create should be organized when building web applications. For the purposes of this course we will focus one of those opinions as outlined in this section. This will help things look familiar as we get into some of the later topics in the course.

## Get Organized  
***slide 12***  **[SHOW](http://techtalentsouth.slides.com/techtalentsouth/html-the-basics-code-immersion?token=-T3Bz9tS#/0/12)**

**Directory structure example**  
*This is a basic example and can/will get more complicated as we progress through the curriculum. This is a great place to start and should get you through the next few lessons.*
```
root_directory
|
│   index.html
│   conat_us.html    
│   ...
|
└───assets
    │
    └───images
        |
        │   image1.jpg
        │   image1.png
        │   ...
        |
        javascripts
        |
        │   jquery.js
        │   slideshow.js
        │   ...
        |
        stylesheets
        |
        │   bootstrap.css
        │   custom.css
        │   ...
        |
```

**Whats going on here?**  

**root_directory**  
Every project has a root directory. This is the top level folder where all files for the project will exist. This is where our .html files will live as well.

**assets**  
assets is a sub-directory uner our root directory. This is the spot we save items we will use with our website. This can be images we intend to display, JavaScript we include for added functionality, or stylesheets we include to make things look cleaner. 


## Popular HTML Tags  
***slide 13***

Let's take some time to cover some of the most common HTML tags you will use when creating web applications. This will be an interactive lession so we can see the result of using these tags in a broswer. We will build a simple travel page for our example. 

## Travel Page Set Up 1  
***slide 14***

Rename root directory **travel_site**

## Travel Page Set Up 2  
***slide 15***

Open file structure in Sublime.  Create the basic structure.

```html
<!DOCTYPE html>
<html>
<head>
  <title>My Travel Site</title>
</head>
<body>

</body>
</html>
```


## Headings  
***slide 16***

The HTML `<h1>` – `<h6>` tags represent six levels of section headings. `<h1>` is the highest section level and `<h6>` is the lowest. 

```html
<h1>Welcome to my Travel Site</h1>

<h2>Here are some places I'd like to visit</h2>
```
 
## Paragraphs  
***slide 17***

The HTML `<p>` tag represents a paragraph of text. 

```html
/*<h1>Welcome to my Travel Site</h1>*/
<p>This site is all about where I want to travel.</p>

/*<h2>Here are some places I'd like to visit</h2>*/
```

## Lists 1   
***slide 18***

HTML has the ability to display lists. Lists are typicaly ordered or unordered. `<ol>` represents an ordered and `<ul>` represents and unordered list. Ordered lists are numbered and undordered lists use some type of bullet. List items are represented by the `<li>` tag in both ordered and unordered lists.

## Lists 2  
***slide 19***

```html
/*<h2>Here are some places I'd like to visit</h2>*/
<ul>
  <li>Spain</li>
  <li>Italy</li>
  <li>Aruba</li>
  <li>Alaska</li>
</ul>
```

## Links 1 
***slide 20***

Links are represented by the anchor tag (`<a>`). The anchor tag creates a hyperlink. A hyperlink is a link from to another location or file, typically activated by clicking on a highlighted word or image on the screen. 

A hyperling can take the user to other web pages, files, locations within the same page, email addresses, or any other URL. The text or element between the oening and closing `<a>` tag is what becomes the link.

## Links 2  
***slide 21***

**Basic anatomy of the anchor tag `<a>`**  
There sevaral attributes for an `<a>` tag. Lets go over a few of the common ones.  

* **href** - The URL or destination hyperlink points to.
* **target** - Specifies where to display the linked URL. Below are the most common values used.
    * **_self:** Load the URL into the same browser window as the current one. This is the default behavior if the target attribute is not used.
    * **_blank:** Load the URL into a new browser window or tab based on how the users browser is configured.

*[Further Reading on the `<a>` tag](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/a)*

## Basic anchor examples  
***slide 22***  **[SHOW](http://techtalentsouth.slides.com/techtalentsouth/html-the-basics-code-immersion?token=-T3Bz9tS#/0/22)**

```html
<!-- external hyperlink opens in a new window -->
<a href="http://www.google.com" target="_blank">google.com</a>

<!-- external link opens in the same window -->
<a href="http://www.google.com">google.com</a>

<!-- Email link that opens users default email client -->
<a href="mailto:user@example.com">Email User</a>

<!-- link to local another file on the same site -->
<a href="my_html_file.html">go to my_html_file</a>

<!-- links to element on this page with id="page-section" -->
<a href="#page-section">Link to page section</a>

<!-- External link that opens in a new window/tab using an image instead of text -->
<a href="https://developer.mozilla.org/en-US/" target="_blank">
  <img src="https://mdn.mozillademos.org/files/6851/mdn_logo.png"/>
</a>
```

## Travel Page: Links  
***slide 23***

```html
<ul>
  <li>
    <a href="http://www.spain.info/en_US/" target="_blank">Spain</a>
  </li>
  <li>
    <a href="http://www.italia.it/en/home.html" target="_blank">Italy</a>
  </li>
  <li>
    <a href="http://www.arubatourism.com/" target="_blank">Aruba</a>
  </li>
  <li>
    <a href="https://www.travelalaska.com/" target="_blank">Alaska</a>
  </li>
</ul>
```
## Images  
***slides 24***

Images are placed in our HTML document using the `<img>` tag. The image tag is a self closing tag and **Does'nt** require a matching closing tag.


There sevaral attributes for an `<img>` tag. Lets go over a few of the common ones.  

* **src** - The image URL. This attribute is mandatory for the `<img>` element.
* **alt** - This attribute defines description of the image. Users will see this text displayed if the image URL is wrong, the image is not in one of the supported formats, or if the image is not yet downloaded.
* **height** - The height of the image in pixels
* **width** - The width of the image in pixels

## Basic Image Examples 
***slide 25***  **[SHOW](http://techtalentsouth.slides.com/techtalentsouth/html-the-basics-code-immersion?token=-T3Bz9tS#/0/25)**

**Basic `<img>` examples**
```html
<!-- image with width and height set -->
<img src="some_image.png" alt="TTS" width="200" height="200">

<!-- Image used as a link -->
<a href="https://developer.mozilla.org/en-US/" target="_blank">
  <img src="https://mdn.mozillademos.org/files/6851/mdn_logo.png" alt="Mozilla Logo"/>
</a>
```
***Discussion Topics***  
*What happens to the image if only width or height is specified?*

*[Further Reading on the `<img>` tag](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/img)*

## Travel Page: Images  
***slide 26***

```html
<li>
  <img src="https://upload.wikimedia.org/wikipedia/en/9/9a/Flag_of_Spain.svg" width="50">
  <a href="http://www.spain.info/en_US/" target="_blank">Spain</a>
</li>
<li>
  <img src="https://upload.wikimedia.org/wikipedia/en/0/03/Flag_of_Italy.svg" width="50">
  <a href="http://www.italia.it/en/home.html" target="_blank">Italy</a>
</li>
<li>
  <img src="https://upload.wikimedia.org/wikipedia/commons/f/f6/Flag_of_Aruba.svg" width="50">
  <a href="http://www.arubatourism.com/" target="_blank">Aruba</a>
</li>
<li>
  <img src="https://upload.wikimedia.org/wikipedia/en/a/a4/Flag_of_the_United_States.svg" width="50">
  <a href="https://www.travelalaska.com/" target="_blank">Alaska</a>
</li>
```

**Links to share**
https://upload.wikimedia.org/wikipedia/en/9/9a/Flag_of_Spain.svg

http://www.spain.info/en_US/

https://upload.wikimedia.org/wikipedia/en/0/03/Flag_of_Italy.svg

http://www.italia.it/en/home.html
 
https://upload.wikimedia.org/wikipedia/commons/f/f6/Flag_of_Aruba.svg

http://www.arubatourism.com/
 
https://upload.wikimedia.org/wikipedia/en/a/a4/Flag_of_the_United_States.svg

https://www.travelalaska.com/

## Tables  
***slide 27***

The HTML `<table>` element represents tabular data. Think of the way a spreadsheet presents data. Tables allow you to arrange rows and columns to present data to the user. We are only going to scratch the surface on tables in this example so please explore the further reading link provided. 

**Basic anatomy of a `<table>`**  
There sevaral nested tags for the `<table>` tag that help build the rows and columns. Lets go over a few of the common ones.  

* **`<tr>`** - represents the start of a new row.
* **`<td>`** - represents the start of a new column within a row.
* **`<th>`** - represents a special header column. `<th>` should only be used in the first column in place of the standard `<td>`. Headers will appear bold.

*[Further Reading on the `<table>` tag](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/table)*

## Basic Table Example  
***slide 28***

```html
<!-- basic table with columb headers -->
<table>
  <tr>
    <th>First name</th>
    <th>Last name</th>
  </tr>
  <tr>
    <td>John</td>
    <td>Doe</td>
  </tr>
  <tr>
    <td>Jane</td>
    <td>Doe</td>
  </tr>
</table>
```

## Travel Page: Tables  
***slide 29***  **[SHOW](http://techtalentsouth.slides.com/techtalentsouth/html-the-basics-code-immersion?token=-T3Bz9tS#/0/29)**

```html
<!-- This is where our list ended -->

<table>
  <tr>
    <th>Destination</th>
    <th>Capital</th>
    <th>Official Language</th>
  </tr>
  <tr>
    <td>Spain</td>
    <td>Madrid</td>
    <td>Spanish</td>
  </tr>
  <tr>
    <td>Italy</td>
    <td>Rome</td>
    <td>Italian</td>
  </tr>
  <tr>
    <td>Aruba</td>
    <td>Oranjestad</td>
    <td>Dutch</td>
  </tr>
  <tr>
    <td>Alaska</td>
    <td>Juneau</td>
    <td>n/a</td>
  </tr>
</table>
```

## Divs  
***slide 30***

The HTML `<div>` element is a generic container for content flow. We are not going to use this in our travel site but you will see it as we go through the curriculum. It is mainly used to group elements for styling and positioning. 

The `<div>` is a block-level element. Browsers typically display the block-level element with a newline both before and after the element.  
[Further reading on block-level elements](https://developer.mozilla.org/en-US/docs/Web/HTML/Block-level_elements)

* Basic `<div>` example
```html
<div>
  <p>Any kind of content here. Such as
  <p> or <table> tags. You name it!</p>
</div>

```

## Spans 
***slide 31***

The HTML `<span>` element is a generic container for content flow. similar to the `<div>`, we are not going to use this in our travel site but you will see it as we go through the curriculum. It is mainly used for styling purposes.

The `<span>` is an inline element. An inline element occupies only the space bounded by the tags that define the inline element. Basically that means you can wrap it around a work or something similar and it wont add new lines above and below the span.
[Further reading on inline tags](https://developer.mozilla.org/en-US/docs/Web/HTML/Inline_elements)

* Basic `<span>` example
```html
  <p>Any kind of <span>content</span> here.</p>
```
## END OF SLIDES

## Student Resources

You will use the internet continuously throughout your career as a resource.  There are too many technologies to know them all and new ones are being developed every week it seems.  In the beginning, you'll use the net to learn the basics and as you develop, you'll use the web to learn new tech. It is important to know how to use the resources that are out there.

We are going to look at two excellent resources for you to use in this course; Mozilla and W3Schools.

[Mozilla Developers Network](https://developer.mozilla.org/en-US/docs)

[W3Schools](https://www.w3schools.com/)

## Activity

* split the students in two groups
* each person in the first group will research an HTML element at Mozilla, the second group will use W3Schools
* after 5 minutes, each student will give a synopsis of the element and how it is used

## Activity

Sign-Up page

Use the instructions below to create a simple sign-up page.  Tomorrow, we'll add CSS to make it look pretty.

1. Start with the standard HTML boilerplate.
2. In the body, add a set of `<div>` tags. Give it an "id" attribute with the value of "login-box".
3. Inside of the top set of `<div>` tags, nest two other sets of `<div>` tags, one right after the other.  In the top `<div>`, put a "class of "left", in the bottom `<div>`, the "class" of "right".
4. Inside of the `<div>` with a class of "left", add an `<h1>` that says "Create Account".
5. Below the `<h1>` tag, put a `<form>` tag. For it's attributes put "class" "action" and "method".  The values for those attributes will be "sign-up", "index.html", and "POST", respectively.
6. Nested in the `<form>` tag, put 4 `<input>` tags. All 4 will have a "type" attribute with the value of "text". All `<input>`s will have "name" and "placeholder" attributes.
   - The value for both "name" and "placeholder" attributes in the 
      - first `<input>` will be "username"
      - second `<input>` will be "email"
      - third `<input>` will be "password"
   - In the fourth `<input>`, the value for "name" will be "confirm-password" and the value for "placeholder" will be "re-type password".
7. After the last `<input>`, but still within the `<form>` tags, add `<button>` tags. The "type" will be "submit", the "class" will be "sign-up-button" and the button should read "Sign Me Up".
8. In the bottom `<div>`, add a `<span>` tag first.  The text with the tags should read "Sign in with Social Network"
9. Next, create 3 sets of `<button>` tags underneath the `<span>` tags. The text for the buttons will be "Log in with FaceBook", "Log in with Twitter", "Log in with Google+". Each `<button>` tag will have two classes.  The first "class" for all 3 will be "social-signin".  The second "class" will either be "facebook", "twitter", or "google", to go along with the corresponding button text.
10. Make a new line between the last two closing `</div>` tags and put a new pair of `<div>` tags there with the text "or" and give it a class of "or".
11. Open the file in your browser...it wont look so good...yet.




## Review Questions

* When we use HTML we are wrapping our contents in? *Tags*
* The tag at the beginning is called the ? tag and the one at the end is a ? tag.  *Opening and closing*
* How do we tell the difference between those? */ in closing tag*
* Do we always need a closing tag?  What is an example of a self closing tag? *`<img>`*
* What do we call the part of a tag that gives it more functionality? *Attribute*
* What does the DOCTYPE declaration do? *Tells the browser what type of document it will render.*
* What purpose does the <head> serve? *Gives info about the document to the browser.*
* When making lists, what is the difference between ol and ul? *ordered/ unordered*
* Which attribute of an anchor tag determines where the link opens? *Target*
* What is the only attribute that an <img> tag must have? *src*
* What are the differences between a <div> and a <span> tag? *Div for large portions of your code and starts on a new line; span for small chunks of code and stays inline*


##Homework
For homework we are going to create a simple page that displays your 5 favorite restaurants in your favorite city. You should use all the tags we used to create our travel site.

**Easy Challenge**  
[Reference for challenge](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/b)  
Find some tags using the reference above to style some of the text on the page. This will help you get familiar with reading documentation. Learning on your own is a big part of being a succesful developer.

**Medium Challenge**  
Make one of your favorite places to eat your own kitchen. Create another HTML page for your kitchen (e.g. my_kitchen.html). Make sure you link to your new page from the main page. Also make sure you can get back to your main page from your new page. This page should **NOT** open in a new window.

***Link to an internal page example*** 
```html
<a href="my_kitchen.html">My Kitchen</a>
```

**Hard Challenge**  
[Reference for challenge](https://developer.mozilla.org/en*US/docs/Web/Guide/HTML/Forms_in_HTML)  
Add a contact form to your page. This form does not need to function. Only worry about getting in to display on your page. Collect the users name, email, and comment. 
