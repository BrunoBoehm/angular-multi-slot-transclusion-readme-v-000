# Multi-slot transclusion

## Overview

One other awesome thing that transclusion offers is the concept of multi-slow transclusion. This allows us to pick at different elements in our transclusded content and put them wherever we like.

## Objectives

- Describe the new multi-slot transclude feature
- Create multiple transclusion slots
- Create named transclusion slots
- Define optional transclusion slots

## transclude: {}

Instead of a string or a boolean value, to do multi-slot transclusion, we pass through an object.

The properties on this object are what we want to name our slots. For instance, if we've got this setup for our directive:

```html
<user-profile>
    <h4>Tim Cook</h4>
    <h6>CEO, Apple</h6>
</user-profile>
```

You can see here that our `<h4>` element is the name and our `<h6>` is the position. Therefore, it makes sense to name our slots `name` and `position`.

We then put the values of our properties to be the element that we're targeting. We'd end up with something like this:

```js
function UserProfile() {
  return {
    transclude: {
        name: 'h4',
        position: 'h6'
    },
    template: [
      '<div class="user">',
        '<h2>User Profile</h2>',
      '</div>'
    ].join('')
  };
}

angular
  .module('app', [])
  .directive('userProfile', UserProfile);
```

We can now access these elements using the familiar `ng-transclude` directive. We pass in the slot name that we want as the value.

```js
function UserProfile() {
  return {
    transclude: {
        name: 'h4',
        position: 'h6'
    },
    template: [
      '<div class="user">',
        '<h2>User Profile</h2>',
        '<div>Name: <span ng-transclude="name"></span></div>',
        '<div>Position: <span ng-transclude="position"></span></div>',
      '</div>'
    ].join('')
  };
}

angular
  .module('app', [])
  .directive('userProfile', UserProfile);
```

You can see how we can put our slots anywhere we like in our directive. Also, we don't have to match the tags that we transclude - you can see that although we've picked out `<h4>` and `<h6>` we're transcluding them inside of a `<span />` element (this does put our header elements *inside* our span element, not just the text).

## Optional slots

Now, if you remove either the `<h4>` or `<h6>`, you will get an error as we're expecting there to be both of them.

We can mark slots as optional, too. Let's make position optional, as everyone has a name but people might not have a position. To make a slot as optional, inside of our string we add a `?` before the element.

```js
function UserProfile() {
  return {
    transclude: {
        name: 'h4',
        position: '?h6'
    },
    template: [
      '<div class="user">',
        '<h2>User Profile</h2>',
        '<div>Name: <span ng-transclude="name"></span></div>',
        '<div>Position: <span ng-transclude="position">No position</span></div>',
      '</div>'
    ].join('')
  };
}

angular
  .module('app', [])
  .directive('userProfile', UserProfile);
```

Now we use our directive with and without specifying a `<h6>` element. If there isn't a `<h6>` element, the contents of our `<span ng-transclude="position">` will be displayed instead, meaning we can gracefully show different content if there is no position specified.