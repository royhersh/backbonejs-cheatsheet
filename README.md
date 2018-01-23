# Backbone.js - cheat sheet

## Models

### Define:
```js
var Toys = Backbone.Model.extend({});
```

### class properties:
```js
var Toys = Backbone.Model.extend({},
    {
        purpose: function() {
            return 'Toys are for playing';
        }
    }
);

console.log ( Toys.purpose() ); // Toys are for playing
```

### Instantiating:
> Simple form
```js
var model = new Backbone.Model();
```

> Custom type
```js 
var Toys = Backbone.Model.extend({});
var lego = new Toy();
```

> With property values
```js 
var toy = new Backbone.Model.extend({
    material: 'wood',
    minAge: 3
});
```
#### Initialize 
Initialize constructor runs every time a new model is created:
```js
var Toys = Backbone.Model.extend({
    initialize: function() {
        console.log ('toy created'):
    }
});

var lego = new Toy() // toy created
```

### Inharitance
### Attributes
### Events
### Identity
### Defaults
### Validation
### toJSON
### save, fetch, destroy