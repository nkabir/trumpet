 Trumpet Documentation
=======================

.. image:: https://travis-ci.org/umeboshi2/trumpet.png?branch=master
   :target: https://travis-ci.org/umeboshi2/trumpet





Abstract
========

This project contains a simple set of essential models, view classes 
and templates that help generate a web application using the pyramid 
framework.

This is ALPHA quality code.  Expect things to break while this project
is slowly formed.



Pyramid Framework: http://www.pylonsproject.org/

SQLAlchemy: http://www.sqlalchemy.org/

Mako Templates: http://www.makotemplates.org/

Icons: http://www.sireasgallery.com/ (trumpet icon)


Setup
========

I like to use virtualenvwrapper::

  sudo apt-get install virtualenvwrapper

  mkvirtualenv trumpet
  workon trumpet

go build haberdashery and install into your virtualenv:

https://github.com/umeboshi2/haberdashery

Prepare the package according to the instructions then
install it as editable::

  python setup.py develop

Development packages are needed to install some of the 
python packages::

  sudo apt-get install libpq-dev python-dev libjpeg62-dev libpng12-dev libfreetype6-dev liblcms1-dev python-requests libxml2-dev libxslt1-dev





TODO
====

- Integrate sucker-compass.  https://github.com/adambom/sucker-compass

- Admin section will handle some site images and text.

  + make thumbnail images

  + better list images page

- start using cryptacular and/or SRP

- add exception logging

- start making REST interface

  + cornice uses url-dispatch, but can possibly work alongside
    traversal in a hybrid manner.

- compare python-magic and filemagic

- add exception pages

- replace fanstatic with webassets


Webviews
===========

A Webview is a name created to denote a single page application.  Trumpet is 
being geared to become a library to help create websites where most/all 
pages are apps.  I have managed to shrink the html page served by pyramid 
to a very small head with one script tag loading requirejs and pointing 
to the loader for the app.  I would love to use another name for this, but 
naming is probably the hardest part of development.

Page Layouts
-------------

Before switching to the SPA paradigm of thought, every page was rendered 
with a template that depended on the presence of a layout model.  The layout 
model was simply an object with attributes that was applied to a mako 
template.  The type of each attribute is either text or mako template.  One 
of my original goals was to store the main layout template in the 
database, as well as css and javascript, to allow the end user/admin to 
customize the site without updating the code.

Server vs. Client
------------------

Mako templates are very powerful, allowing the author to wield the 
full power of python when rendering the template.  In fact, the 
templates are versatile enough to bypass writing code in the 
view callable and put all the logic in the template, although this 
is generally not the wisest thing to do.  The largest problem with 
using the mako templates is that the code is executed server side, 
preventing me from being able to protect the service from mistakes 
or malice.  I thought about using a more restrictive template 
system, but soon realized that the inherent problem was the 
server side rendering, and the server templates would either have 
to be too limited to be of more than cosmetic use, or flexible 
enough to bypass the policy of the server.

This is where client side templating comes to the rescue.  With 
client side templates, it is far more difficult to endanger the 
service that is being provided.  I am expecting the worst of the 
problems to be dysfunctional pages, although the admin pages that 
edit the templates should always work.  I also see a very slim (I 
hope!) possibility that bad templates could cause a denial of 
service, but I don't expect this to be a problem that occurs often.

Being less familiar with javascript than with python, I had to 
search and compare templating styles.  I started with underscore 
templates, but found them to be very limiting.  I then started 
using EJS templates, and found them to be very similar to mako 
templates, although not as versatile.  Nevertheless, I settled 
on using EJS templates for trumpet.

After believing I was happy with using EJS templates, I was 
looking around the stackoverflow forums to find a solution 
that I was having with CoffeeScript and learned about 
teacup.  Teacup is a domain specific language that works 
very well as a templating system.  With teacup, not only do you 
have the full flexibility of javascript when rendering the 
template, you are also using coffeescript to define the template, 
which is far more elegant than anything else I have seen on either 
the python or javascript sides.  I did like how concise jade is, but 
I feel that teacup will be a better fit for trumpet.

REST
----------

The general idea behind REST isn't really hard to understand.  It 
was having to unlearn the way by which much of the web had already 
been operating for years.  I spent many years knowing nothing of 
PUT and DELETE, but only being familiar with GET and POST, which I 
naively treated (loosely) as read/write methods.  Now that I look 
back (and not very far either) I was often using GET to perform both 
deletions of single objects, as well as attaching relations together.

After learning REST, my ability to write arbitrary url's to perform 
a function has been severely hampered, and this is a good thing.  I 
now have only four verbs that I am able to use, and I am completely 
restricted from putting verbs in the url, or even identifying the url 
as an action.  These restrictions help keep the web services well 
structured and coherent.  In fact, a good REST API decouples the 
server from the browser, allowing a larger variety of clients to 
have access to the services.

Static Resources
-------------------

