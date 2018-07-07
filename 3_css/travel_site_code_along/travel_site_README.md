# Exercise: Travel Site
Following this example will help us take our travel site and turn it into a modern looking website with the help of CSS

![Completed Exercise Image](../../resources/images/travel_site_complete.jpg)

***images for exercise***  
[spain_flag.svg](../../resources/images/spain_flag.svg)  
[italy_flag.svg](../../resources/images/italy_flag.svg)  
[aruba_flag.svg](../../resources/images/aruba_flag.svg)  
[chile_flag.svg](../../resources/images/chile_flag.svg)  
[travel_hero.jpg](../../resources/images/travel_hero.jpg)  

1. Create the following directory structure on your desktop
    * If you are having trouble doing this from the command line, please revisit [CodeCademy - Learn the command-line](https://www.codecademy.com/learn/learn-the-command-line)  
```
css_travel_site
|
│   index.html  
|
└───assets
    │
    └───images
        |
        │   spain_flag.svg
        |   italy_flag.svg
        |   aruba_flag.svg
        |   chile_flag.svg
        |   travel_hero.jpg
        |
        javascripts
        |
        stylesheets
        |
        │   custom.css
        |
```


**Setup your files** 

2. Open index.html and create the basic html structure for a page.
4. Add the following code to your `<body>` tag
```html
<!DOCTYPE html>
<html>

    <head>

        <link type="text/css" rel="stylesheet" href="assets/stylesheets/custom.css"/>

        <title>Amazing Travel Site</title>

    </head>

    <body>

        <div id="masthead">

            <div id="logo">
                <a href="/home.html">My Travel Site</a>
            </div>
            
            <div id="navbar">
                
                <ul id="nav-list">

                    <li class="nav-link">
                        <a href="/home.html">Home</a>
                    </li>

                    <li class="nav-link">
                        <a href="/about.html">About</a>
                    </li>

                    <li class="nav-link">
                        <a href="/stuff.html">Stuff</a>
                    </li>

                    <li class="nav-link">
                        <a href="/contact.html">Contact</a>
                    </li>

                </ul>

            </div>

        </div>

        <div id="content">

            <div id="hero"></div>

            <div class="header-statement">

                <h1>Welcome to my travel site</h1>

                <p>This site is about places I would like to travel</p>

                <h2>Here are some places I would like to visit</h2>
            </div>

            <div id="cards" class="page-row">

                <div class="card" id="spain">
                    <h3>Spain</h3>
                    <a href="http://www.spain.info/en_US/" target="_blank">Spain Tourism Site</a>
                </div>

                <div class="card" id="italy">
                    <h3>Italy</h3>
                    <a href="http://www.italia.it/en/home.html" target="_blank">Italy Tourism Site</a>
                </div>

                <div class="card" id="aruba">
                    <h3>Aruba</h3>
                    <a href="http://www.arubatourism.com/" target="_blank">Aruba Tourism Site</a>
                </div>

                <div class="card" id="chile">
                    <h3>Chile</h3>
                    <a href="http://chile.travel/en/" target="_blank">Chile Tourism Site</a>
                </div>

            </div>

            <div class="page-row" id="travel-table">
                <h3>Some information about the places I would like to visit</h3>
                <table>
                    <tr>
                        <th>Destination</th>
                        <th>Capital</th>
                        <th>Official Language</th>
                        <th>Population (aprox.)</th>
                        <th>National Bird</th>
                    </tr>
                    <tr>
                        <td>Spain</td>
                        <td>Madrid</td>
                        <td>Spanish</td>
                        <td>46.77 million</td>
                        <td>Eagle</td>
                    </tr>
                    <tr>
                        <td>Italy</td>
                        <td>Rome</td>
                        <td>Italian</td>
                        <td>59.83 million</td>
                        <td>Bluebirds</td>
                    </tr>
                    <tr>
                        <td>Aruba</td>
                        <td>Oranjestad</td>
                        <td>Dutch</td>
                        <td>102,911</td>
                        <td>Aruban Burrowing Owl</td>
                    </tr>
                    <tr>
                        <td>Chile</td>
                        <td>Santiago</td>
                        <td>Spanish</td>
                        <td>17.62 million</td>
                        <td>Andean condor</td>
                    </tr>
                </table>
            </div>

        </div>

        <div id="footer">

            <div id="copyright">&copy; Travel is fun</div>

            <div id="social-links">

                <a href="http://facebook.com">Facebook</a>

                <a href="http://twitter.com">Twitter</a>

            </div>

        </div>

    </body>

</html>
```
5. Link your stylesheet to your html file using the `<link>` tag
```html
    <head>
        <link rel="stylesheet" type="text/css" href="assets/stylesheets/custom.css">
    </head>
```


**Exercise Instructions**  
Complete the steps below to the best of your ability using what you already know about HTML and CSS.

1. Open custom.css and paste in the following code
2. Add the relevent CSS rules below each comment to complete the task
```css
/*
Set the box sizing for every element on the page
to border-box. Elements should have padding and 
border included in the element's total width and height
*/



/*
Set some common styles for the body of the page
 - backgroud color to black
 - text color to white
 - font to helvetica
 - font size to 16px
*/



/*
override our h1 style to have a font size of 32px
*/



/*
override our h2 style to have a font size of 28px
*/



/*
style the element with the id of header-statement to have centered text
*/



/*
style the elements with a class of page-row
 - width of 100%
 - clear: both; //ensures elements that are floated cant be on the left or right of this element
 - give the element a padding of 5%
 - give the element a margin of 2% on the top and bottom
*/



/*
style the elements with a class of card
 - width of 20%
 - height of 300px
 - margin of 2.5%
 - top padding of 15%
 - float cards to the left
 - background color of white
 - align the text to the center
 - make the text color black
 - set the background size to contain //Scale the image to the largest size such that both its width and its height can fit inside the content area
 - set the background to not repeat
*/

 
/*
style the element with the id of hero
 - width of 100%
 - height of 450px
 - set the background size to cover //Scale the background image to be as large as possible so that the background area is completely covered by the background image.
 - set the background to not repeat
 - set the background image to our hero image (travel_hero.jpg) in our assets/images directory
*/



/*
 target the element with the id of spain
 set the background image to (spain_flag.svg) in our assets/images directory
*/



/*
 target the element with the id of italy
 set the background image to (italy_flag.svg) in our assets/images directory
*/
 


/*
 target the element with the id of aruba
 set the background image to (aruba_flag.svg) in our assets/images directory
*/
 


/*
 target the element with the id of chile
 set the background image to (chile_flag.svg) in our assets/images directory
*/


/*
 target the element with the id of masthead
  - width of 100%
  - height of 100px
  - line height of 100px
*/
 

/*
 target the element with the id of logo and float it to the left
*/
 


/*
 target the element with the id of nav-links and float it to the right
*/
 


 /*
 target the element with the id of nav-list
  - remove all margin and padding
  - remove bullets from the unorder list
*/


/*
 target the elements with the class of nav-link
  - make the links sit on the same line
  - give a top and bottom padding of 5px
  - left and right padding of 15px
*/


/*
 target the links inside element with the id of masthead
  - remove the underline from the links
  - make sure the text is white
*/
 

/*
 target the element with the id of footer
  - set the width to 100%
  - set the height to 100px
  - set the line height to 100px
*/


/*
 target the element with the id of copyright and float it to the left
*/


/*
 target the table element 
  - give a 2px black solid border to the table
  - collaps the border to remove spacing between cells
  - make the width 100%
*/


/*
 target the tr, td, and th elements
  - give a 2px black solid border to the table
*/


/*
 target the th element 
  - set the background color to black
  - set the text color to white
*/


/*
 target the element with the id of travel-table 
  - make the text color black
  - make the background color white
*/



/*
 target the h3 element within the element with the id of travel-table 
  - center align the text
*/



/*
 target the element with the id of social-links
  - float the social links to the right
  - give a top and bottom padding of 5px
  - give a left and right padding of 15px
*/
 


/*
 target the links inside element with the id of social-links
  - make sure the text is white and there is no underline
*/
```
