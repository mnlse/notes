Author: Matthew Nielsen
License: [Creative Commons](https://creativecommons.org/licenses/by/4.0/)
# The Ruby language
## Intro
Interactive shell: `irb`  
File extension: `.rb`  
## Basic
Commenting is done via the hash (`#`) symbol.
~~~
# This is a comment
~~~

Print to the screen using `puts` and `print`.
~~~
puts "abc"
print "abcdefghijklmnoprstuvwxyz" 
~~~
`puts` (put string) adds a newline at the end. `print` does not.

---


The **if** statement:
~~~
if condition
  body
elsif condition2
  body2
else
  body3
end
~~~

### Loops
For loops are aliases for the `.each` method.

~~~
someHash = {
  "key" => "value",
  "key2" => "value2"
}

someHash.each { |k, v| puts k }
                 ^^^^
                 k = key, v = value
~~~                 

Infinite loops:
~~~
loop do
  input = gets.chomp
  if input == "x"
    break
  end
end
~~~

---
Methods are similar to JavaScript and Python:
~~~
"Hello says world".split(" ").reverse.join(" ")
=> world says Hello
~~~

To get a listing of each method of an object, use the `.methods` method.
~~~
[1, 2, 3].methods
"abc".methods
~~~

### Functions
~~~
def fnc
  ...
end
~~~

## Data types
Class hierarchy of Ruby:
![text](https://i.stack.imgur.com/1taqB.png)
<sup>(right click -> view image)</sup>

Data types:
~~~
String      =>      "abc".class
Fixnum      =>      1.class
Integer     =>      1.class.superclass
Numeric     =>      1.class.superclass.superclass
Float       =>      1.337.class
Numeric     =>      1.337.class.superclass
NilClass    =>      nil.class
Hash        =>      {}.class
Symbol      =>      :hello.class
Array       =>      [].class
Range       =>      (0..13).class
~~~

## Classes
~~~
class Person
  def initialize(name)
    @name = name
  end
end

class Teenager < Person
  def speak
    puts "#{@name}: What is this Ubuntu?"
  end
end  

class Adult < Person
  def speak
    puts "#{@name}: What I did on New Year's Eve? Just recompiling Gentoo."
  end
end

jack_inst = Teenager.new("jack")
jack_inst.speak

tom_inst = Adult.new("tom")
tom_inst.speak
~~~

# Rails
## Basics
To install rails: 
`gem install rails`  
Once installed, you can use the `rails` command.
### Initialize a new project
`rails new project_name`  
This creates a new directory (project_name):
| file/folder | purpose |
| ------ | ----------- |
| app/ | Contains the controllers, models, views, helpers, mailers and assets for your application. Main directory |
| bin/ | executables (server) |
| config/ | Configure your application's routes, database, and more |
| <span>config.</span>ru | Rack configuration for Rack based servers used to start the application. |
| db/ | Contains your current database schema, as well as the database migrations. |
| Gemfile Gemfile.lock | These files allow you to specify what gem dependencies are needed for your Rails application. These files are used by the Bundler gem. |
| lib/ | Extended modules for your application. |
| log/ | Application log files. |
| public/ | The only folder seen by the world as-is. Contains static files and compiled assets. |
| Rakefile | This file locates and loads tasks that can be run from the command line. The task definitions are defined throughout the components of Rails. Rather than changing Rakefile, you should add your own tasks by adding files to the lib/tasks directory of your application. |
| <span>README</span>.md | This is a brief instruction manual for your application. You should edit this file to tell others what your application does, how to set it up, and so on. |
| test/ | Unit tests, fixtures, and other test apparatus. |
| tmp/ | Temporary files (like cache and pid files). |
| vendor/ | A place for all third-party code. In a typical Rails application this includes vendored gems. |

---

### MVC Architecture
**MVC** -> **M**odel - **V**iew - **C**ontroller
<iframe width="560" height="315" src="https://www.youtube.com/embed/eTdVkgF_Slo?rel=0&amp;controls=1&amp;showinfo=0" frameborder="1" allowfullscreen></iframe>


~~~
HTTP GET ----> Router ----> Appropriate Controller
                                      |
                                      | require extra data?
                                      |
                             --------------------
                             |                  |
                             | yes              | no
                             |                  |
                           Model                -------------->>> Render View
                             |                                      ^
                             |                                      |
                             |                                      |
                          Database >----------->-------->-----------+
~~~                          

[A Detailed Overview of the Model-View-Controller (MVC) Coding Structure](http://www.onextrapixel.com/2012/03/14/a-detailed-overview-of-the-model-view-controller-mvc-coding-structure/)

MVCs are stored in:
~~~
app/models/
app/views/
app/controllers/
~~~

Routes:  
~~~
config/routes.rb
~~~

Assets:
~~~
app/assets/images/
app/assets/stylesheets/
app/assets/javascripts/
~~~
This is where you store your custom-made files for your app.

Helpers:
~~~
app/helpers/
~~~
This is where you store common logic **for your views** (only).

Database config:
~~~
config/database.yml
~~~

Settings for your environemnts:
~~~
config/environments/development.rb
config/environments/production.rb
config/environments/test.rb
~~~

Your migration files:
~~~
db/seeds.rb
~~~
A migration file is a file that modifies your database.   
[more on migrations](http://edgeguides.rubyonrails.org/active_record_migrations.html)

~~~
Gemfile
~~~
Is a file that specifies the dependencies of your application.

~~~
Gemfile.lock
~~~
Is a file that Bundler (ruby dependency manager) generates. It specifiies the exact versions of gems that were installed.
It is used by Heroku.

~~~
Rakefile
~~~
Is a config file for Rake - automation build tool.  
[More on Rake](http://jasonseifer.com/2010/04/06/rake-tutorial)  
Rake is similar to Make:  
"The make utility automatically determines which pieces of a large program need to be recompiled, and issues commands to recompile them"  
You can make it run tasks.

`_TODO_`

---
To run the server of your rails app:
`rails server`

---
### Create a simple page:
1. Create a route  

_config/routes.rb_
~~~
# get (routename), to: (controllername)#(action)

get 'welcome/index', to: welcome#showall
#    -------------       ------  ------
#         URL        controller  action
~~~
Confirm that you created the route: `rake routes`
~~~
Prefix        Verb URI           Pattern    Controller#Action
welcome_index GET  /welcome/index(.:format) welcome#showall
~~~

2. Create a controller  

Create a new file _welcome_controller.rb_:  

_app/controllers/welcome_controller.rb_
~~~ruby
class WelcomeController < ApplicationController

end
~~~

2.1. Add the appropriate action ("showall" here)
~~~ruby
class WelcomeController < ApplicationController
  def showall

  end
end
~~~
You don't have to put in anything if you just want to display a simple view.

3. Add a view  

Create a new folder:
~~~
app/views/welcome/
            ^ name of controller
~~~

Add the URL:  
_app/views/welcome/index.html.erb_  

~~~html
<h1>Hello world</h1>
~~~

.erb = embedded ruby - it works like PHP - you can write HTML & embed Ruby code.

Now, your application will render index.html.erb at 'welcome/index' URL   
(`localhost:3000/welcome/index`)

### Deploying a production app 
[Heroku](http://www.heroku.com/)  
Register.  
Now, create a production environment section in your `Gemfile`:  
~~~ruby
group :development, :test do
    # development environment gems
    gem 'sqlite3' # etc
end

group :production do
    gem 'pg' # PostgreSQL
    gem 'rails_12factor'
    # This is for Heroku - it doesn't support sqlite
end
~~~
Next, `bundle install --without production`  
Install the Heroku CLI.  
Login into Heroku: 
`heroku login`  
`heroku create` - create the production environment  
You can authenticate via SSH key: `heroku keys:add`  
To upload to Heroku: `git push heroku master`

Change url/name of your app: `heroku rename (name)`

### Links
~~~eruby
# <%= link_to (display name), (route path) %>
# Example:

<%= link_to 'About Us', aboutus_show_path %>
#                       ------  ---- ----

# aboutus = controller
# show = action
# path = it has to be appended to get the path
~~~

### Database
**CRUD**:  

**C**reate  
**R**ead  
**U**pdate  
**D**estroy  

These are the common tasks that you want to perform on your database records (create a new record, read the record, update the record, destroy the record).

**SQL** - **S**tructured **Q**uery **L**anguage

_fruits_
~~~
| ID | Name   | Price |
|----+--------+-------|
| 1  | Orange | 999   |
| 2  | Apple  | 2000  |
| 3  | Banana | 0     |
~~~
You don't have to specify the primary key(ID). In Rails it is generated automatically.

The model is an intermediate agent between the database and your program. 

### Generate CRUD object
~~~ruby
rails generate scaffold Fruit name:string price:int
#                        ^ model      ^type       ^type
#                          name

# This has generated a migration file, but we haven't affected the db yet.
rake db:migrate # run the migration - modify the db
~~~
What this command does: 
* create a model
* create a controller
* create views - CRUD actions
* create appropriate routes
* create migration file

This creates a fully featured CRUD object in your website.  
You can create, read, update, destroy records through your website form.

### Naming rules
Model name: singular, uppercase  
Table name: plural, lowercase of model name  
Model name filename: singular, lowercase  
Controller name: plural of model + `_controller`  

Example:  
Model: **Article**  
Table: **articles**  
Model file: **<span>article</span>.rb**  
Controller: **article_controller.rb**  

File names are snake_case, class names are CamelCase.

Another example:  
Model: **User**  
Table: **users**  
Model file: **<span>user</span>.rb**  
Controller: **user_controller.rb**  

### CRUD Manually
1. Create a migration file:  
`rails generate migration generate_fruits`  
Generates a migration file in: `db/migrate/`  
2. Create your columns
~~~ruby
class GenerateFruits < ActiveRecord::Migration
  def change
    create_table :fruits do |t|
      t.string :name
      t.int :price
    end
  end
end  
~~~
3. Save & `rake db:migrate`  
If you've made a mistake, you can rollback the database by one migration: `rake db:rollback`, or `n` migrations `rake db:rollback STEP=n`

To add a column to an existing table - generate a new migration:   
`rails generate migration add_country_of_origin`  

Edit: `db/migration/(date)add_country_of_origin.rb`

Use the `add_column` function:
~~~ruby
class AddCountryOfOrigin < ActiveRecord::Migration
  def change
    add_column :fruits, :country, :string
    # ^ fnc     ^table    ^colname   ^coltype
  end
end
~~~

4. Create a model  
Create `app/models/fruit.rb`
~~~ruby
class Fruit < ActiveRecord::Base

end
~~~

5. Create routes:  
`config/routes.rb`:
~~~ruby
resources :fruits
~~~

5. Create a controller with actions  
Create `app/controllers/fruits_controller.rb`:  
~~~ruby
class FruitsController < ApplicationController
  def new
    @fruit = Fruit.new  # we will use it in the view
  end
end
~~~

6. Create a view
`app/views/fruits/new.html.erb`
~~~html
<h1>Hello world</h1>
~~~

Now your page will display this simple view.  
Next, we need to add a form. We will use [form helpers](http://guides.rubyonrails.org/form_helpers.html):
~~~html
<h1>Add new fruit</h1>
<%= form_for @fruit do |f| %>
  <p>
    <%= f.label :name %> <br>
    <%= f.textfield :name %>
  </p>
  <p>
    <%= f.label :price %> <br>
    <%= f.textfield :price %>
  </p>
  <p>
    <%= f.submit %> <!-- uses action create -->
  </p>
<% end %>
~~~

7. f.submit uses action `create`, so let's define it:  
`app/controllers/fruits_controller.rb`:  
~~~ruby
class FruitsController < ApplicationController
  def new
    @fruit = Fruit.new
  end

  def create
    render plain params[:fruit].inspect
    #            ^ passed       ^ display
    #              params
  end
end  
~~~
Construct new `Fruit` instance:  
`app/controllers/fruits_controller.rb`:  
~~~ruby
class FruitsController < ApplicationController
  def new
    @fruit = Fruit.new
  end

  def create
    render plain params[:fruit].inspect
    #            ^ passed       ^ display
    #              params
    @fruit = Fruit.new(fruit_params)
    @fruit.save
  end

  private
  def article_params
    params.require(:fruit).permit(:name, :price)
  end
end  
~~~

8. Now it works(it actually saves to database), create a `create` action template:
~~~ruby
  def create
    render plain params[:fruit].inspect
    @fruit = Fruit.new(fruit_params)
    @fruit.save
    redirect_to fruits_path(@fruit)
    #            ^ path      ^ pass the fruit object to define which one to show (by ID)
  end
~~~

### Managing DB via model:  
#### Create
~~~ruby
fruit = Fruit.new
fruit.name = "Apple"
fruit.price = 3000

fruit.save # attempt to save to DB
~~~
~~~ruby
myVar = Fruit.new(name: "Apple", price: 3000)
myVar.save
~~~
~~~ruby
myVar = Fruit.create(name: "Apple", price: 3000)
#               ^ automatically attempts to save to DB, no need for myVar.save here
~~~
#### Read
~~~ruby
fruit = Fruit.find(3)
#                  ^ ID (of an existing row)
~~~

#### Update
~~~ruby
fruit = Fruit.find(3)
fruit.name = "BlackBerry" # ah! much better

fruit.save
~~~

#### Destroy
~~~ruby
fruit = Fruit.find(1)
fruit.destroy
~~~

To see all rows in your table: `Fruit.all` (this queries `fruits` table via `Fruit` model)

### Timestamps:  
When creating a table:
~~~ruby
def change
  create_table :fruits do |t|
    t.timestamps # created_at, updated_at timestamps (automatically)
  end
end
~~~

When updating a table:
~~~ruby
def change
  add_column, :fruits, :created_at, :datetime
  add_column, :fruits, :updated_at, :datetime
end  
~~~

### Validation
`models/fruit.rb`  
~~~ruby
class Fruit < ActiveRecord::Base
  validates :name, presence :true
  #                 ^ does not allow empty (name) value
end
~~~

~~~ruby
rails console # interactive rails console

fruit = new Fruit
fruit.price = 42
fruit.save # Doesn't work - "name" is empty

# check errors:
fruit.errors.any?
# => true
fruit.errors.full_messages
# => ["Name can't be blank"]
~~~

Length validation:
~~~ruby
validates :name, presence: true, length: { minimum: 2, maximum: 13 }
~~~

`created_at` & `updated_at` are automatically maintained by Rails. 

Check out your schema: `db/schema.rb` shows you how your db structure looks like.

### CRUD Routes
_config/routes.rb_
~~~ruby
resources :articles
~~~
Creates:
~~~
Prefix       Verb   URI Pattern                  Controller#Action
articles     GET    /articles(.:format)          articles#index
             POST   /articles(.:format)          articles#create
new_article  GET    /articles/new(.:format)      articles#new
edit_article GET    /articles/:id/edit(.:format) articles#edit
article      GET    /articles/:id(.:format)      articles#show
             PATCH  /articles/:id(.:format)      articles#update
             PUT    /articles/:id(.:format)      articles#update
             DELETE /articles/:id(.:format)      articles#destroy
root         GET    /                            welcome#index
~~~                                                                            
Next, you would have to create the controller + actions & view.
`app/controllers/articles_controller.rb`, `app/views/articles/new.html.erb` (action view)

### Failed validation
In your controller, you may get a failed validation.

~~~ruby
def create
  @fruit = Fruit.new(fruit_params)
  if @fruits.save
    # ok
    flash[:notice] = "Article was successfully created" 
    # ^ this has to be rendered in the view later
    redirect_to fruit_path(@fruit)
  else
    # render error
    render 'new'
    #        ^ render 'new' view
  end
end

private
def fruit_params
  # ...
end
~~~

Now to render flash messages:  
`views/layouts/application.html.erb`  
~~~html
<!-- insert: -->
<% flash.each do |key, msg| %>
  <ul>
    <li>
      <%= msg %>
    </li>
  </ul>
<% end %>
~~~

---

Automated way to display errors:  
~~~html
<!-- insert: -->
<% if @fruit.errors.any? %>
  <h2>Errors:</h2>
  <ul>
    <% @fruit.errors.full_messages.each do |msg| %>
      <li><%= msg %></li>
    <% end %>
  </ul>
<% end %>
~~~
This prints **verbose, user readable** errors.
