[![Build Status](https://travis-ci.org/rhalff/dot-object.png)](https://travis-ci.org/rhalff/dot-object)

Dot-Object
========

Dot-Object makes it possible to transform javascript objects using dot notation.


#### Move a property within one object to another location
```javascript
var dot = require('dot-object');

var obj = {
  'first_name': 'John',
  'last_name': 'Doe'
};

dot.move('first_name', 'contact.firstname', obj);
dot.move('last_name', 'contact.lastname', obj);

console.log(obj);

{
  contact: {
    firstname: 'John',
    lastname: 'Doe'
  }
}

```

#### Copy property from one object to another
```javascript
var dot = require('dot-object');

var src = {
  name: 'John',
  stuff: {
    phone: {
      brand: 'iphone',
      version: 6
    }
  }
};

var tgt = {name: 'Brandon'};

dot.copy('stuff.phone', 'wanna.haves.phone', src, tgt);

console.log(tgt);

{
  name: 'Brandon',
  wanna: {
    haves: {
      phone: {
        brand: 'iphone',
        version: 6
      }
    }
  }
}

```

#### Transfer property from one object to another

Does the same as copy but removes the value from the source object:

```javascript
dot.transfer('stuff.phone', 'wanna.haves.phone', src, tgt);

// src: {"name":"John","stuff":{}}
// tgt: {"name":"Brandon","wanna":{"haves":{"phone":{"brand":"iphone","version":6}}}
```


#### Transform an object

```javascript
var dot = require('dot-object');

var row = {
  'id': 2,
  'contact.name.first': 'John',
  'contact.name.last': 'Doe',
  'contact.email': 'example@gmail.com',
  'contact.info.about.me': 'classified'
};

dot.object(row);

console.log(row);

{
  "id": 2,
  "contact": {
    "name": {
      "first": "John",
      "last": "Doe"
    },
    "email": "example@gmail.com",
    "info": {
      "about": {
      "me": "classified"
      }
    }
  }
}
```

To convert manually per string use:
```javascript
var dot = require('dot-object');

var tgt = { val: 'test' };
dot.str('this.is.my.string', 'value', tgt);

console.log(tgt);

{
  "val": "test",
  "this": {
    "is": {
      "my": {
        "string": "value"
      }
    }
  }
}
```

#### Pick a value using dot notation:
```
var dot = require('dot-object');

var obj = {
 some: {
   nested: {
     value: 'Hi there!'
   }
 }
};

var val = dot.pick('some.nested.key', obj);
console.log(val);

Hi there!
```

### Using modifiers

You can use modifiers to translate values on the fly.

This example uses the [underscore.string](https://github.com/epeli/underscore.string) library.



```javascript
var dot = require('dot-object');

var _s = require('underscore.string');

var row = {
  'nr': 200,
  'doc.name': '    My Document   '
};

var mods = {
  "doc.name": [_s.trim, _s.underscored],
};

dot.object(row, mods);

console.log(row);
```

```
{
  "nr": 200,
  "doc": {
    "name": "my_document"
  }
}
```

Or using .str() directy:

```javascript

var dot = require('dot-object');
var _s = require('underscore.string');
var obj = { id: 100 };

// use one modifier
dot.str('my.title', 'this is my title', obj, _s.slugify);

// multiple modifiers
dot.str('my.title', '   this is my title  ', obj, [_s.trim, _s.slugify]);

console.log(obj);
```
Result:
```json
{
  "id": 100,
  "my": {
    "title": "this-is-my-title"
  }
}
```

## Using a different seperator

If you do not like dot notation, you are free to specify a different seperator.

```javascript
var Dot = require('dot-object');

var dot = new Dot('->');

var _s = require('underscore.string');

var row = {
  'nr': 200,
  'doc->name': '    My Document   '
};

var mods = {
  "doc->name": [_s.trim, _s.underscored],
};

dot.object(row, mods);

console.log(row);
```

```
{
  "nr": 200,
  "doc": {
    "name": "my_document"
  }
}
```

## Transforming SQL results to JSON

SQL translation on the fly:

```javascript
 // TODO

```


> Copyright © 2013 Rob Halff, released under the MIT license
