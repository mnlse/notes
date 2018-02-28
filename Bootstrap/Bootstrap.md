# Typography
## h1, h2, h3, h4 as a class
~~~html
<h1>Heading one</h1>
<h4 class="h1">Heading one</h4> <!-- Adding the h1 class to h4 makes it look like h1 -->
~~~
## Muted text
![text-muted](images/text_muted.png)

Adding muted text (looks grey):
~~~html
<h1>Hello World
  <small class="text-muted"></small>
<h1>
~~~
## Display property
![typography-display](images/typography_display.png)

Display text:
~~~html
<h1 class="display-1">Display 1</h1>
<h2 class="display-2">Display 2 </h2>
<h3 class="display-3">Display 3</h3>

<!-- or h1 with display-3 -->
<h1 class="display-3">Display 3</h1>
~~~
## Font weight
Font weight:
~~~html
<p class="font-weight-light">font-weight-light</p>
<p class="font-weight-normal">font-weight-normal</p>
<p class="font-weight-bold">font-weight-bold</p>
~~~

## Lowercase, uppercase, capitalize (text transforms)
![typography_transforms](images/typography_transforms.png)
~~~html
<p class="text-lowercase">Test</p>        => test
<p class="text-uppercase">test</p>        => TEST
<p class="text-capitalize">test</p>       => Test
~~~

## Blockquotes
![typography_blockquote](images/typography_blockquote.png)
~~~html
<blockquote class="blockquote">
  This is my blockquote
</blockquote>
~~~


### Blockquote with footer
![typography_blockquote_footer](images/typography_blockquote_footer.png)
~~~html
<blockquote class="blockquote">
  Hello World
  <footer class="blockquote-footer">
    Some author
  </footer>
</blockquote>
~~~

### Right aligned blockquote with footer
![typography_blockquote_right_footer](images/typography_blockquote_right_footer.png)
~~~html
<blockquote class="blockquote text-right">
  This is my right-aligned blockquote with an author (footer)
  <footer class="blockquote-footer"> Some author </footer>
</blockquote>
~~~

## Lists
~~~html
<!-- remove bullet points -->
<ul class="list-unstyled">
  <li>List item</li>
  <li>List item</li>
</ul>

<!-- inline list -->
<ul class="list-inline">
  <li class="list-inline-item">Item</li>
  <li class="list-inline-item">Item</li>
  <li class="list-inline-item">Item</li>
</ul>
~~~

# Text-alignment & Display
## Justify
~~~html
  <p class="text-justify">
    Adipisicing molestias ipsa excepturi pariatur possimus. Non distinctio in aspernatur?
  </p>
~~~

## Align
~~~html
  <p class="text-center">
    Text
  </p>

  <p class="text-right">
    Text
  </p>

  <p class="text-sm-right">
    Aligned right on small or larger
  </p>

  <p class="text-md-right">
    Aligned right on medium or larger
  </p>

  <p class="text-lg-right">
    Aligned right on large or larger
  </p>

  <p class="text-xl-right">
    Aligned right on xl or larger
  </p>

  <p class="text-sm-center">
    Aligned center on small
  </p>
~~~

### Vertical alignment:
![alignment_vertical](images/alignment_vertical.png)
~~~html
<span class="align-baseline">baseline</span>
<span class="align-top">top</span>
<span class="align-bottom">bottom</span>
<span class="align-middle">middle</span>
<span class="align-text-top">text-top</span>
<span class="align-text-bottom">text-bottom</span>
~~~

### Display as inline
Turns block to inline-block:
~~~html
<h1 class="d-inline">Hello World</h1>
~~~

Display background on element:
~~~html
<h1 class="d-inline bg-success">Hello World</h1>
~~~

### Display as block:
~~~html
<span class="d-block bg-success">
  Hello World
</span>
~~~

### Display as inline-block
~~~html
<span class="d-inline-block bg-success">
  Hello World
</span>
~~~

# Floats & Fixed positions
~~~html
<div class="float-left"></div>
<div class="float-right"></div>
<div class="float-none"></div>
~~~

Responsive:
~~~html
<div class="float-sm-right"></div>
<div class="float-md-right"></div>
<div class="float-lg-right"></div>
<div class="float-sm-left"></div>
~~~

## Clearfix
~~~html
<div class="bg-success clearfix">
  <div class="float-left"></div>
  <div class="float-right"></div>
</div>
~~~

## Fixed & Sticky
Fixed to the top:
~~~html
<div class="fixed-top"></div>
~~~

Fixed to the bottom:
~~~html
<div class="fixed-bottom"></div>
~~~

Normal flow, when goes out of viewport sticks to the top (becomes fixed):
~~~html
<div class="sticky-top"></div>
~~~

