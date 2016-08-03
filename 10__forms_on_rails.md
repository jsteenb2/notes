###  Review of HTML Forms

##### What do the method and action attributes of an HTML form mean for your Router?
Action defines the path where the form is submitted too (matches a controlller).

Method is the use of GET or POST.

#####  What does the HTML field attribute type have to do with how the form works?
Tells the form what to render it as. Either checkbox, text field, dropdown.... and with that comes some basic validation.

##### What do the HTML field attributes name and value have to do with how the form is received on the back end?
The name attribute is usually in the params hash.

##### How are multiple radio buttons linked together?
Use the same name for each btn, and it groups them together.

##### How do you make text show up in an HTML form when it is first rendered by the page?
Use value or (placeholder if not wanting it to be actual submittable text).

##### What are the two ways you can link a <label> tag to a specific input?
Either use the for attribute or wrap the input with the <label> open and closing tags. Can only wrap one input at a time.

##### What do you need to submit a form?
To submit the entered information. In the submit input need to add attribute type="submit".

##### How is a <textarea> tag different from an <input type="text"> tag?
Can be multiline input.

##### How do you specify <textarea> size?
Using rows and cols attributes.

##### What security features are NOT provided by <input type="password">
No hiding the actual password unless you submit it via HTTPS.

##### How do you put grayed out placeholder text?
Use placeholder attribute.

##### How do you default a checkbox to "checked"?
Make the checked attribute true.

```html
  <input type="checkbox" name="vehicle" value="Car">
```

##### How do you disable a form input?
Just set disabled="true".

##### Why should you never trust disabling a form input as a real data security measure?
Because a user can submit values still if they feel inclined.

##### What are hidden fields and why are they used?
Used to mark a value or enter in useful information for the backend. They are hidden b/c the user doesn't need to know about them.

##### What two hidden fields are used by Rails?
The utf8 input and the authenticity input.

```html
<form>
  <input name="utf8" type="hidden" value="✓">
  <input name="authenticity_token" type="hidden" value="t/72yWAJ4gzmlfy/QH6CQPbQx5Cz8MRK+/8fueweUU8=">
</form>
```

##### If you build a dropdown where the user selects an integer, how does that look on the server side?
Looks like a string. All forms submitted as strings.

### Working with Forms in Rails

##### What information is contained in your Rails server output when you submit a form?
It will contain the http type and what controller it matches with. Also included is the params hash.

```ruby
Started POST "/user" for 127.0.0.1 at 2013-11-21 19:10:47 -0800
Processing by UsersController#create as HTML
Parameters: { "utf8"=>"✓", 
              "authenticity_token"=>"jJa87aK1OpXfjojryBk2Db6thv0K3bSZeYTuW8hF4Ns=",
              "email"=>"foo@bar.com", 
              "commit"=>"Submit Form" }
```

##### Why can't you just submit a POST request from a standard HTML form to Rails?
It will not contain the authenticity token and possibly not connect with the correct controller. Need autenticity toke for POST requests.

##### How do you add an authenticity token to your own forms?

```ruby
# use the form_authenticity_token method

<input type="hidden" name="authenticity_token" value="<%= form_authenticity_token %>">
#=> <input name="authenticity_token" type="hidden" value="jJa87aK1OpXfjojryBk2Db6thv0K3bSZeYTuW8hF4Ns=">
```

##### Is the "utf8" field required?
Note required but is a best practice.

##### What HTML input field attribute determines the key of your data in the params hash? The value?
The key matches the name attribute of form and value matches the value the key points too in the params hash.

##### How do you nest data coming from your HTML form?
Use hard brackets.

```html
<input type="text" name="post[title]">
    <input type="text" name="post[body]">
```

Produces:

```ruby
Parameters: { "utf8"=>"✓", 
              "authenticity_token"=>"wd4R0OmkSallVj+rey34+8TCKtExrs6WTEuexx12dh8=", 
              "post"=>{ "title"=>"Cool Title", 
                        "body"=>"Cooler Body"}, 
              "commit"=>"Submit Post"}
```

