# Ruby on Rails Tutorial sample application

This is the sample application for the
[*Ruby on Rails Tutorial:
Learn Web Development with Rails*](https://www.railstutorial.org/)
by [Michael Hartl](https://www.michaelhartl.com/).

## License

All source code in the [Ruby on Rails Tutorial](https://www.railstutorial.org/)
is available jointly under the MIT License and the Beerware License. See
[LICENSE.md](LICENSE.md) for details.

## Getting started

To get started with the app, clone the repo and then install the needed gems:

```
$ gem install bundler -v 2.3.14
$ bundle _2.3.14_ config set --local without 'production'
$ bundle _2.3.14_ install
```

Next, migrate the database:

```
$ rails db:migrate
```

Finally, run the test suite to verify that everything is working correctly:

```
$ rails test
```

If the test suite passes, you'll be ready to run the app in a local server:

```
$ rails server
```

For more information, see the
[*Ruby on Rails Tutorial* book](https://www.railstutorial.org/book).

Chapter 3 notes

This is chaptewr 3 in the Ruby on Rails Tutorial
It we are builing a stitic page
we run rails new sample app --skip bundle
adjust the Gemfile
bundle config set --local without 'production"
bundle install
git init, add, commit
create a github repo
deploy to fly.io
in the book it says to make a gemfile.lock which specifies 
exactly which gems to use
also we are deplying to fly.io from the local machine not from github
the book hs a push heroku command which we are not using with fly.io
it seems to make a separate repo integrated with heroku
there are similar features in fly.io, like deploying from a repo, or
'continunuos deployment' where puched changes are automatically deployed but
Im saving this for later.

this chapter we are using rails and MVC to make a statiuc page.
we create actions and views.
we checkout a new static-pages branch in git.

we use the command 

rails generate controller StaticPages home help

  rails generate controller ControllerName action1 action2

this createsa a set of files. 
in the app folder it creates a controllerstatic_pages_controller.rb
in adds config/routes.rb of get "static_page/home" and get "static_pages/help
also /views/static_pages, /views/static_pages/home /views/static_pages/help
also helper and tests

the book talks about shortcuts as well as undoing the genereate command with destroy
and the db.migrate with db.rollback

in config/routes.rb the generate controller command abnove has
created get "static_pages/home" and get "static_page/help" rules
it says this "maps" URL requests to actions in the static_pages_controller

in app/controllers/static_pages_controller.rb 
we have the method definitions of home and help in the StaticPagesController class 
which was created from the generate command
StaticPagesController inherits from ApplicationController methods specific to rails
when visiting URL /static_pages/home Rails looks in the static pages controller 
and executes the home action, and then renders the view corresponding to the action.

in 3.2.2 we add content to the html help and home pages in views.

we then start using the test/controllers/static_pages_controller_test.rb
which we created with the generate command.

there are two tests fo rhte two pages home and help
the test has a string, "should get home", a get static_pages_home_url, 
and assert :success which is a response statuc code of 200 ant the page didnt crash. 

we then create a red test for a page we haven't created yet.
this would be the "about page"

the test says

test "should get about" do
  get static_pages_about_url
  assert_response :success
end

for the other pages the generate command created the view
and the action and the route automatically.

so we can start with the route first
config/routes.rb
  get "static_pages/about"

the book says that this will create a helper called
static_pages_about_url

running the test it says the action 'about' is not found in the controller
so in staticPagesController we add 
def about end

test says missing template, which is the same as a view
so we need to create a view

in the testing we use the assert select which tests the content of an html tag
in this case "title" 
 then we factor the tests using an instance variable
 def setup @base_title = "Ruby on Rails Tutorial Sample App" end
 then placed into the test with string interpolation.
using #{} notation

refactoring. we are goiing to use embedded ruby. 

on the app/views/static_pages/home.html.erb we add
<% provide(:title, "Home") %>
...
<%= yield(:title) %>
  here <%%> executes code and <%= %> executes and inserts

now each web page has the same html, with the only difference 
being in the body tag
Rails has a layout page
app/views/layouts/application.html.erb
into this html, in the title tag de insert
<%= yield(:title) %>
to use the <% provide(:title, "Home") %> from the home.html.erb
and in the body of the application.html.erb
<%= yield %>
will take the body from the home.html.erb.

so the home.html.erb(and the help and about pages) have embedded ruby
<% provide(:title, "home") %>
and the /<body>' contents
amd rails will try to insert this into to the application.html.erb
in views. 

Now there is a exercise to make contact page
config/routes.rb    > get static_pages/contact   
create view         > app/views/static_pages/contact.html.erb
  add <% provide(:title, "Contact") %> and content
app/controllers/application_controller.rb action   > def contact end



3.4.4 setting the root route
in config/routes.rb we say 
root "static_pages#home"
we use a hash mark rather thatn just a slash. \
this creates a root_url helper

Chapter 4 notes

custion helpers. we use embedded ruby to make abase title in case one is not provided in the view page. helpers are function for use in the view page.

the title says About | Ruby on Rails Tutorial Sampole App
if the About is not provided it says
| Ruby on Rails Sample App
to make a base tile which elimintes the vertical bar at eh begining we make a 
helper function full title that returns that title without the bar is no page title is provided.

string interpolation #{}
 printing
  puts
  print is like puts but without a newline

  chapter 5
  5.2 sass and asset pipeline
  asset piepline refers to management of static assets of css and images. there are asset directories, manifest files and prepocessor engines.
  the asset directories are in app, lib or vendor directories. which depends if the css or images are scopes to the project, to the dev team of if they come from a third party. each asset directoru has csss and images subdirectories. so this is where we place css files, or in this case custom.scss in app/assets/stylesheets
  manifest files combine css files for production. it's in the app/assets/stylesheets/application.css file, but we dont have to change the defaults.
  the preprocessor engine works with the manifest file to process the css for production to mak eit more efficient for the browser to load.

  scss is like css, but with added features like nesting a variable declarations

  with nesting, for example you have a class with an atribute, then the class combinend with a tag, you can nest the second insode the first.
  if you want to :hover you have to nest it with the ampersand  &:hover
  varibles are declared
  $variable-name: attribute;
  for example
  $light-gray: #777;
Bootstrap uses Less not Sass,but it's alsom the same, you cahnge teh @ sign for a dollar sign. you can look at the bootstrap page of less variables

5.3 layout links
we for rails we want to use named routes

<%= link_to "About", about_path %>

"About" is the word that is linked, and about_path is the named route.
  
here assert_template compares with the result opf get root path
then assert select looks for the tag with a=href and the question mark is replaced 
  with the string value refered by the named route.
so first we we think of the path we want to test
the root path, we look at the page that is should be, 
using assert template. this wil compare get root path with 
app/views/static_pages/home.html.erb
which is found with just static_pages/home
but the links we will check with assert_select 
are actually in the header and footer
