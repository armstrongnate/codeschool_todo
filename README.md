`bin/rails new todo`
`cd todo/`
`bin/rails generate model User name:string email:string`

You should have a new model file with the following code:
```
class User < ActiveRecord::Base
end
```

`bin/rake db:migrate`

If this is the first time you have run the migration you should get an output
to the console saying a users table was created.

### Test your new User model:

The rails console allows you to interact with your ruby classes within the rails environment.

Type `bin/rails console` in your terminal.

This will launch the console. Once you are in the console type:

<pre>User.first</pre>

That should return `nil`.

To create a a new user type:

<pre>User.create(name: 'Nate', email: 'nate@custombit.com')</pre>

Now if you type `User.first` you should not get nil but instead get a user object.

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
