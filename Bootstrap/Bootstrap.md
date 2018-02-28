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
You can specify numbers from 1 to 5. Example
~~~html
  <h1 class="bg-success m">Hello World</h1>
~~~