# Colors & Background
## Colors:
![colors_text](images/colors_text.png)
~~~html
<p class="text-primary">text-primary</p>
<p class="text-secondary">text-secondary</p>
<p class="text-success">text-success</p>
<p class="text-info">text-info</p>
<p class="text-warning">text-warning</p>
<p class="text-danger">text-danger</p>
<p class="text-light">text-light</p>
<p class="text-dark">text-dark</p>
<p class="text-white">text-white</p>
~~~

## Background
~~~html
<div class="bg-primary text-white">Test</div>
<div class="bg-secondary">Test</div>
<div class="bg-success">Test</div>
<div class="bg-info">Test</div>
<div class="bg-warning">Test</div>
<div class="bg-danger text-white">Test</div>
<div class="bg-light">Test</div>
<div class="bg-dark">Test</div>
<div class="bg-white">Test</div>
~~~

# Margins & Padding
## General
Margins are prefixed by `m`. You can specify `m-1`, `mx-1`, `my-1`, `mt-1`, `mb-1`

Margin of top, right, bottom, left are specified by the general `m` property.
You can specify numbers from 1 to 5. Examples:
![margin_padding](images/margins_padding.png)
~~~html
  <!-- ---------------- MARGIN ---------------- -->
  <!-- general margin (x and y axis) -->
  <h1 class="m-1">m-1</h1>
  <h1 class="m-2">m-2</h1>
  <h1 class="m-3">m-3</h1>
  <h1 class="m-4">m-4</h1>
  <h1 class="m-5">m-5</h1>

  <!-- margin on x axis -->
  <h1 class="mx-1"></h1>
  <h1 class="mx-2"></h1>
  <h1 class="mx-3"></h1>
  <h1 class="mx-4"></h1>
  <h1 class="mx-5"></h1>

  <!-- margin on y axis -->
  <h1 class="my-1"></h1>
  <h1 class="my-2"></h1>
  <h1 class="my-3"></h1>
  <h1 class="my-4"></h1>
  <h1 class="my-5"></h1>

  <!-- margin left -->
  <h1 class="ml-1"></h1>
  <h1 class="ml-2"></h1>
  <h1 class="ml-3"></h1>
  <h1 class="ml-4"></h1>
  <h1 class="ml-5"></h1>

  <!-- margin right -->
  <h1 class="mr-1"></h1>
  <h1 class="mr-2"></h1>
  <h1 class="mr-3"></h1>
  <h1 class="mr-4"></h1>
  <h1 class="mr-5"></h1>

  <!-- ---------------- PADDING ---------------- -->

  <!-- general padding (x and y axis) -->
  <h1 class="p-1"></h1>
  <h1 class="p-2"></h1>
  <h1 class="p-3"></h1>
  <h1 class="p-4"></h1>
  <h1 class="p-5"></h1>


  <!-- padding on x axis -->
  <h1 class="px-1"></h1>
  <h1 class="px-2"></h1>
  <h1 class="px-3"></h1>
  <h1 class="px-4"></h1>
  <h1 class="px-5"></h1>

  <!-- padding on y axis -->
  <h1 class="py-1"></h1>
  <h1 class="py-2"></h1>
  <h1 class="py-3"></h1>
  <h1 class="py-4"></h1>
  <h1 class="py-5"></h1>

  <!-- padding left -->
  <h1 class="pl-1"></h1>
  <h1 class="pl-2"></h1>
  <h1 class="pl-3"></h1>
  <h1 class="pl-4"></h1>
  <h1 class="pl-5"></h1>

  <!-- padding right -->
  <h1 class="pr-1"></h1>
  <h1 class="pr-2"></h1>
  <h1 class="pr-3"></h1>
  <h1 class="pr-4"></h1>
  <h1 class="pr-5"></h1>
~~~

# Sizing & Borders
## Width:
![width_property](images/width_property.png)

~~~html
<div class="w-25">This element takes 25%</div>
<div class="w-50">This element takes 50%</div>
<div class="w-75">This element takes 75%</div>
~~~

## Height:
![width_height_classes](images/width_height_classes.png)
~~~html
<div style="width: 300px; height: 300px; border: 1px solid black; m-5" class="parent">
  <div class="child bg-primary w-25 h-25"> <!-- takes 25% width and 25% height of parent -->
  </div>
</div>
~~~

You cannot specify other values in the class.


## Borders
~~~html
<div class="m-5 w-50 h-50 border border-primary"></div>
<div class="m-5 w-50 h-50 border border-secondary"></div>
<div class="m-5 w-50 h-50 border border-success"></div>
<div class="m-5 w-50 h-50 border border-info"></div>
<div class="m-5 w-50 h-50 border border-danger"></div>
<div class="m-5 w-50 h-50 border border-warning"></div>
<div class="m-5 w-50 h-50 border border-light"></div>
<div class="m-5 w-50 h-50 border border-dark"></div>

