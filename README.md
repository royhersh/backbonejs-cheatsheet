# Backbone.js - cheat sheet

## Models

### Define:

```js
var Toys = Backbone.Model.extend({});
```

### class properties:

```js
var Toys = Backbone.Model.extend(
  {},
  {
    purpose: function() {
      return "Toys are for playing";
    }
  }
);

console.log(Toys.purpose()); // Toys are for playing
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
  material: "wood",
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

```js
var A = Backbone.Model.extend({
  initialize: function() {
    console.log("Initialize A");
  },

  toString: function() {
    return JSON.strigify(this.toJSON());
  }
});

var a = new A({
  one: "1",
  two: "2"
});

console.log(a.toString);
// Initialize A
// {"one":"1", "two":"2"}

var B = A.extend({});
var b = new B({
  three: "3"
});
console.log(b.toString());
// Initialize A
// {"three":"3"}

console.log(b instanceof B); // true
console.log(b instanceof A); // true
console.log(b instanceof Backbone.Model); //true
console.log(a instanceof B); // false
```

### Attributes

> Single atribute / Property

```js
var lego = new Toys();
lego.set("material", "plastic");
```

> Multi attributes / Properties

```js
var lego = new Toys();
lego.set ({
    'material', 'plastic',
    'minAge': 3
    });
```

> Read attributes

```js
lego.get("material");
// plastic
```

> escape is same as get but escape html tags

```js
lego.set("description", '<script>alert("script injection");</script>');
lego.escape("description");
// <script>alert("script injection");</script>
```

> Test for attribute

```js
var lego = new Toys();
lego.set("material", "plastic");

lego.has("material");
// true
lego.has("size");
// false
```

### Events

```js
lego.on('change' function () {
    console.log ('something has changed');
});
lego.on('change:type', function () {
    console.log ('type changed');
};

lego.set('color', 'white');
// something has changes

lego.set('type', 'castles');
// type changed
// something has changed
```

> Custom model events

```js
var volcano = _.extend({}, Backbone.Events);
volcano.on("disaster:eruption", function(args) {
  console.log("duck and cover - " + args.plan);
});

volcano.trigger("disaster:eruption", { plan: "run" });
// duck and cover - run

volcano.off("disaster:eruption");
volcano.trigger("disaster:eruption", { plan: "run" });
//
```

### Identity

Models get an 'id' property once it persist to a server. until that is has a 'cid' property

```js
var lego = new Backbone.Model({});
console.log(lego.id);
console.log(lego.cid);
// undefined
// c0
```

> Check if model hasn't saved to server yet (has cid but not id)

```js
console.log(lego.isNew());
// true
```

### Defaults

```js
var Vehicle = Backbone.Mode.extend({
  defaults: {
    type: "car"
  }
});

var car = new Vehicle();
car.get("type"); // car
```

### Validation
validate function run on ```save``` and on ```set``` with ```{validation: true}``` as option.  <br/>
When returned ```undefined``` it means the validation pass.  <br/>
For anything else it will emit ```invalid``` event.

```js
var Toys = Backbone.Model.extend({
    validate: function(attrs){
        var materialIsValid = function(material) {
            var validMaterials = ['plastic', 'wood', 'tin'];
            return _(validMaterials).include(material);
        };
        if (!materialIsValid(attrs.material)) {
            return 'Invalid Material';
        }
    }
});

var lego = new Toys();
lego.on('invalid', function(model,error){
    console.log('error:',error);
});

lego.set ('material', 'uranium',{validate: true});
// Invalid Material
```
```isValid()``` is used to programatically run the validate function (in case you did not supply the ```{validation: true}``` to the ```set``` method)
### toJSON
converts a model's attributes to a javascript object.
```js
var lego = new Toys();
lego.set('material', 'plastic');
lego.toJSON(); // {material: 'plastic'}
```
### save, fetch, destroy
Save - Insert or Update to the server

Fetch - update the model with server side state.

Destroy - deletes the model from the server.