Managing static resources can become very messy the more involved 
a project becomes in using them.  The very large variety of 
javascript libraries and css frameworks available can be 
overwhelming.  Making sure that everything fits together and works 
can be an arduous task.  Tracking upstream dependencies is probably 
a bit more difficult for a python/pyramid programmer than it is 
for a person using rails or nodejs.  I had been (and I am currently 
still) using fanstatic to help manage these resources.  There are 
quite a few prepackaged libraries depending on fanstatic available 
on the Python Package Index.  These packages don't seem to be in 
much use, and after updating quite a few of them myself, I decided 
to wean myself away from fanstatic.  I am currently investigating 
webassets, which seems to be a far more robust and capable asset 
manager.

Moreover, and more especially with css, it can 
become very time consuming to modify two or more upstream css 
resources to match the general style of your page.




Notes
=====

Making an action button
------------------------

- make a 'div' with 'action-button' in the class

- also name the div in either the class or id, so it
 can be selected with jquery

- in the div, make a hidden input with value=url

- make sure that the action-button is imported in css

- make jquery script that performs action on click

  + example:  window.location = url






haberdashery
============

[![Build Status](https://travis-ci.org/umeboshi2/haberdashery.png?branch=master)](https://travis-ci.org/umeboshi2/haberdashery)

Introduction
--------------


Common accessories for web applications

The purpose of this project is to provide a common code base for 
providing command static resources, such as css and javascript, 
for my pyramid projects.  This is basically just a metapackage 
that depends on many javascript libraries that are deployed with 
fanstatic.  I create the css files with compass, and while a 
framework is here to compile css with compass, as well as compile 
javascript using coffee, this is usually done at the project level.  
The framework here only provides a reference for how this can 
be done in the project.

Also, with regards to generating css, the sass/partials directory 
is used as a set of components where resources are built.  This 
directory is symlinked in the sass directory of my projects, which is 
what I will be doing until I find a better method of using the compenents, 
or spend the time to create mixins.

Setup
======

Setup Compass
----------------

Make sure rubygems is on your system:
```sh
sudo apt-get install rubygems
```


Setup local gem environment:

```sh
mkdir -p ~/local/gems
```
Add to ~/.bashrc:

```sh
#setup gems if directory exists
if [ -d ~/local/gems ]; then
    export GEM_HOME=~/local/gems
    export PATH=~/local/gems/bin:$PATH
fi
```
Source the bashrc or spawn another shell and install the gems:

```sh
gem install compass
gem install susy
gem install sassy-buttons
gem install bootstrap-sass
gem install compass-ui
```

In the haberdashery directory, prepare and compile css:

```sh
python generate-scss.py
compass compile
```


Setup NodeJS
-------------

Get nodejs for debian
--------------------------

For debian, build a current version of nodejs using
the build scripts in this repository:

https://github.com/mark-webster/node-debian

Follow the instructions to build the debian package, 
then install it.

Then, install these packages globally:

```sh
sudo npm install -g coffee-script
sudo npm install -g grunt-cli
sudo npm install -g bower
```

Build Haberdashery
--------------------

In the project directory, get the packages for grunt:

```sh
npm install
```

Then install the bower packages:

```sh
bower install
```

Bower packages can contain whole git repositories, which can 
be excessive when deploying a python package of static resources.  I 
have written a script that helps to deploy only what is needed from 
the bower components.  The script is not very smart, but handles any
bower package that points to a single file, or list of files very well.


```sh
python prepare-bower-components.py
```

Next, we need some google fonts.  We need the requests package 
for python to run the next script.  Either install it system wide:

```sh
sudo apt-get install python-requests
```

or install it into your virtualenv:

```sh
pip install requests
```

 
Then download the fonts from google by running a python 
script:

```sh
python get-google-fonts.py
```

This will install the fonts in the haberdashery package 
directory.


run grunt

```sh
grunt
```

make package

```sh
python setup.py (develop/install/sdist)
```

 
CSS Framework
================


-  [Compass](http://compass-style.org/):  
   Compass is the tool I use to generate my CSS resources.  The CSS 
   specification has no definitios for variables, forcing many web 
   developers to make class names such as "green" and then add CSS 
   code like this:
   
        .green {
		background-color: green;
		}
			   
   But what the developer really needs is something more along this 
   idea:
   
        .warn {
		background-color: $warning-background;
		}
		   
   . . . which helps to simplify the structure of the CSS and remove some 
   of the bad hacks that are used to workaround the deficiencies of 
   the CSS specification.

-  [Susy](http://susy.oddbird.net/):
   Susy is a grid layout system that will allow for responsive webpages.  I 
   am not using this anymore, as bootstrap is currently handling the 
   responsive grid layout, but Susy is superior to bootstrap and since I 
   am also using bootstrap-sass, I feel that I can eventually reimplement 
   the bootstrap grid layout in Susy.

-  [Sassy Buttons](http://jaredhardy.com/sassy-buttons/): 
   This is a collection of mixins and defaults that help a developer make
   custom buttons very easily.

-  [Bootstrap for Sass](https://github.com/thomas-mcdonald/bootstrap-sass): 
   This wonderful package allows me to refrain from using the css that is 
   provided with bootstrap and quickly make a custom version that I can 
   integrate more closely with other objects on the page.  Having bootstrap 
   in this form allows me to adjust how bootstrap operates and allows me 
   to only choose the parts I need (Currently everything is included).
   
-  [FontAwesome](http://fontawesome.io/):
   Instead of just using the basic css, I have chosen to use the 
   fontawesome-sass distribution.  This provides scalable vector icons
   to websites.
   
-  [Compass UI](https://github.com/patrickward/compass-ui): 
   This compass plugin provides the ability to generate jQueryUI themes
   with a minimum of effort.  I have spent hours on the themeroller before
   trying to create a custom theme that would match the general colors that 
   I use on a web page.  With this plugin, all I have to do is set the 
   variables to correspond to the color variables that I use elsewhere on the 
   page and I instantly get themed widgets that don't look like they came 
   from another site.
   

Basic Javascript Libraries
===============================

-  [Requirejs](http://requirejs.org):
   Required.
   
-  [jQuery](http://jquery.com/): 
   jQuery is a very good for selecting and maninpulating elements in the DOM.

-  [jQuery User Interface](http://jqueryui.com/): 
   jQueryUI is used for the fullcalendar widget, as well as for dialog boxes 
   and other user interface elements that aren't used through boostrap.  The 
   corresponding styles are maintained with compass.

-  [Bootstrap v3](http://getbootstrap.com/): 
   Bootstrap is a CSS/Javascript framework used to help make responsive 
   websites.  Bootstrap was selected to be used in order to serve to 
   mobile devices.  The CSS is handled through compass with bootstrap-sass.
   
-  [Underscore.js](http://underscorejs.org/): 
   Underscore is a library full of useful utilities, and like jqueryui, is 
   depended upon by other javascript libraries I use.

-  [Backbone.js](http://backbonejs.org/): 
   Backbone is an excellent library that provides an api to make very 
   rich views tied to models that are seamlessly synchronized with 
   the server via a REST interface.

-  [FullCalendar](http://arshaw.com/fullcalendar/): 
   FullCalendar is a very good library that provides an interactive 
   calendar where events can be retrieved dynamically and grouped, 
   colored, or otherwised styled in many ways.  The calendar provides 
   monthly, weekly, and daily view models to interact with.

-  [Ace Editor](http://ace.c9.io/#nav=about): 
   The ACE editor is a good text editor that is very useful for 
   editing html, css, java/coffee scripts, and other formats that
   aren't being used yet.
   
-  [CoffeeScript](http://coffeescript.org/):
   I am currently experimenting executing coffeescript on the client 
   using the browser to compile the code.  While compilation is 
   generally quick on the browser, the size of the compiler (196KB, and 
   already minified) encourages me to consider implementing server side 
   compilation.
  
-  [Teacup](http://goodeggs.github.io/teacup/):
   "Teacup is templates in CoffeeScript."  -- nuff said
   http://en.wikipedia.org/wiki/Domain-specific_language
  



Misc Javascript
=================

This section lists libraries and widgets that I am experimenting with:


Knockback
http://kmalakoff.github.io/knockback/

TinyMCE
http://www.tinymce.com/

Modernizr
http://modernizr.com/

EJS Templating (I was using this until found out about teacup)




Creating CSS
-------------

This project is configured to make use of compass to compile sass/scss stylesheets into css to be deployed through fanstatic.


Using Javascript
--------------------

This project is using coffeescript to generate javascript to be 
deployed.  The coffee command is best installed by installing the 
nodejs packages from sid (I used apt pinning for this), then using the 
npm installer to install coffee to /usr/local. (more info on this when I 
remember what I did).

Coffee Script: http://coffeescript.org/




Other Packages
---------------------

https://github.com/umeboshi2/js.jquery

This package uses jQuery2 to provide the jquery resource.  This is 
meant to be a drop-in replacement for js.jquery, and using 
this modification shouldn't be required for the rest of the 
code to work properly.

https://github.com/umeboshi2/js.ace

This update was necessary for EJS templates.  I created an update 
script that helps to upgrade the package periodically.


https://github.com/umeboshi2/js.deform

I have been upgrading to deform version 2.  This package is 
currently just a mirror of https://github.com/dairiki/js.deform .


https://raw.github.com/PaulUithol/Backbone-relational/master/backbone-relational.js


http://pathable.github.io/supermodel/



Minified Resources
---------------------

The watch script will automatically minify every scss/sass and 
coffee file that is created or modified.  In order to do this, you 
need to install yui-compressor.  I am using the package from 
wheezy to provide this.

New System
-------------------

Fanstatic has been removed completely.  All javascript dependencies are 
now being maintained with javascript development tools.  Package management 
is community driven and operated.  By partitioning the project to have the 
appropriate community help maintain the packages and dependencies, I firmly 
feel that the project can be more easily updated and maintained.


TODO
------------

- Clean stdlib directory.  Make sure bower package exists for each resource.

- Start tracking css selector usage and use this to make smaller stylesheets.

- Use requrirejs optimizer on each app's main.js and build single 
  concatenated file of app tree.
