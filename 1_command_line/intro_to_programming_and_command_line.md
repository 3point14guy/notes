# Introduction to Programming and Command Line Fun

This day will be for introductions to each other and some of the concepts that will help us get started with the journey to becoming a programmer.

## Useful Resources

[Presentation Slides](http://techtalentsouth.slides.com/techtalentsouth/charlotte-summer-2016-introduction?token=8x52SQrV)  
[CodeCademy - Learn the command-line](https://www.codecademy.com/learn/learn-the-command-line)  
[Mac Terminal Commands Cheat Sheet](https://github.com/0nn0/terminal-mac-cheatsheet)  
[Windows Command Prompt Cheat Sheet](http://simplyadvanced.net/blog/cheat-sheet-for-windows-command-prompt/)  
[Tech Talent South Portal](http://portal.techtalentsouth.com/)  
[Rails Install instructions provided to students](https://techtalentsouth.com/rails_install/)  

---

1. Introductions
2. Ice Breaker - Machines
3. Keys to Success
4. What you should expect
5. Intro to some useful concepts
    1. Computer Programming
        1. Exercise - Make a bowl of cereal
    2. Markup Languages - Focus on HTML
    3. Client Server Model - How does the web work?
    4. Convention over Configuration
    5. The KISS "Keep it simple, stupid" Principle
    6. The DRY "Don't Repeat Yourself" Principle
6. Ruby and Rails
7. Environment Check
8. The command-Line
9. Exercises
    1. [Navigating the command-line](exercises/navigating_the_commandline/)
    2. [Create sublime command-line shortcut](exercises/install_sublime_shortcut/)
10. Homework

---

## Introductions
Let's use this time to get to know everyone.

***PRO TIPS***

* Encourage students to sit next to someone new everyday. This will help students learn from eachother.
* Encourage students to talk through issues with their neighbors. Through helping eachother they will help themselves retain the curriculum

**Go around to each student and ask the talk about the following:**

1. Tell us your name and and a sentance or two about yourself.
2. What types of software are you interested in developing and why?
3. What profession are you in currently?
4. Tell us one cool/interesting thing about 


## Ice Breaker - Machines
The purpose of the Ice Breaker is to highlight critical thinking, working together, and breaking problems into small pieces. Basically the concepts It is also meant to be fun so have fun with it please. 

1. Split the class into teams of 2 or 3 depending on the size of the class.
2. Teams should be standing up and in their respective groups. It's a team exercise. Let's see those teams.
3. Go around to each team and **Secretly** assign them a machine. These are typically household appliances or something similar. Here are some examples.
    * Toaster
    * Fridge
    * Washing Machine
    * Weed Whacker
    * Popcorn Machine
    * Juicer
    * etc...
    
4. Each team needs to act out or perform their machine one at a time for the rest of the class. They can't talk but can make the noises the machine would make. Each person must participate in the process.
5. As each team is performing, the rest of the class is to guess the machine the group is acting out.

***Discussion Topics***  
*Talk about why we do the ice breaker and how it relates to how we solve coding problems*

*Discuss student takeaways from this exercise*

## Keys to Success
This is advice that will help students be successful in the program and as a professional developer.

* Get in the right mindset
    - Think about the long term big picture and dont let the short term frustration distract you
* You **MUST** put in the time.
* Take ownership of your education
* Try teaching/helping others. To teach is to learn!
* Pair program. Two minds is usally better than one. This type of thing is done a lot in professional environments
* Don't be afraid to fail. Most code doesn't run the first time. It's about the proccess, not about getting it to work the first time.
* Get out of your comfort zone. The more time you spend out of your comfort zone the bigger it gets.
* Be active and engaged in the development community. Go to meetups and stay on top of new technologies. Always be learning!


## What you should expect
Set expectations of what students should expect from the boot camp and its intructors

* Materials will be published in the TTS Portal (link in Useful Resources) after each piece of the curriculum has been delivered
* Homework will be breifly reviewed at the start of each session. This will give an opportunity to address any questions. Office hours are a good way to deep dive into questions related to homework
* Sessions will start on time
* Instructors will show up before class starts
* We will build several projects during the bootcamp
    - Please feel free to work on as many of your own projects as you like for practice. 
* There will office hours for each session. Please take advantage of this time to get answers to any questions or address any concerns you may have. This is also a great time for you to give us feedback on how we are doing.

##Intoduction to some useful concepts
In this section we will touch upon some common conepts in the programming world. We will discuss what computer programming is, how the web works, and some common principles related to programming

### Computer Programming
At a very high level computer programming is the concept of telling a computer through a programming language with a given set of instructions.

***Discussion Topics***  
*What are some things that come to mind when you hear the words "computer programming" or "writing software"?*

#### Are computers smart?
Computers are fast, not smart. They only do what they are told, no more no less. I know computers can seem smart but that's because they were programmed to behave that way. Computers are really great at calculations but should probably leave the higher level thinking to us.

#### Programming languages
As discussed, in order to program a computer we need to use a programming language. There are a ton of programming languages in the world andome are more popular than others. For this class we will focus on a language called "Ruby". Ruby is a simple and very powerful object-oriented language.

***Discussion Topics***  
*Can anyone name any other programming languages?*  

*Can anyone explain the difference between a front-end and back-end language?*

#### Exercise - Make a bowl of cereal
In plain english write step-by-step instructions for making a bowl of cereal.
Be as specific as possible. Assume nothing.

**Example**
> 1. Open panty
> 2. Locate Cereal
> 3. Chosse between Lucky Charms, Kix, or Cheerios.
> ...

***Discussion Topics***  
*Discuss some of the solutions to the exercise and touch on how this process is similar to how we tell a computer what to do through a programming language.*

### Markup Languages - Focus on HTML
There are several Markup languages out there. A couple of the most commonly known are HTML and XML. Markup languages put an opinion on how to format data in a text file usually for processing and presentation. They do this with the use of "tags" or "elements". HTML is the markup language we will focus on in this bootcamp.

**HTML Example**
```html
<!DOCTYPE html>
<html>
  <head>
    <title>This is a title</title>
  </head>
  <body>
    <p>Hello world!</p>
  </body>
</html>
```

***Discussion Topics***  
*Discuss the structure and some of the tags in the HTML example*

### Client Server Model - How the web works
When browsing the web, the client (your device) makes a request to a server (web server) which then responds to the client with the information requested. A client in this scenario would your computer, phone, tablet, or any similar device that has a web browser installed on it. You open your browser and request google.com and googles web servers then respond to you with the content that makes up google.com. When talking specifically about the web, HTTP Protocol that is used for communication between the client and server. HTTP stands for Hyper Text Transfer Protocol. We don't need a deep understanding of the HTTP protocol but we should understand that it is a defined set of rules that outline how clients and servers talk to eachother as it relates to the web.

***Discussion Topics***  
*Great opportunity to draw this out on the whiteboard*  

*Discuss other protocols like FTP, SMTP, FTP*

### Convention over Configuration
This is the concept that following conventions outlined by a full featured web framework can really speed up the development process. The framework has made a lot of decisions for you and your not stuck writing all that low lever configuration and boiler plate code.

This concept really comes into play when Ruby and Rails are combined. Ruby is a programming language and Rails is a web framework that is used with Ruby to build applications for the web. The framework has provided you all the building blocks for your application. You can just start using them and build an application in no time.

### The KISS Principle "Keep it simple, stupid"
The KISS principle states that most systems work best if they are kept simple rather than made complicated; therefore simplicity should be a key goal in design and unnecessary complexity should be avoided.

***Discussion Topics***  
*Break problems down into small pieces. You want to get to the solution one piece at a time*

*Don't over-engineer. Build only the features you need for basic functionality. You can do the fancy stuff once the core functionality is solid*

###  The DRY "Don't Repeat Yourself" Principle
It's a common occurance to catch yourself duplicating code in your software application. Sometimes you just need to do the same thing somewhere else in your code. This is where the DRY principle comes in. Rather than duplicating the code we should try to find a way to make that code re-useable. Making code re-useable makes it easier to maintain. You can now update code in one spot and it's fixed in all the places it is used in the application. You can imagine the incosistensies and errors that may occur if you had to remember all the places you need to update that duplicated code. 

***Discussion Topics***  
*Finding a balance between making code re-usable before making it work. As you start out I would prefer you repeat your code until the core functionality works. You can always improve after you have your proof of concept*


## Ruby and Rails
Ruby and Rails are not the same thing. This section aims to talk Ruby and Rails and how they relate to each other.

**Ruby**  

* Ruby is a programming language
* The code we write is in the Ruby language
* Ruby is an object-oriented

**Rails**  

* Rails is a web application framework.
* Provides a set of tools and functionality to rapidly develop web applications (Convention Over Congiguration).
* Rails is ***NOT*** a programming language
* Rails is written with Ruby


## Envrionment Check
Let's take a few minutes to make sure our environments are configured properly. We are going to verify the versions of the software we need to complete this bootcamp. This will also give us a sneek peek at the command line.

**Verify versions of the following software**

***ruby (2.3)***
```sh
$ ruby -v
ruby 2.3.3p222 (2016-11-21 revision 56859) [x86_64-darwin14]
```

***rails (5)***
```sh
$ rails -v
Rails 5.0.1
```

***git (not picky on version)***
```sh
$ git --version
git version 2.8.1
```

** OSX Only **  

***xcode-select (2339 or higher)*** 
```sh
$ xcode-select -v
xcode-select version 2339.
```

***gcc*** 
```sh
$ gcc --version
Configured with: --prefix=/Applications/Xcode.app/Contents/Developer/usr --with-gxx-include-dir=/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.11.sdk/usr/include/c++/4.2.1
Apple LLVM version 7.0.2 (clang-700.1.81)
Target: x86_64-apple-darwin14.5.0
Thread model: posix
```

***rvm (1.28.0)*** 
```sh
$ rvm -v
rvm 1.28.0 (latest) by Wayne E. Seguin <wayneeseguin@gmail.com>, Michal Papis <mpapis@gmail.com> [https://rvm.io/]
```

***homebrew (>=1)***
```sh
$ brew -v
Homebrew 1.1.6-45-g9cce341
Homebrew/homebrew-core (git revision c0e2a; last commit 2017-01-10)
```

***Reccommended bootcamp software***

* **[Sublime Text 3](https://www.sublimetext.com/)** - Text Editor used for writing code
* **[Chrome Web Browser](https://www.google.com/chrome/browser/desktop/)** - The preferred browser for this bootcamp.

***Optional software installs for OSX***

* **[iTerm2](https://www.iterm2.com/)** - More features than the default termnial. Tabs and split screen are useful

***Recommended software installs for Windows***

* **[Git Bash](https://git-scm.com/downloads)** - Linux like git command line experience on mac

**Incase the above didn't go so well**  
Issues with student environments should be taken care of before class. If for some reason you run into issue please use cloud9 as a backup environment.

* **[Cloud9](https://c9.io/)** - A cloud based Development Environment


## The command-Line
The command line is a quick, powerful, text-based interface developers use to more effectively and efficiently communicate with computers. Once you get the hang of it, you'll be able to navigate directories, run programs and launch
applications much faster than pointing and clicking. It may feel a bit awkward at first after years of relying on a GUI interface, but the more you use the command line the more comfortable you will feel.

**Review the following commands**

* **`cd directory_name`** - change directory
* **`cd ..`** - go one level up in the directory structure
* **`ls`** (`dir` in Windows) - list contents of current directory
* **`pwd`** - print current working directory
* **`mkdir directory_name`** - make a new directory
* **`touch file_name`** (`type NUL >> filename` in windows) - create a new file
* **`mv from_path to_path`** (`move` in windows) - move file or directory, also used to rename
* **`cp from_path to_path`** (`copy` in windows) - copy file or directory
* **`rm file_name`** (`del` in windows) - deletes a file an with some flags can delete directories as well. `rmdir` is available as well for directory removal

***Discussion Topics***  
*show how to use the `man` command to pull up the manual pages for these commands (e.g. `man ls`)*  

*show some examples with flags like `ls -lash` and how to find thos flags in the manual page*

***Discussion Topics***  
*Tab completion with the comman-line to speed things up*

*Using the up/down arrows to go through previous commands*

*`history` command to show previous commands run*

*CTRL-C to exit an active command*

##Homework
It is suggested the students go through the [CodeCademy - Learn the command-line](https://www.codecademy.com/learn/learn-the-command-line) lesson.

***PRO TIP***  
*You know your students and their progress. Feel free to come up with homework of your own that aligns with the students current needs*