<!-- You can also add rounded corners through the "rounded" class: -->
<div class="border border-primary rounded"></div>
<div class="border border-secondary rounded"></div>

<!-- To remove a bottom border: -->
<div class="border border-primary rounded border-bottom-0"></div>
~~~

# Buttons
~~~html
<button class="btn btn-primary"></button>
<button class="btn btn-info"></button>
<button class="btn btn-success"></button>
<button class="btn btn-alert"></button>
<button class="btn btn-warning"></button>
<button class="btn btn-danger"></button>
~~~

## Outline buttons
~~~html
<button class="btn btn-outline-primary"></button>
<button class="btn btn-outline-info"></button>
<button class="btn btn-outline-success"></button>
<button class="btn btn-outline-alert"></button>
<button class="btn btn-outline-warning"></button>
<button class="btn btn-outline-danger"></button>
~~~

## Button styles
~~~html
<!-- large button -->
<button class="btn btn-primary btn-lg"></button>
<!-- disabled button -->
<button class="btn btn-primary disabled"></button>
<!-- active button -->
<button class="btn btn-primary active"></button>
~~~

## Dropdown button
~~~html
<div class="dropdown">
  <button type="button" class="btn btn-primary dropdown-toggle" data-toggle="dropdown">
    Hello World
  </button>
  <div class="dropdown-menu">
    <a class="dropdown-item" href="#">Test</a>
    <a class="dropdown-item" href="#">Test</a>
    <a class="dropdown-item" href="#">Test</a>
  </div>
</div>
~~~

## Button groups
~~~html
<div class="btn-group">
  <button class="btn"></button>
  <button class="btn"></button>
</div>
~~~

# Navbar & Nav
## Navbar
~~~haml
nav.navbar.navbar-expand-sm.navbar-light.bg-light
  .container
    .navbar-brand
      ul.navbar-nav
        li.nav-item
          a.nav-link Test
        li.nav-item
          a.nav-link Test
~~~

[Official Documentation](https://getbootstrap.com/docs/4.0/components/navbar/)

## Nav
[Official Documentation](https://getbootstrap.com/docs/4.0/components/navs/)

# List groups & Badges
## Lists groups:
~~~haml
ul.list-group
  li.list-group-item Text

-#
  To make it light up when you hover:
ul.list-group
  li.list-group-item.list-group-item-action Text
~~~

You can use contextual classes with list groups:
~~~haml
ul.list-group
  li.list-group-item.list-group-item-primary
  li.list-group-item.list-group-item-success
  li.list-group-item.list-group-item-warning
~~~

## Badges
![list_with_badges](images/list_with_badges.png)
[Official Documentation](https://getbootstrap.com/docs/4.0/components/list-group/)

## Breadcrumbs
![breadcrumbs](images/breadcrumbs.png)
~~~haml
ol.breadcrumb
	li.breadcrumb-item
		a{ :href => Home } Home
	li.breadcrumb-item.active
		a{ :href => Home } Library
	li.breadcrumb-item
		a{ :href => Home } Data
~~~

# Forms
~~~haml
.form-group
	%label{ for: "name" } Name
		%input.form-control{ id: "name", type: "text" }
-#
	You can make the form control large or small

.form-control.form-control-sm
.form-control.form-control-lg
~~~

Form row:
~~~haml
.form-row
	.col
		input.form-control{ placeholder: "First name" }
	.col
		input.form-control{ placeholder: "Last name"
~~~

Form validation:
~~~haml

-#
	Add the .is-valid class to give a green border

.form-group
  label{ for: "test" } Test
	input.form_control.is-valid{ id: "test" }

-#
	Invalid is .is-invalid

.form-group
  label{ for: "test" } Test
	input.form_control.is-invalid{ id: "test" }
~~~

# Input groups
![input_group](images/input_group.png)
~~~html
<div class="input-group">
  <div class="input-group-prepend">
    <span class="input-group-text">$</span>
  </div>
  <input type="text" class="form-control">
  <div class="input-group-append">
    <span class="input-group-text">.00</span>
  </div>
</div>
~~~

# Alerts
Alerts also have contextual classes.

~~~html
<div class="alert alert-primary">
  Primary
</div>
<div class="alert alert-secondary">
  Secondary
</div>
<div class="alert-alert-success">
  Success
</div>
<!-- etc. -->
~~~

Alerts can be dismissable:
![dismissable_alert](images/dismissable_alert.png)
~~~html
<div class="alert alert-success alert-dismissable fade show">
  <button type="button" class="close" data-dismiss="alert">
    <span>&times;</span>
  </button>
  <strong>Dismissable</strong> Blog post added
</div>
~~~
