# JavaScript & jQuery
## Make Your Site Dynamic!

We're going to head back to the browser and learn a little about JavaScript before we start in on Rails.

Useful Resources:
[Javascript.com](https://www.javascript.com/)
[W3Schools - JavaScript](https://www.w3schools.com/js/)
[jQuery Official Site](https://jquery.com/)
[jQuery API Docs](https://api.jquery.com/)
[jQuery UI](https://jqueryui.com/)
[Ruby & JS: WAT?](https://www.destroyallsoftware.com/talks/wat)

---

1. Homework Review
2. JavaScript Intro
	* Client vs. Server
	* History of JS
	* Frameworks
3. The Console
4. Strings
5. Numbers
6. Mathmatical Operations
7. Data Type Conversion
8. Writing JavaScript Code
9. Variables
10. Dynamic Programs
11. Functions
12. HTML Functions
13. Conditionals
14. JS Activity: MadLibs
15. jQuery Intro & Set-Up
16. Click & Hover Functions
17. More jQuery Functions
18. jQuery UI


---

### JavaScript Intro
JavaScript is an object oriented programming language that is primarily used to make web pages interactive (though it can be used to run databases, mobile apps, and even robotics). It is also the most popular programming language in the world! 
 
Why is it so popular? Because it runs in all browsers, or client side, so it's functionality works on all devices with a browser. This gets rid of our need for server requests and makes web apps much, much faster. 
 
JavaScript is growing everyday, and is becoming a ubiquitous language across all platforms.


#### Client vs. Server
The client is your computer, or more specifically, your browser. And remember the browser can only read three languages! 

Because of that, any web application that we have written in any other language has to send a request to a remote server. This request takes time. The more requests you make, or the bigger the request is, the slower your application. 


#### JS: A History
JavaScript was developed by Netscape in the early 1990s as a way to provide interactivity in web pages. Before JavaScript, web pages were essentially static documents: once you loaded a page, the content didn't change.

Actually, it was created in 10 days. And because it was a rush job, it wasn't fully functional, nor was it standardized.


#### JS Frameworks
[Js library comic](http://techtalentsouth.slides.com/techtalentsouth/javascript-jquery-ci?token=QRVJGNY1#/0/5)

Because JavaScript is a language that has been sort of hacked together over the ages, developers have been trying to make the language more uniform and easier to write. 

The common way this is done is through a library (or framework). A JavaScript library is pre-written JS which allows for easier development of common JS practices. Unlike other languages which have just a few frameworks each, JS has TONS of libraries.


### The Console
Let's get into some pure JavaScript and run some commands, functions, and programs in the console. Where is your console? In your browser, of course!
 
Console is another term for terminal, but basically it is an area that reads and executes inputted commands. Fortunately for us, Chrome has a built-in console that reads and executes JavaScript!

Go to any web page, right click, select inspect element,
go to the console tab.  While here, enter:

```sh
> console.log("Hello, World!");
```

* We type commands
* Something may be is displayed
* A value is returned


### Strings
*Instructor Note:
If you are running this lesson after going through Ruby, the students should obviously know what a String is.*

“Hello World” is what is called a String, a data type frequently used in JavaScript and all programming. A String  is a way of representing a sequence of characters. Here are some examples of JavaScript strings:

```javascript
“My name is Nick”

‘word’

“1234”

// we can assign to a variable

a = "word"

// and we can find out what data type we are working with by using a function.  (We know about methods, methods are Ruby's version of a function.)

typeof(a);

// we can also do

typeof("1234");

```
As you can see above, we surrounded our Strings in single-   or double-quotes .

Take special note of the last example, “1234.” Although it has a numeric value in it, you may not be able to perform arithmetic operations on it. We'll touch more on that in a bit.


### Numbers
*Instructor Note:
Once again, if you're teaching in a post-Ruby environment, students know Integers and Floats - however, point out that JavaScript does not descriminate between those two types, and they're all just Numbers.*

Within JavaScript, we have numbers as well. These numbers can be with or without decimals. This is the Number data type. Take a look at the examples below:
```javascript

	// show what would be considered in Ruby an integer
  > 1234
	
	// show what would be considered in Ruby a float
  > 23.4  

	// show those two divided
	> 1234 / 23.4
	
	// which will return
	<* 52.7350....
	
	// now let's divide two integers:
	> 9 / 4
	<* 2.25
	
	// What's happening?  Let's check it's data type
	> typeof(9)
	<* "number"
	
	> typeof(23.4)
	<* "number"
```

There is no separate float data type in JS. What you know of as Integers and Floats are just considered numbers, but JS numbers will behave like Ruby Floats. This is not usually the case in programming languages. Usually whole numbers and decimals are two different data types!! The good news is that a modulus will still work like you expect:

```sh
> 9 % 4
<* 1
```

### Mathmatical Operations
The following are basic  arithmetic operators :
 
\+  for addition
\-  for subtraction
\*  for multiplication
\/  for division

Now let’s use a few of these in console :
```sh
> 12 * 4
<* 48
> 34 / 3
<* 11.333333333333334
> 8 / 0
<* Infinity
```

From these examples, you can see that one of JavaScript ’s many functions is to perform simple calculations.

#### String Arithmetic
Interestingly enough, you can also use the arithmetic operators mentioned above on strings.
```sh
> “Hello, “ + “stranger”;
```
This is known as concatenation! You will see it a lot in the code examples in this course.

We can do a version of interpolation, which JS refers to as using template litterals. Note those little marks are back-tics and not quotes:

```sh
	> name = "stranger";
	> `Hello, ${name}`
```

You can also combine strings with numbers.

```sh
> 5 + “dollars”;
<* "5dollars"
```

Try these:
```sh
> 16 + 4 + " Bucks!";
<* "20 Bucks!"
> "The answer is " + 21 + 21;
<* "The answer is 2121"
```

JS evaluates expressions from left to right. Different sequences can produce different results!


### Data Type Conversion
In most programming languages, before we combine data types together, we must convert them to match. We can do that in JavaScript. 

Converting to String
```javascript
String(456)

String(100.7 + 23)

(7).toString()
```

Converting to Number

*there is no function toNumber()*
```javascript
Number("98")
Number("3.14159")
Number(" ")
Number("54 42") //Returns NaN which stands for Not a Number!
```

Fortunately for us, JavaScript does a pretty good job of Automatic type conversion!! (null and undefined are both ways to refer to the absence of value. 
```javascript
// 9 + null    // returns 9 - because null is converted to 0
// "9" + null  // returns "9null" - because null is converted to "null"

"5" + 2     // returns 52 - because 2 is converted to "2"
"5" - 2     // returns 3 - because "5" is converted to 5
"5" * "2"   // returns 10 - because "5" and "2" are converted to 5 and 2
```

Sometimes it's a little unpredictable, but it usually does what you want it to :)


### Writing JavaScript Code
In order for us to run JS programs in Chrome, we have to run it through an HTML file.

	* Create a new folder called JavaScript.
	* In that folder create a new file called js_code_along.html
	* Build out your html structure.
	* Open that file in Chrome. 
	* Open up the console.

In your html code, add the script tag to your body:
```html
<!-- conversion.html -->
<body>
   <script src="code_along.js"></script>
</body>
```

In the same JavaScript folder, create a new file called height_to_centimeters.js
 
Once this file is opened, enter the following line of code:
```javascript
console.log(72 * 2.54);
```

Once these changes have been made and you’ve saved your program, refresh your browser to see:

```sh
182.88       code_along.js:1
```
Congrats! You just wrote your first program!

Did you notice the semi-colon?

#### ;
 
In JavaScript (as in many languages), the semi-colon is very important. It denotes the end of an expression and tells the runtime that particular action is over. We will see these at the end of a lot of lines of code (but not all of them!).
 
Do not forget your semi-colons!


### Variables
In JS, variables are memory locations which hold any data used within a given program. Think of it like a container. Let’s try a simple example. Navigate back to your code_along.js  file to enter the following line of code in place of what we entered before:

```javascript
let a = 72;
const pi = 3.14

console.log(a * 2.54);
```

Refresh the page in Chrome, look at the Console, and you will see that it outputs the same value as before, but returning the value from a different line.

In JS, we declare a variable using the var keyword. This creates a new instance of the variable. We can reassign the variable's value later, but when we do, we do not need to use the var key word again.

```javascript
let x = "Aaron is dangerous";
console.log(x);
x = "Aaron is more dangerous than juggling chainsaws...on fire!";
console.log(x);
```

JS is also Dynamically Typed, which means there is no limitation on what a data type variable can store.
```javascript
let theAnswer = 42;
console.log(theAnswer);
theAnswer = "Abe Lincoln";
console.log(theAnswer);
```

Also, variable names must start with a lowercase letter. It is convention to use camel case if it has more than one word. 

Comment out the existing code. Enter the following code into the program via Sublime:
```javascript
let myName = 'John Smith';

let heightInches = 60;

let weightPounds = 120;

let heightCentimeters = heightInches * 2.54;

let weightKilograms = weightPounds * 0.453592;

console.log(myName + ' is ' + heightCentimeters + ' cm and ' + weightKilograms + ' kg.');
console.log(`${myName} is ${heightCentimeters} and ${weightKilograms} kg.`);

```

As you can see, we’ve used a bunch of variables to store different information such as name, height, and weight. We then went on to call these variables back in at later points in the program, making use of the automatic data conversion and concatenation.

Once the program has been called in the Console, the output should be a sentence containing information about the name, height (centimeters), and weight (kilograms) of John Smith. Bear in mind that **console.log** is responsible for this, as it prints out to the console what was passed to it.


### Dynamic Programs
Up until now, we have just been creating static programs. Meaning, if we wanted the outcome of our program to change, we would have to go into our source code and hard code the changes ourselves.

Problem is: that defeats the whole purpose of programming, right? Let's make our programs interactive, by retrieving user responses and using them in our program. 

The best way for us to do this in Chrome is to use a function called **prompt()**. Prompt is a built in function that will pop up an alert box, and allow the user to enter data. The data is then stored in memory for our use in the program! With this we must also have a user prompt, to let the user know exactly what we are looking for. 

```javascript
let deepThought = prompt("What is the answer to life, the universe, and everything?");
```

Navigating back to **height_to_centimeters.js** in Sublime, let’s make use of **prompt()**:
```javascript
let a = prompt("How tall are you in inches?");

console.log(a * 2.54);
```

Refresh your browser, and notice the pop up that now wants your info. Once you enter any value, it saves the value to the variable a and applies it to the rest of the program.
 
**Classroom Challenge:** Edit your code in Sublime to have it output something like this in Console:
```sh
> 60 inches = 152.4 centimeters
```

#### Classroom Challenge:
*Instructor Note: Ask students for input*

Edit your Imperial_to_metric_height code to be fully interactive:
```javascript
let myName = prompt("What is your name?");

let heightInches = prompt("How tall are you in inches?");

let weightPounds = prompt("How much do you weigh in pounds?");

let heightCentimeters = heightInches * 2.54;

let weightKilograms = weightPounds * 0.453592;

console.log(`${myName} is ${heightCentimeters} and ${weightKilograms} kg.`);
```


### Functions
***slide 25*** **[SHOW](http://techtalentsouth.slides.com/techtalentsouth/javascript-jquery-ci?token=QRVJGNY1#/0/25)

We've already used several built in JS functions. It’s important to note that the following can all be considered methods:
	console.log("Hello handsome!");
	prompt("Type your name");
	String(45);
	(45).toString();
	("Hello").length
	("stop yelling...").toUpperCase();

That’s right! All of these are considered functions . This is because they all must be applied to some sort of object (i.e., argument ) in order to produce any meaningful output.

A JS function is a block of code designed to perform a particular task. A function is executed when "something" invokes it (calls it).

A function allows us to package many lines of code, and perform those lines in one simple call. We can use this short call to perform the function over and over again, without us writing the code ourselves. This helps keeps things DRY!

*Instructor Note:
Make sure the student understand that...
javascript:function::ruby:method
*

While there are many built in/native functions, we can also build any custom function that we want.

```javascript
function greeting() {
	console.log("Hello Zack!");
}

greeting();

// fat arrow function version

greeting = () => {
	console.log("Hello Zack!")
}

// we call it the same way
greeting()
```

We use the function keyword, followed by the functions name and a pair of parenthesis. This is also followed by curly brackets. Whatever is written inside of these brackets is performed when the function is called. 

Unlike many other programming languages, we can pass variables into the function.

```javascript
let myName = "Zack";

function greeting() {
	console.log("Hello " + myName);
}

greeting = (name) => {
	console.log(`Hello ${myName}`)
}

greeting(myName);
```
This is not recommended however.
*Instructor Note: just as using a global (@) variable into a Ruby method is possible, but not recommended.*

What is recommended is passing data into the function via parameters/arguments.

```javascript
let myName = "Zack";

function greeting(name) {
	console.log("Hello " + name);
}

greeting(myName);
```

In the function name, we have an parameters inside of the parenthesis. This acts as a variable. Then when we call the function, we must pass in data through that argument slot. 

We can have as many arguments as you want. Though they must be entered in order.

```javascript
let myName = prompt("What is your name?");
let newLang = prompt("Do you know a language besides English?");
let welcomeSaying = prompt("How do you greet others in that language?")

function greeting(name, language, saying) {
	console.log(`${saying} ${name}, nice to speak with someone who knows ${language}`);
}

greeting(myName, newLang, welcomeSaying);
```

A function can also "return" a value.
```javascript
function addItUp(x,y) {
  let z = x + y;
  return z;
  // or simply:
  // return x + y;
}
```

Note: We will have to call return here...it is not implicit like in Ruby. You can then assign the call of that method to a variable:
```javascript
let num = addItUp(40,2);

// you can also still call it
// within console.log() or alert()
```

Change your **imperial_to_metric_height code** and use a function to help you there!

```javascript
function convertToCentimeters(number) {
    return number * 2.54
}

let myName = prompt("What is your name?");

let heightInches = prompt("How tall are you in inches?");

let weightPounds = prompt("How much do you weigh in pounds?");


let weightKilograms = weightPounds * 0.453592;

console.log(`${myName} is ${convertToCentimeters(heightInches)} cm and ${weightKilograms} kg.`);
```

Again the curly brackets open and close our function. You can also see that we named the function **convertInchesToCentimeters**, again using camel-casing. We included number as our argument. Finally, we called the function that we created back into the last few lines of the program passing in a variable we create earlier, heightInches. This variable is retrieving user input via the prompt function!


### Conditionals
*Instructor Note: We know this stuff from Ruby... there's just curly brackets involved now*

Sometimes, we want JS to perform an action only if  a certain condition is met.

```javascript
let num = 45;

if ( num < 50 ) {
    console.log("Less than half.");
}
```

In addition to if , you will also usually tell the program what else  to do:

```javascript
let num = 45;

if ( num < 50 ) {
    console.log("Less than half.");
} else if {
    console.log("More than half way there.");
} else {
    console.log("That ain't half bad!");
}
```


*### JS Activity: MadLibs
Instructor Note: only do if you think there's enough time*

*Create a Mad Lib Program!
	* Have at least 10 inputs
	* Include at least one number*

***Example**
Please enter the following.
Exclamation: Holy Bananas
Name: Zack
Verb: jump*

*Holy Bananas! Is that Zack? Wow, they sure do love to jump!*

```javascript
alert("Please enter the following...");
var excl = prompt("Exclamation");
var name = prompt("Name");
var verb = prompt("Verb");

console.log(exc + "! Is that " + name "? Wow, they sure do love to " + verb + "!");
```

*Create your own story, or steal one from the internetz. *


### HTML Functions
Let's move out of the console. JavaScript has built-in functions that let us modify HTML & CSS.

First, we need to establish which HTML element we want to affect.
```javascript
document.getElementById()

document.getElementsByClass()

document.getElementsByTagName()
```

Notice how the last two address "Element s "?
That's why it's best to just work with IDs!

### HTML Functions
***slide 34*** **[SHOW](http://techtalentsouth.slides.com/techtalentsouth/javascript-jquery-ci?token=QRVJGNY1#/0/34)

Once you select something to change, we have to figure out what to do with it.

```javascript
//add or replace HTML text inside an element with a certain ID
document.getElementById("div1").innerHTML = "Something changed here...";

//add a class to an element with a certain ID
document.getElementById("div2").className = "nice-div";

//add an ID to an element with a certain ID
document.getElementById("div3").id = "cool_id";
```


### jQuery Intro & Set-Up
jQuery is a JavaScript library...
It sits on top of the existing JavaScript language and reduces the amount of code we have to write when we want to use basic functions.

Let's practice using jQuery in a new space...

**Step 1:** Create a new folder called jQuery. In that folder, let's create an index.html page.
 
**Step 2:** Inside index.html, build out the basic structure of a HTML page - include the DOCTYPE, a title, a head and a body.
 
**Step 3:**
In your <head>, create a link to an external stylesheet called style.css, then create that file in your folder. 
Next create a JavaScript file called site.js. Link that script file and our HTML file together, and then embed the jQuery library into our file via a CDN. 

Note: JavaScript is typically included at the bottom of your HTML page.  The reason for this is we want our HTML to load before our JavaScript loads, because again, our JS is manipulating our HTML.

```html
<!-- index.html -->
<body>
    <!-- Lots of HTML. Oh yeah! -->   
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.1.1/jquery.min.js"></script>
    <script src="site.js"></script>
</body>
```

**Step 3 Explained:**
The first line is pulling in the library of jQuery code from jQuery’s website and letting us use it in our site! Hey, thanks!

The second line performs virtually the same function as our <link> element to our stylesheet - after our site pulls in the jQuery library, it will look to our site.js file for any scripts that we write and pull them in so that whatever we write in our script file will function in our webpage, exactly like how when we write CSS in our style.css file.

#### HTML/CSS Challenge
*Instructor Note: Give about 2-5 minutes for students to complete*

	* Create two red squares and one green circle. 
 
	* You should have a "square" class and a "circle" id.
	
	* in between the squares, enter a line of text in a ```<p>``` tag.
 
	* Big Hint: use the border-radius property.


### Click & Hover Functions
Now that you've got your shapes, go into your script file and type in the following: 
```javascript
$('#circle').click(function() {
 alert('Clicked!'); 
});
```

$ - “string” character. A sign to define/access jQuery

(“#circle”) - our “selector”. This selects what we’ll be applying a function to

.click(function() - tells jQuery when it should apply the function, in this case when the user clicks

{alert(“Click!”);}) - the “function”, or what we actually want to happen - in this case, fire a pop-up alert with the text “Click!”

Now try to select one of your squares instead of your circles, refresh and see if your script still works!

Now, instead of creating an alert when you click on a shape,
let’s change some text.  In your HTML file, comment out your squares and put in a paragraph element with a short sentence, like...
```html
<p>”This is a sentence!”</p>
```

Back in your site.js file, we’re still going to use the top line of our code - selecting our circle and performing a function when the user clicks on it - but we’re going to change what that function is. Let's replace the alert...
```javascript
$('#circle').click(function() {
  $('p').html(“We’ve changed the text!”);
});
```
*show including both into one function. Then put back to two bits of jQuery*

What if we want the text change when we hover over the circle? We would simply change click to hover on Line 1! It's that easy!

Changing some text is cool, but we can change more stuff too... like a HTML attribute. Insert an image under our <p> tag, remembering to include the “src=” attribute. Then, in our script file, replace Line 2 with this:
```html
$('img').attr('src', '...');
```
Replace that ellipsis with the URL of another image. Now refresh your page and try it out!

Now, we’ll actually work with the styles of our elements. Let’s bring back our squares in the HTML file and get rid of our paragraph and images. Use jQuery to change the squares from red to blue when clicked:
```javascript
$('.square').click(function() {
  $(this).css('background-color', 'blue');
});
```

**this**? What does that mean?
*this *is a JavaScript function that simply means the object that’s been referenced immediately before (so, in this case, 'div'). ```this``` does not play well with fat arrow functions

#Not sure what ```this``` is refering to?  ```console.log(this)```

The CSS function is just like the attr function we used a minute ago, with a CSS attribute/value pair within the parantheses.


#### jQuery Challenge
*Instructor Note: give students 2-5 minutes to complete*

Without the instructor showing you what to do, I’d like you to change the width of the squares to 400px after the user hovers over them.

```javascript
$('.square').hover(function() {
  $(this).css('width', '400px');
)}
```


### More jQuery Functions

.css() adds the CSS rules into a style attribute for the specified HTML tag, which we learned is not optimal. Why not just add and remove pre-made CSS classes?

**.addClass()**, **.removeClass()**
Take a parameter of a class (no dot/period needed)

**.toggleClass()**
Takes same parameter as above,
but will do both on the same clickable ID

```javascript
$('.square').hover(function() {
	$(this).toggleClass('blue');
	console.log(this);
});
```

**.val()**
Can pull or set the value from a specified input field
 
**.append()**, **.prepend()**
Adds to text rather than replace (unlike .html() or .text() )
 
**.after()**, **.before()**
Similar to append/prepend, but adds before/after
the selector, rather than within

**.hide()**, **.show()**
Essentially adds and takes away CSS rule of display: none;
 
**.fadeIn()**, **.fadeOut()**
Same as hide/show, but not so immediately
(you can set the rate in milliseconds as a parameter)

**.fadeToggle()**
Combines fadeIn/fadeOut into one clickable

**.fadeTo()**
Does fadeOut, but to a certain opacity
(given as a second parameter)

**.slideUp()**, **.slideDown()**
Once again, a twist on hide/show

**.slideToggle()**
Combines slideUp/slideDown into one clickable



### jQuery UI
jQuery UI is the official home to a lot of great pre-built plugins that save lazy people like you and me some time. There are two files we need to import to be able to use plugins from the UI:
```html
<link rel="stylesheet" href="https://code.jquery.com/ui/1.11.4/themes/smoothness/jquery-ui.css">

<script src="https://code.jquery.com/ui/1.11.4/jquery-ui.min.js"></script>
```

The link tag should go in your <head> underneath your CSS link. Your script should go just before your own script file
at the bottom of the <body>.

Now that we’ve imported these two files, we can use special classes and functions to give us even more possibilities. UI is a lot like Bootstrap, in that it uses pre-built styling and custom classes.

#### Draggable
To make an element, like a <div>, draggable, it only takes a few lines of code now that we’re using the jQuery UI library. 
Create a <div> with the ID **“draggable”** and the class **“ui-widget-content”**. Inside that <div>, create a paragraph with the sentence “Drag me around."
In your script file, we’re going to select the div by using its ID and call the function **draggable**.

```javascript
$(function() {
  $('#draggable').draggable();
  })
```

Notice that we aren’t telling the function when to activate (click, hover, etc.). We skip the whole “selector” process and go straight to the “function”. This is because we want the script to be running all the time. 

#### Resizable
We only have to change a few things to turn our <div> from **draggable** to **resizable**! What do you think we should do?

```javascript
$(function() {
  $('#resizable').resizable();
  })
```

And don't forget to add the "resizable" id to an HTML element on index.html!

#### Challenge: Sortable
See if you can implement this!
Visit [https://jqueryui.com/sortable/](https://jqueryui.com/sortable/), and click 'view source' to see how to implement.