##### How would you submit data to your back end which shows up at params[:user][:attributes][:hair_color]?
Just do it like a normal nested hash.

##### What is the flow of a typical new/create cycle between the browser and your Rails app?

1. User visits the /new page
2. Router redirects him to the new action
3. Renders new.html.erb
  * uses form_for on an @user object
4. Submits the params hash to the create action
5. Params hash is whitelisted and then creates a new User with that data
6. When save is succesful we are redirected to /show page.
7. If can't be saved, rerenders the /new page.

##### How do you manually create a form which submits a "PUT" or "DELETE" request?
Include a hidden form element with value="patch" or "delete"

### Rails Form Helpers

##### What do Rails form helpers return?
Gives you dynamic form submissions when using form_for, can do both "create" and "edit" with same form.

##### How can you play with Rails helper methods from the console?
Use helper method

```ruby
> helper.tag(:div)
#=> "<div />" 
> helper.content_tag(:div)
#=> "<div></div>" 
```

##### How does form_tag work?
form_tag takes a block representing all the inputs to the form and takes care of the hidden authenticity stuff for you.

Can stuff any content tags really inside your form_tag

##### Do you need the authenticity token for GET requests?
Not need for GET requests

##### How would you add a class onto the form generated by form_tag?
Use the options hash as last parameter to the tag.  Looks like:

```ruby
:class => "class_name"
```

##### How would you add a class onto an input generated by a *_tag?
Use the same options hash above but include it inside the content tag inside form_tag.

##### What is the main difference between form_tag and form_for?
form_for gives you shortcuts for routing and eliminates alot of the path coding.

##### How does form_for know whether your model object is saved or not?
Just checks to see if the object is persisted or not.

##### What is a "form builder" object?
Is the arg that follows form_for do as below:

```ruby
# app/controllers/articles_controller.rb
def new
  @article = Article.new
end

#app/views/articles/new.html.erb
# f is the form builder object
<%= form_for @article do |f| %>
  <%= f.text_field :title %>
  <%= f.text_area :body, size: "60x12" %>
  <%= f.submit "Create" %>
<% end %>
```

##### What format do inputs send from a form_for form arrive in your controller as?
Come in the same as they come in in form_tag.  As strings and are nested under the object (f) passed in by form_for.

##### How is adding an HTML class to your form_for form different from the form_tag form?
Need to be properly name-spaced inside a sub-hash

```ruby
<%= form_for @post, {:html => { :class => "your_class" } } do |f| %>
```

The nested *_tag helpers can take classes like they did inside the form_tag element.

### Active Record Validations

##### What are the three major levels of form validation?
Topmost level: write JS that validates if someone has filled out form properly

2nd layer: server level validation

3rd layer: database level

##### Why are JavaScript validations useful?
Great for user experience b/c feedback is instant

##### Why are JS validations not enough?
Can be easily circumvented

##### Why are database validations necessary beyond just application-level validations?
Can guarantee uniqueness using indices and so forth

##### How do you manually run an object's validations?
By calling the valid? method.  Can also run persisted? and new_record? methods to help validate.

##### What happens if one fails?
An error is raised. 

##### How do you only run a validation on a specific controller method?
Using the :on attribute, like the following:

```ruby
# app/models/post.rb
class Post < ActiveRecord::Base
  validates :title, 
            :presence => true, 
            :on => :create  #<<<<<<<<
end

# if you want to raise an exception insetad of adding errors to the model object use :strict => true
# app/models/post.rb
class Post < ActiveRecord::Base
  validates :title, 
            :presence => true, 
            :strict => true
end
```

##### How would you manually skip validations when saving an object?
?????  

There are methods that skip validation

##### How do you register validations?
Run the validates method atop the model.

```ruby
# app/models/post.rb
class Post < ActiveRecord::Base
  validates :title, :presence => true
end
```

##### How do you add a custom validation message?
In the validates method you can pass options like you would any other hash.  See the following:

