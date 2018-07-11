# CSS: How to make HTML look good
CSS stands for Cascading Style Sheets. CSS is responsible for describing how elements should be displayed on a page by overriding your browsers default styling.

## Useful Resources
[CSS Demo](http://www.w3schools.com/css/css_intro.asp) - multiple style sheets for same page 
[CSS Documentation](https://developer.mozilla.org/en-US/docs/Web/CSS)  
[CSS Selector Reference](https://developer.mozilla.org/en-US/docs/Learn/CSS/Introduction_to_CSS/Selectors) - a list of common properties
[CSS Diner](https://flukeout.github.io/) - good game to test skills
[CSS Selector Demo](https://www.w3schools.com/cssref/trysel.asp)
[CSS Properties index](https://meiert.com/en/indices/css-properties/)
[CSS Validation Service](http://jigsaw.w3.org/css-validator/)

---

1. What is CSS?
    1. What do we mean by Cascading?
    2. Inline Styling
    3. Internal Styles using the `<style>` tag
    4. External Styles using the `<link>` tag
    5. Why a separate file?
2. CSS Syntax and Selectors
    1. Simple Selectors
    2. Attribute Selectors
    3. Pseudo-class Selectors
    4. Combinators and Multiple Selectors
3. CSS Properties
4. Exercises
    1. [Basic CSS Styling](exercises/basic_css_styling/)
    2. [Travel Site](exercises/travel_site/)
    3. [Sign up form](exercises/sign_up_form/)
6. Homework

---

## What is CSS?
Let's learn what CSS (Cascading Style Sheets) are exactly.

* CSS is the language for describing the presentation of Web pages, including colors, layout, and fonts using a set of rules. 
* It allows one to adapt the presentation to different types of devices, such as large screens, small screens, or printers. 
* CSS is independent of HTML and can be used with any XML-based markup language.

### What do we mean by Cascading?
Cascading through CSS sources and
Cascading from general to more specific
"Cascading" in context of CSS means that because more than one stylesheet rule could apply to a particular piece of HTML, there has to be a known way of determining which specific stylesheet rule applies to which piece of HTML.

The rule used is chosen by cascading down from the more general rules to the specific rule required. The most specific rule is chosen.

Lets build out a file structure like yesterday:

day2
code_along_1
    index.html
    assets
        stylesheets
            styles.css
        images
        javascripts
        
In the index.html add the following:

    *  !Doctype prefilled for you.
    *  Add some elements with some dummy content: an <h1> tag, 3 <h2> tags, and a <p> tag.
    *  You can auto-fill some dummy text by typing lorem and then hitting the tab key.
    
Have students open in their browser.

### Inline Styling
We can actully write style inline with our HTML tags. This is done using the style attribute. Although this is not best practices it can certainly come in handy when trying to test things out. It would be wise to move any inline styling to a separate style sheet before making it live in production.

**Inline Style Example**  
```html
  <h1 style="text-align: center;">Look Here!</h1>
  <p style="height: 100px; color: red;">This is inline CSS</p>
```

### Internal Styles using the `<style>` tag
This is really a compromise between inline styles and a separate style sheet. You can use a style tag in the `<head>` or `<body>` tag. Keeping it in the head tag is best practices without a solid use case. 

**Internal Style Example**  
```html
  <head>
    <style>
      h1 {
          height: 200px;
          background-color: black;
          color: white;
      }
    </style>
  </head>
```

When writing CSS you are really creating rules. Each rule consists of a selector and a declaration block. The declaration block is is wrapped in curly braces `{}`. Each declaration and its value are separated by a colon (**:**) and end with a semi-colon (**;**)

The syntax or recipe for how this works looks like this:
 
**CSS Rule Example**  
```css
  h1 {
    height: 200px;
    background-color: black;
    color: white;
  }
```

### External Styling **using the `<link>` tag**
The `<link>` tag facilitates making your external style sheet available to your html file. The `<link>` tag should be placed in the `<head>` tag of your site. You should include your style sheets in order from generic to specific.

**Basic anatomy of the link tag `<link>`**  
There sevaral attributes for a `<link>` tag. Lets go over a few of the common ones.  

* **type** - The common use of this attribute is to define the type of style sheet linked and the most common current value is text/css, which indicates a Cascading Style Sheet format.
* **rel** - This lets the browser know what type of file we are linking. In this case it will be stylesheet. [Further Reading](https://developer.mozilla.org/en-US/docs/Web/HTML/Link_types)
* **href** - This attribute specifies the URL of the linked resource.

*[Further Reading on the `<link>` tag](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/link)*

### External Styling
**`<link>` Example**
```html
  <head>
    <!-- reference to your CSS page -->
    <link type="text/css" rel="stylesheet" href="assets/stylesheets/sweet-styles.css" />
  </head>
```
**?? Why would we want an external style sheet?**

### External Styling Why a separate file?
Although we can write CSS in our HTML pages directly via the `<style>` tag and using inline styles, it is best practices to keep you CSS in its own file with the extension **.css**.

***Benefits of a separate CSS file***  

* We can apply the same styling to multiple HTML pages
* Keeps our site more organized. We know where to find our CSS quickly.
* Keeps you from writing the same code over and over again.

show bookmark for:
[CSS demo: 1 page- multiple styles](https://www.w3schools.com/css/css_intro.asp)

***Discussion Topics***  
*Re-enforce Separation of Concerns and DRY (Don't Repeat Yourself) concepts.*
*Show some different browsers default styling*

## CSS Syntax and Selectors
 
 _Material from this slide covered earlier._

* **h1** - represents the selector
* **height, background-color, and color** - represents the declarations

### Simple Selectors
Simple selectors directly match one or more elements of a document, based on the type of element (or its class or id). These are the selectors you will use most often. 

class and id names can be anything you want, but usually good to use something descriptive

[Further Reading on Simple Selectors with activities](https://developer.mozilla.org/en-US/docs/Learn/CSS/Introduction_to_CSS/Simple_selectors)

**Simple Selector Examples** 
```css

  /* targeting an element by it's name */
  h1 {
    ...
  }

 /* targeting an element by a given class (class="myClass") */
  .myClass {
    ...
  }

  /* targeting an element by a given id (id="myId") */
  #myId {
    ...
  }

  /* The Universal Selector. Select all the things */
  * {
    ...
  }

```

### Attribute Selectors
Attribute selectors are a special kind of selector that will match elements based on their attributes and attribute values.

[Further Reading on Attribute Selectors with activities](https://developer.mozilla.org/en-US/docs/Learn/CSS/Introduction_to_CSS/Attribute_selectors)

* still need html to go with example below
**Common Attribute Selector Examples** 
```css

  /* All elements with the attribute "href"
   are bolded */
  [href] {
    font-weight: bold;
  }

  /* All elements with the attribute "href"
     with the exact value "#" are 2x as big */
  [href="#"] {
    font-size: 2rem;
    color: white;
  }

```

show [Attribute selector rules](https://www.w3schools.com/css/css_attribute_selectors.asp)

***Discussion Topics***  
*Talk about substring value arrtibute selectors and encourage students to play with the different selectors on their own.*

### Pseudo-class Selectors
CSS pseudo-class is a keyword that represents the state something may be in. It is added to a selector separated by a colon (**:**). It allows you to style selected elements only when they are in certain state.  

[Further Reading on Pseudo-class Selectors with activities](https://developer.mozilla.org/en-US/docs/Learn/CSS/Introduction_to_CSS/Pseudo-classes_and_pseudo-elements)

**Common Attribute Selector Examples** 
```css

  /* Change what a link should look like when hovered over with the mouse */
  a:hover {
    color: darkred;
    text-decoration: none;
  }

  /* Change what a link should look like when its been visited */
  a:visited {
    color: blue;
    text-decoration: none;
  }

```


***Discussion Topics***  
*Talk about Pseudo-element selectors and encourage students to play with the different selectors on their own.*

Basically a pseudo-class is a selector that assists in the selection of something that cannot be expressed by a simple selector, for example :hover . A pseudo-element however allows us to create items that do not normally exist in the document tree, for example `::after`.

### Combinators and Multiple Selectors
This can get complicated pretty fast. Let's touch on the basics for now. combinators and multiple selectors save you time by allowing you to style multiple elements at once. They also let you get very specific on how they are targeted.

[Further Reading on Combinators and Multiple Selectors with activities](https://developer.mozilla.org/en-US/docs/Learn/CSS/Introduction_to_CSS/Pseudo-classes_and_pseudo-elements)

**Simple Combinators and Multiple Selectors examples**
```css

  /* target all <p> tags within a <div> and make the text red
  div p {
    color: red;
  }

  /* target all <div> and <p> tags on the page and make their text blue */
  div, p {
    color: blue;
  }

```


## CSS Properties 1
***slide [13](http://techtalentsouth.slides.com/techtalentsouth/css-basics-code-immersion?token=EmxRE2wf#/0/13)***

There a ton of CSS properties; 370 and counting. Don't be scared, we'll only be using about 50 in this class. Auto-fill will help alot w both props and values.

***Common Properties***
[Common Property Reference](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Properties_Reference)

* **background** - Control all things related to the background including but not limited to color and image
* **color** - Controls the color of the text within an element. **SHOW** [CSS Color Reference](http://www.w3schools.com/cssref/css_colors.asp)

## CSS Properties 2
***[slide 14](http://techtalentsouth.slides.com/techtalentsouth/css-basics-code-immersion?token=EmxRE2wf#/0/14)***

* **font-weight** - controls the boldness of the font. Common values are normal and bold
* **font-size** - specifies the size of the font. Usually in pixes or em units. 

SHOW [CSS Font Size Reference](https://developer.mozilla.org/en-US/docs/Web/CSS/font-size)
* **font-family** - specifies the font to use. 

SHOW [CSS Web Safe Font Reference](http://www.w3schools.com/cssref/css_websafe_fonts.asp)

### CSS Properties 3
* **height** - controls the height of the content area of an element 
* **width** - controls the width of the content area of an element
Use px, em, rem, or %

### CSS Properties 4

The box model:

By default width is setting content width; padding, border and margin are all added onto the width value.
can use **box-sizing** property with a value of **border box** and it will make the width property include the padding, border and margin.
* **padding** - sets the padding space on all sides of an element. 
* **margin** - sets the margin on all sides of an element. 
* **border** - sets the border on all sides of an element. 

### End of Slides


***PRO TIP***  
*https://developer.mozilla.org/en-US/docs/Web/CSS/YOUR_PROPERTY_HERE - Replacing YOUR_PROPERTY_HERE with something like 'color', 'background', or any other property you can think of should bring you to documentation on that specific property*

### *Discuss [Custom Fonts with Google Fonts](https://fonts.google.com/)*

Select your font and it will generate a link tag to put in the '<head>' of your HTML and give you a declaration to put in your CSS.

### Cascading Challenge
    ```
    <div id = "the-id" class = "class-one class-two"> </div>

    #the-id {
        color: pink;
    }
    /* complex simple selector : ( */
    div.class-two {
        border: 1px solid;
    }

    .class-one {
        width: 100px;
        height: 100px;
        color: blue;
        border: 5px dotted red;
    }

    .class-two {
        height: 50px;
    }

    ```
### Class activity 1

    Travel Site

### Class activity 2

    Log-in Form

##Homework

***Easy***

![pie-chart.png](resources/images/pie-chart.png)

**Instructions**  

1. Build out the pie chart seen in the image using only html and css (no images!)
2. Set the background color of each section to the color it is labeled

---

***Medium***

![masthead.png](resources/images/masthead.png)

***images for challenge***  
[cart-icon.png](resources/images/cart-icon.png)  
[search-icon.png](resources/images/search-icon.png)  

**Instructions**  

1. Build out the masthead seen in the image masthead.png
2. Use the 'cart-icon.png' and 'search-icon.png' images; everything else should just be html and css

<!--stackedit_data:
eyJoaXN0b3J5IjpbNTMyOTgzMTM1XX0=
-->
