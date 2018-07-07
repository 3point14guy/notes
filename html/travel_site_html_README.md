# Exercise: HTML - A Simple travel site  

* Create a folder on your desktop called html_basics
* Create a file called travel.html
* Lets add the following html to the file so we have our basic page structure
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

* Let's go ahead and fill in the title for the site. Your title tag should resemble the following example
```html
<title>My Travel Site</title>
```

***All the work from this point should take place between the `<body>...</body>` tags. This is where we put the code we want to be visible to the end-user.***

* Let's add 2 headings to our site. We will use the `<h1>` and `<h2>` tags.
```html
<h1>Welcome to my Travel Site</h1>

<h2>Here are some places I'd like to visit</h2>
```

* Let's add a paragraph below our `<h1>`
```html
<h1>Welcome to my Travel Site</h1>
<p>This site is all about where I want to travel.</p>

<h2>Here are some places I'd like to visit</h2>
```

* Go ahead and add either an ordered `<ol>` or unordered `<ul>` list of places you would like to travel. Add the list below our `<h2>`. 
```html
<h1>Welcome to my Travel Site</h1>
<p>This site is all about where I want to travel.</p>

<h2>Here are some places I'd like to visit</h2>
<ul>
    <li>Spain</li>
    <li>Italy</li>
    <li>Aruba</li>
    <li>Alaska</li>
</ul>
```

* Lets add links that open in a new window to the tourism sites for the places we would like to visit. 
```html
<h1>Welcome to my Travel Site</h1>
<p>This site is all about where I want to travel.</p>

<h2>Here are some places I'd like to visit</h2>
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

* Lets add images of the flags that represent each place you would like to visit. The images came from wikipedia and the width has been set to 50.
```html
<h1>Welcome to my Travel Site</h1>
<p>This site is all about where I want to travel.</p>

<h2>Here are some places I'd like to visit</h2>
<ul>
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
</ul>
```

* Lets add a table below our list with some information about the places you want to visit.
```html
<h1>Welcome to my Travel Site</h1>
<p>This site is all about where I want to travel.</p>

<h2>Here are some places I'd like to visit</h2>
<ul>
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
</ul>

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
    <td>English</td>
  </tr>
</table>
```