```ruby
# app/models/post.rb
class Post < ActiveRecord::Base
  validates :title, 
            :presence => true, 
            :message => "Need a title, dude!"
end
```

##### How do you allow an attribute to contain blank or nil values?
Just pass that option in as well in your validates method

```ruby
# app/models/post.rb
class Post < ActiveRecord::Base
  validates :title, 
            :presence => true, 
            :message => "Need a title, dude!",
            :allow_blank => true,
            :allow_nil => true
end
```

##### How do you register multiple fields for validation with one line?
By passing them in separated by commas

```ruby
# app/models/post.rb
class Post < ActiveRecord::Base
  validates :title, :body, :author_name,
            :presence => true
end
```

##### How do you validate the presence of a field?
Add the presence hash, indicates the attribute is required.

```ruby
# app/models/post.rb
class Post < ActiveRecord::Base
  validates :title, 
            :presence => true 
end
```

##### How do you validate the length of a field?
Add the uniqueness option which checks the if hte index is unique

```ruby
# app/models/user.rb
class User < ActiveRecord::Base
  validates :email, 
            :uniqueness => true
end
```

##### How do you validate the length of a field?
Add the length key/value hash and input what you want to validate too.

```ruby
class Post < ActiveRecord::Base
  validates :title, 
            :length =>{ :maximum => 40,
                        :minimum => 10,
                        :in => 10..40, # same as above
                        :is => 16 } 
end
```

##### How do you validate that a field value is within an acceptable subset?
Add in the inclusion hash with options array

```ruby
# app/models/student.rb
class Student < ActiveRecord::Base
  validates :role, 
            :inclusion => [ "Team Leader", 
                            "Class President", 
                            "Student" ]
end
```

##### How to you validate the format of a field?
Can be done with REGEX and other ways.

```ruby
# app/models/user.rb
class User < ActiveRecord::Base
  validates :email, 
            :format => { :with => /@/ }
end
```

##### What happens when you fail to validate an object?
It'll pass along unscathed regardless if it fits or not. The DB may still reject it.

