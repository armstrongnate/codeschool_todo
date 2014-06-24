The slideshows can be found here: http://tinyurl.com/losrh8l
# Getting started with our Todo app
<pre>bin/rails new todo</pre>
<pre>cd todo/</pre>
<pre>bin/rails generate model User name:string email:string</pre>

You should have a new model file with the following code:
```ruby
class User < ActiveRecord::Base
end
```

At this point we have created the migration file but we still have to _run_ the  migration.
To run migrations type:
<pre>bin/rake db:migrate</pre>

If this is the first time you have run the migration you should get an output
to the console saying a users table was created.

## Test your new User model:

The rails console allows you to interact with your ruby classes within the rails environment.

In your terminal type:
<pre>bin/rails console</pre>

This will launch the console. Once you are in the console type:

```ruby
User.first
```
That should return `nil`.

To create a a new user type:

```ruby
User.create(name: 'Nate', email: 'nate@custombit.com')
```

Now you should be able to get the first user:

```ruby
User.first
```

### Play!
```ruby
# get a user
user = User.first
user.name # should print the name you set previously
user.email # should print the email you set previously

# update user
user = User.first
user.name = 'Bob'
user.save # should print true

# delete a user
user = User.first
user.destroy # should print true

# create a new user
user = User.new
user.name = 'John'
user.email = 'john@doe.com'
user.save
```

## Validations

We can validate that a user must always have a name
```ruby
# app/models/user.rb
class User < ActiveRecord::Base
  validates :name, presence: true
end
```

* NOTE: if you change something in the model file you must reload your console in order for it to pick up on the changes. *

From the console run:
<pre>reload!</pre>

Now test the new validation:

<pre>user = User.create(email: 'test@test.com')</pre>

The above line should return `false` because we did not include a `name`.

You can test to see if a record is valid or not with:

<pre>user.valid?</pre>

In our case this should return `false`. Why?

Rails allows us to view any errors on a record that might be present.

<pre>user.errors</pre>

This will tell us that the `name` field can not be blank.

## Associations

In this app, a `user` will have many `tasks`. So let's create a new Task model that belongs to a user.

Back in the terminal (exit the rails console by typing `exit`)
<pre>bin/rails generate model Task title user:references</pre>

This created a few files, one main one being the migration file. So, let's run our migrations.

<pre>bin/rake db:migrate</pre>

Now we have a `tasks` table. We must define the relationship within the model classes.

Open up `app/models/task.rb` and app/models/user.rb files and make them look like this:

```ruby
# app/models/task.rb
class Task < ActiveRecord::Base
  belongs_to :user
end

# app/models/user.rb
class User < ActiveRecord::Base
  has_many :tasks
  validates :name, presence: true
end
```

Test the association by opening the console again (`bin/rails console`).
You should now be able to get a users tasks.

<pre>User.first.tasks</pre>

The above code should return an empty array since you have not created any tasks.

* __Note:__ Ensure that you have at least one user by running `User.count`. If the number is zero create a new user (see above). *

### Create a new task that belongs to a user

In the console:
```ruby
user = User.first
Task.create(title: 'Demo', user: user)
```

Now our first user has one task. Confirm by typing:

<pre>User.first.tasks</pre>

That should return an array with one task.

## Add a new column to a table

We want want a way to keep track of whether or not a task has been completed or not. Let's add a `completed` column to our `tasks` table.

Exit the console and type the folling into the terminal.

<pre>bin/rails generate migration AddCompletedToTasks completed:boolean</pre>

Now if you look in `db/migrate` directory there will be a new file created that should look like:

```ruby
class AddCompletedToTasks
  add_column :tasks, :completed, :boolean
end
```

To test that this new column exists, boot up the rails console and type the following:

<pre>Task.first.completed</pre>

* __NOTE:__ You should have at least one task created. *

We can update this column in the console like so:

```ruby
task = Task.first
task.update_attributes(completed: true)
```

## Users Controller

Generate a controller for the User model.

<pre>bin/rails generate controller Users</pre>

Start a rails server:

<pre>bin/rails server</pre>

### Users Controller Index

Open your rails app in a browser at http://localhost:3000/users - You'll get an error that no route matches /users.

Open `config/routes.rb` and add a route.

```ruby
Rails.application.routes.draw do
  get '/users' => 'users#index'
end
```

Open `app/controllers/users_controller.rb` and add an index action.

```ruby
class UsersController < ApplicationController
  def index
    @users = User.all
  end
end
```

Create a file `app/views/users/index.html.erb`

```
<p>Number of users: <%= @users.count %></p>

<ul>
  <% @users.each do |user| %>
    <li>
      <%= user.name %>
      <%= user.email %>
    </li>
  <% end %>
</ul>
```

### Users Controller Show

Open `config/routes.rb` and add a users#show route:

```ruby
get '/users/:id' => 'users#show', as: 'user'
```

Run `bin/rake routes` to see a list of our routes.

Open `app/views/users/index.html.erb` and link the user.

```
<p>Number of users: <%= @users.count %></p>

<ul>
  <% @users.each do |user| %>
    <li>
      <%= user.name %>
      <%= user.email %>
      <%= link_to 'Show', user %>
    </li>
  <% end %>
</ul>
```

Open `app/controllers/users_controller.rb` and add a show action.

```ruby
def show
  @user = User.find(params[:id])
end
```

Create a file `app/views/users/show.html.erb`

```
<h1>Showing <%= @user.name %></h1>
<p><%= @user.email %></p>
```

### Users Controller New

Open `config/routes.rb` and add a users#show route:

```ruby
get '/users/new' => 'users#new'
```

pen `app/views/users/index.html.erb` and link the new action.

```
<p><%= link_to 'New User', users_new_path %></p>
```

Open `app/controllers/users_controller.rb` and add a new action.

```ruby
def new
  @user = User.new
end
```

Create a file `app/views/users/new.html.erb`

```
<h1>New User</h1>

<%= form_for @user do |f| %>
  <p>
    <%= f.label :name %>
    <%= f.text_field :name, placeholder: 'Your Name' %>
  </p>

  <p>
    <%= f.label :email %>
    <%= f.text_field :email, placeholder: 'Your Email' %>
  </p>

  <p><%= f.submit %></p>
<% end %>
```
