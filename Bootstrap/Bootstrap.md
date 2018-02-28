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