???? [On This](https://www.vikingcodeschool.com/dashboard#/basic-forms-and-active-record/active-record-validations)
While a method gives you the most flexibility, you can also (particularly in this simple example) just use a Proc:

```ruby 
# app/models/student.rb
class Student < ActiveRecord::Base
  validates :sid, 
            :presence => true, 
            :if => Proc.new{ registered }
end
```

##### How do you access the errors on an object?
There are a number of methods you can use to access the methods object and its information.  

```ruby
# app/models/post.rb
class Post < ActiveRecord::Base
  validates :title, :length => { :maximum => 20 }
end

p = Post.new
#=> #<Post id: nil, title: nil, body: nil, created_at: nil, updated_at: nil> 
> p.title = "short title"
#=> "short title" 
> p.valid?
#=> true 
> p.title = "very super duper long title"
#=> "very super duper long title" 
> p.valid?
#=> false 
> p.errors
#=> #<ActiveModel::Errors:0x007f9df7da9778 @base=#<Post id: nil, title: "very super duper long title", body: nil, created_at: nil, updated_at: nil>, @messages={:title=>["is too long (maximum is 20 characters)"]}> 
> p.errors[:title]
#=> ["is too long (maximum is 20 characters)"] 
> p.errors.full_messages
#=> ["Title is too long (maximum is 20 characters)"] 
```

##### How do you access the errors for a specific attribute of an object?
Select the errors object and provide it an attribute key to access the data there

```ruby
errors[:attribute] 
# ex
errors[:username]
```

errors.full_messages gives you an array populated with all the error messages

##### What's the high level process for writing your own custom validator?
Just have to inherit from ActiveModel::Validator class. Will provide errors messages to the errors object. Follow the following blueprint.

```ruby
class MyValidator < ActiveModel::Validator
  def validate(record)
    unless record.name.starts_with? 'X'
      record.errors[:name] << 'Need a name starting with X please!'
    end
  end
end

class Person
  include ActiveModel::Validations
  validates_with MyValidator
end
```

#####CODE REVIEW

```ruby
@user.persisted?
@user.new_record?
@user.valid?

# Sample validation with all the options
validates :title, :body, :subheading
            :presence => true, 
            :uniqueness => true,
            :length =>{ :maximum => 40,
                        :minimum => 10,
                        :in => 10..40, # same as above
                        :is => 16 },
            :inclusion => [ "Team Leader", 
                            "Class President", 
                            "Student" ],
            :format => { :with => /@/ },
            :on => :create,
            :strict => true,
            :message => "didn't work!",
            :if => :some_method_returns_true,
            :unless => Proc.new{ self.registered }

# Skipping validation
@user.save(:validate => false)

# Accessing errors
@user.errors                # ActiveModel::Errors object
@user.errors[:name]         # Array (just one string)
@user.errors.full_messages  # Array (potentially many strings)
```

### Handling Validation Errors

##### How do you get the count of error messages on your failed model object?
Access the errors hash in the view to inform user of errors.

When a create fails per say, you render the /new page and the errors object is passed in. Attaches it to the object being passed.

```ruby
# app/views/posts/new.html.erb
<% if @post.errors.any? %>
  <div id="error_explanation">

    # Check out that pluralize method! Cool, no?
    <h2><%= pluralize(@post.errors.count, "error") %> prohibited this post from being saved:</h2>

    <ul>
    <% @post.errors.full_messages.each do |msg| %>
      <li><%= msg %></li>
    <% end %>
    </ul>
  </div>
<% end %>
```

##### How do you pluralize a string in Rails?
Use the pluralize method as seen above ^

##### How can you get a list of form errors to display atop the form?
Add conditional logic that adds to the page at the top, generally above the 

```ruby
# app/views/posts/new.html.erb
<% if @post.errors.any? %>
  <div id="error_explanation">

    # Check out that pluralize method! Cool, no?
    <h2><%= pluralize(@post.errors.count, "error") %> prohibited this post from being saved:</h2>

    <ul>
    <% @post.errors.full_messages.each do |msg| %>
      <li><%= msg %></li>
    <% end %>
    </ul>
  </div>
<% end %>
```

##### How can you display an error next to each field?
Add conditional logic local to the field and include the name of the field as the key to the errors object hash.

```ruby
# app/views/posts/new.html.erb
...
<%= form_tag posts_path do %>
  <% unless @post.errors[:title].empty? %>
    <div class="error">
      <%= "Post #{@post.errors[:title].first}" %>
    </div>
  <% end %>
  <%= text_field_tag :title %>
  ...
<% end %>
```

##### How might you abstract that into a view helper?

```ruby
# app/helpers/posts/posts_helper.rb
module PostsHelper
  def field_with_errors(object,field)

    # No errors if no errors!
    if object.errors[field].empty?
      error = ""
    else
      # Otherwise, create an error <div> around the message
      error = content_tag(:div, :class=>"error") do
        field.to_s.titleize + " " + object.errors[field].first
      end
    end
    # Combine our normal input tag with the error message
    text_field_tag(field) + error
  end
end
```

##### What does form_for do for you when you have errors?
You can attach the form_for's reference object into the helper method above. 

```ruby
# app/views/posts/new.html.erb
...
<%= form_tag posts_path do %>
  ...
  <%= field_with_errors(@post,:title) %>
  ...
<% end %>
```

And it intelligently maps errors to the correct field and creates a special div with the field_with_errors class (for styling in css). Only provides additional wrappers but doesn't tack in the message itself.  Only marks where in the form the error is.

If the @post fails to save with:

```ruby
# app/views/posts/new.html.erb
...
<%= form_for @post do |f| %>
  ...
  <%= f.text_area :body %>
  ...
<% end %>
```

The form_for renders:

```html
<form accept-charset="UTF-8" action="/posts" class="new_post" id="new_post" method="post">
...
  <div class="field_with_errors">
    <textarea id="post_body" name="post[body]">
      Lorem Ipsum body text
    </textarea>
  </div>
...
</form>
```