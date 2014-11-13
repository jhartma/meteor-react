updated version of luma:react-meteor with ReactJS 0.12.0

react-meteor
============

This repository defines a Meteor package that automatically integrates the
[React](http://facebook.github.io/react/) rendering framework on both the
client and the server, to complement or replace the default Handlebars
templating system.

The React core is officially agnostic about how you fetch and update your
data, so it is far from obvious which approach is the best. This package
provides one answer to that question (use Meteor!), and I hope you will
find it a compelling combination.

Quick start
-----------

If you have not yet installed Meteor, do that:
```
curl https://install.meteor.com | /bin/sh
```

Clone this repository:
```
git clone https://github.com/benjamn/react-meteor.git
```

Fire up one of the examples:
```
cd react-meteor/examples/leaderboard
meteor
```

Finally, visit [localhost:3000](http://localhost:3000) in your browser.
For extra fun, try using the example in multiple browser windows!

Adding the package to your app
------------------------------

Although the Meteor package API is deliberately undocumented and remains
in flux before v1.0, here is how I currently recommend adding this package
to your Meteor app:
```
cd path/to/my-app/
echo react >> .meteor/packages
git clone https://github.com/benjamn/react-meteor.git packages/react
```

If you want to share the package between multiple apps, you can clone it
to a common location and make `packages/react` a symbolic link.

How it works
------------

The package exposes a special `ReactMeteor.Mixin` object that can be used
to enable reactive data fetching for your React components.

To add the `ReactMeteor.Mixin` to a React component, simply include it in
the `mixins` class property:
```js
var MyComponent = React.createClass({
  mixins: [ReactMeteor.Mixin],

  // Make sure your component implements this method.
  getMeteorState: function() {
    return {
      foo: Session.get("foo"),
      ...
    };
  }
});
```
The `getMeteorState` method should return an object of properties that
will be accessed via `this.state` in the component's `render` method or
elsewhere.  Dependencies will be registered for any data accesses
performed by `getMeteorState` so that the component can be automatically
re-rendered whenever the data changes.

Alternatively, if you prefer not to declare `mixins` explicitly, you can
create the class with `ReactMeteor.createClass`:
```js
var MyComponent = ReactMeteor.createClass({
  // Make sure your component implements this method.
  getMeteorState: function() {
    return {
      foo: Session.get("foo"),
      ...
    };
  }
});
```
