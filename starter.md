# Starter code for Thursday June 25 Demo

We are going to continue with our Todo app by creating a user interface (UI) for creating new todo items and checking them off, marking them as complete.

To accomplish this we are going to include Bootstrap and Angular into our app to make it look good and also user friendly.

Because there is a lot of initial setup required I have provided some code that you can simply copy and paste into your project.

## Step 1: Download Angular and Bootstrap

The files you will need are in Dropbox and can be downloaded from [here](https://www.dropbox.com/sh/dfa7pnsg6twuo71/AAAJkRBmYgdBOxO8yHZafXfia).

Once downloaded:
* copy the javascript (*.js) files to `vendor/assets/javascripts/`
* copy `bootstrap.css` to `vendor/assets/stylesheets/`

Now you need to tell `application.css` and `application.js` to load these added files.

Add the following lines to `application.js` (found in the `app/assets/javascripts` directory):

```javascript
//= require angular
//= require angular-resource
```
_You can add these two lines just above the `require_tree .` line._

Do the same for `application.css` (found in the `app/assets/styleesheets` directory):

```css
*= require bootstrap
```
_You can add this line just above the `require_tree .` line._

**DO NOT RENAME THE FILES FROM THEIR ORIGINAL NAMES**

## Step 2: Javascript File for Tasks

Create a file in `app/assets/javascripts` called `tasks.js`. If `tasks.js.coffee` already exists, rename it to `tasks.js` instead, removing the `.coffee` extension.

In `tasks.js` add the following code:

```javascript
app = angular.module('todo', ['ngResource']);

$(document).on('page:load', function() {
  angular.bootstrap(document.body, ['todo']);
});

app.config([
  '$httpProvider', function($httpProvider) {
    $httpProvider.defaults.headers.common['X-CSRF-Token'] = $('meta[name=csrf-token]').attr('content');
    $httpProvider.defaults.headers.common['X-Requested-With'] = 'XMLHttpRequest';
  }
]);

app.factory('Task', function($resource) {
  var task = $resource('/tasks/:id', { id: '@id' }, {
    update: { method: 'PATCH' }
  });
  return task;
});
```

## Step 3: Stylesheet for Tasks

Create a file in `app/assets/stylesheets` called `tasks.css`. If `tasks.css.scss` already exists, rename it to `tasks.css` instead, removing the `.scss` extension.

In `tasks.css` add the following code:

```css
.panel {
  margin-top: 50px;
}

.list-group {
  margin-top: 20px;
}

.list-group-item .text-muted {
  text-decoration: line-through;
}
```

## Step 4: Tasks Index

In `app/views/tasks/index.html.erb`, delete any code that might already exist in this file or create this file if it does not yet exist and copy the below into it:

```
<div>
  <div class='col-md-4 col-md-offset-4'>
    <div class='panel panel-default'>
      <div class='panel-heading'>
        <h3 class='panel-title'>Tasks</h3>
      </div>
      <div class='panel-body'>
        <div class='row'>
          <div class='col-md-10'>
            <input type='text' placeholder='Add a new task..' class='form-control' />
          </div>
          <div class='col-md-2'>
            <button class='btn btn-primary'>add</button>
          </div>
        </div>
        <div class='row'>
          <div class='col-md-12'>
            <!-- tasks will go here -->
          </div>
        </div>
      </div>
    </div>
  </div>
</div>
```

