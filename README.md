<pre>bin/rails new todo</pre>
<pre>cd todo/</pre>
<pre>bin/rails generate model User name:string email:string</pre>

You should have a new model file with the following code:
```
class User < ActiveRecord::Base
end
```

At this point we have created the migration file but we still have to _run_ the  migration.
To run migrations type:
<pre>bin/rake db:migrate</pre>

If this is the first time you have run the migration you should get an output
to the console saying a users table was created.

### Test your new User model:

The rails console allows you to interact with your ruby classes within the rails environment.

In your terminal type:
<pre>bin/rails console</pre>

This will launch the console. Once you are in the console type:

<pre>User.first</pre>

That should return `nil`.

To create a a new user type:

<pre>User.create(name: 'Nate', email: 'nate@custombit.com')</pre>

Now you should be able to get the first user:

<pre>User.first</pre>

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
