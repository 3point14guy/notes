# Exercise - Create and style a sign-up form

![Completed Exercise Image](../../resources/images/css_form_completed.png)

***PRO TIP***  
*You may want to check out the [Mozilla](https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/Forms/My_first_HTML_form) or [W3](http://www.w3schools.com/html/html_forms.asp) documentation on how to create a form*

**The HTML**

1. Lets add the following html to the file so we have our basic page structure
```html
  <!DOCTYPE html>
  <html>
  <head>
    <title>Sign-up Form</title>
  </head>
  <body>

  </body>
  </html>
```
2. Inside the `<body>` create a container `<div>` with the `id` attribute of login-box
3. Within the login-box `<div>`, create another `<div>` with a `class` of left
4. Inside of the 'left' `<div>`, create a top-level header that contains the text: 'Create Account' 
```html
  <!DOCTYPE html>
  <html>
  <head>
    <title>Sign-up Form</title>
  </head>
  <body>

    <div id="login-box">
      <div class="left">
        <h1>Create Account</h1>
      </div>
    </div>

  </body>
  </html>
```
5. Inside the `<div class="left">`, Create a `<form>` element which contains:
    * A `class` attribute with a value of ***sign-up*** 
    * An `action` attribute with a value of ***index.html*** 
        * The `action` attibute defines the location (a URL) where the form's collected data would be submited.
    * A `method` attribute with the value of ***POST***
        * The `method` attribute defines which HTTP method to send the data with (it can be "GET" or "POST").
```html
    <div id="login-box">
      <div class="left">
        <h1>Create Account</h1>
        <form class="sign-up" action="index.html" method="POST">
        </form>
      </div>
    </div>
```
6. Create 4 `<input>` elements within the `<form>`
    * A username `<input type="text" name="username" placeholder="username">`
    * An Email `<input type="text" name="email" placeholder="email">`
    * A password `<input type="text" name="password" placeholder="password">`
    * A confrim_password `<input type="text" name="confirm_password" placeholder="re-type password">`
7. Create a `<button>` inside the `<form>` under your `<inputs>` elemtents with the following attributes:
    * A `class` attribute with the value ***sign-up-button***
    * A `type` attribute with the value of ***submit***
    * e.g. `<button class="sign-up-button" type="submit">Sign Me Up</button>`
```html
    <div id="login-box">
      <div class="left">
        <h1>Create Account</h1>
        <form class="sign-up" action="index.html" method="POST">
          <input type="text" name="username" placeholder="username">
          <input type="text" name="email" placeholder="email">
          <input type="text" name="password" placeholder="password">
          <input type="text" name="confirm_password" placeholder="re-type password">
          <button class="sign-up-button" type="submit">Sign Me Up</button>
        </form>
      </div>
    </div>
```
8. Create a `<div>` with a `class` attribute of ***right*** below the `<div class="left">` closing tag but still inside the `<div id="login-box">`
9. Create a `<span>` with the `class` attribute value of ***loginwith*** inside of the `<div class="right">` 
10. Inside the `<span>` put the text: ***Sign in with Social Network***
11. Inside the `<div class="right">`, Create 3 `<button>` elements with the `class` attribute value of ***social-signin***
12. Add an additonal `class` attribute value of ***facebook*** to the first button and include the text ***Log in with Facebook***
13. Add an additonal `class` attribute value of ***twitter*** to the second button and include the text ***Log in with Twitter***
14. Add an additonal `class` attribute value of ***google*** to the first button and include the text ***Log in with Google+***
15. Do the same thing for Twitter and Google+
```html
    ...
        </form>
      </div>
      <div class="right">
        <span class="loginwith">Sign in with Social Network</span>
        <button class="social-signin facebook">Log in with Facebook</button>
        <button class="social-signin twitter">Log in with Twitter</button>
        <button class="social-signin google">Log in with Google+</button>
      </div>
    </div>
```
16. Create a `<div>` with the `class` attribute value of ***or*** and the place the text ***OR*** inside the `<div>`. This should be placed below the `<div class="right">` closing tag but still inside the `<div id="login-box">`
```html
    ...
        <button class="social-signin google">Log in with Google+</button>
      </div>
      <div class="or">OR</div>
    </div>

```

**The CSS**  

1. Lets make default broswer styling a little better with normalize.css
  *  Add the following link to normalize.css to the bottom your `<head>` tag 
      * `<link rel="stylesheet" type="text/css" href="https://necolas.github.io/normalize.css/5.0.0/normalize.css">`
```html
  ...
    <title>Sign-up Form</title>
    <link rel="stylesheet" type="text/css" href="https://necolas.github.io/normalize.css/5.0.0/normalize.css">
  </head>
```
2. Create a css file called ***sign_up.css*** in your project directory with the following comments pasted in
3. Complete each step from top to bottom.
    * Refresh after each step to see what changes
    * Don't forget to use a semi-colon `;` between each rule 
    
```css
/*
Set the box sizing for every element on the page using the * selector.
When you set box-sizing: border-box; on an element, the padding and border of that element no longer increase its width.
*/


/*
using the :focus pseudo selector for the sign-up-button apply the following styles
  - set the outline to none
*/



/*
Bring in the Noto Sans font using @import url() syntax
 - url: https://fonts.googleapis.com/css?family=Noto+Sans|Comfortaa:400,300,700
*/



/*
Set some common styles for the body of the page using the body selector
 - set a background color of `#DDD`
 - set the text color to `#222`
 - set the font fmaily to be 'Noto Sans'
 - set the font wieght to 300
*/



/*
Style the element with the class 'login-box'
Useful link https://www.w3schools.com/cssref/css3_pr_box-shadow.asp
 - set its position to relative
 - set the top and bottom margin to 5%
 - set the left and right margin to auto
 - set the background color to '#FFF'
 - set the width to 600px
 - set the height to 400px
 - set the border radius to 2px
 - give the box a box-shadow with the following values:
   - horizontal shadow of 0
   - vertical shadow of 2px
   - blur of 4px
   - color of rgba(0,0,0,0.4)
*/



/*
Style the div with the class of left
  - set the padding to 40px
  - set the width to 300px
  - set the height to 400px
  - set the position to absolute 
  - place it at the top left of its parent
*/



/*
Style the div with the class of right
  - set the padding to 40px
  - set the width to 300px
  - set the height to 400px
  - set the position to absolute 
  - place it at the top right of its parent
  - set the background to url('https://goo.gl/YbktSj');
  - set the background size to cover
  - set the background position to center
  - set the the bottom and right border radius to 2px
*/



/*
Style the h1 element
 - set the bottom margin to 20px
 - set the font weight to 300
 - set the font size to 2em
*/

/*
Style the <input> element
 - remove the border
 - set a solid bottom border of 1px with a color of '#AAA'
 - set the display to block
 - add a bottom margin of 20 px
 - padding of 4px
 - set a width of 220px
 - set a height of 32px
 - set a font weight of 400px
*/



/*
target the button with the class of sign-up-button
 - give a top and bottom margin of 5px
 - set a width of 220px
 - set a height of 32px
 - remove the border
 - set the border radius of 2px
 - set the background color to #16A085
 - set the text color to #FFF
 - make the text uppercase
 - set the font wieght to 400
*/



/*
using the :hover pseudo selector for the sign-up-button apply the following styles
  - set the opacity to 0.8
  - set the box shadow to '0 2px 4px rgba(0, 0, 0, 0.4)'
  - set the transition to '0.1s ease'
  - 
*/



/*
using the :active pseudo selector for the sign-up-button apply the following styles
  - set the opacity to 1
  - set the box shadow to '0 1px 2px rgba(0, 0, 0, 0.4)'
*/


/*
Style the elemet with loginwith class
  - set display to block
  - set the bottom margin to 40px
  - set the font size to 2em
  - set the text color to #FFF
  - make the text centered
*/



/*
style the <button> elements
 - set the bottom margin to 20px
 - set the width to 220px
 - set the height to 36px
 - remove the border
 - set the border-radius to 2px
 - set the text color to #FFF
*/



/*
Target the button with the class of facebook
 - set the background color to '#32508E'
*/



/*
Target the button with the class of twitter
 - set the background color to '#55ACEE'
*/



/*
Target the button with the class of google
 - set the background color to '#DD4B39'
*/



/*
Target the element with a class of 'or'
 - set the position to absolute
 - position it 180px from the top
 - position it 280px from the left
 - set the width and height to 40px
 - set the background color to #DDD
 - give a border radius of 50%
 - align the text center
 - set the line height to 40px
 - set the box shadow to '0 2px 4px rgba(0, 0, 0, 0.4)'
 - 
*/
```
3. Yay! you're all done.