jqueryClass
===========

A Simple, Powerful, Lightweight Class for jQuery

The plugin to use OOP with jQuery.
Based on [John Resig’s Simple Class](http://ejohn.org/blog/simple-javascript-inheritance/)

* [jsperf report](http://jsperf.com/class-inherit-method-call/2)
* [Source fron bitovi blog](http://bitovi.com/blog/2010/06/a-simple-powerful-lightweight-class-for-jquery.html)

Features
===========
* Static and prototypal inheritance
* Introspection
* Namespaces
* Setup and initialization methods
* Easy callback creation


Usage
===========
To create a monster class with static and prototype properties:

```javascript
$.Class("Monster",
// static properties
{
  count: 0  
},
// prototype properties
{
  // constructor function
  init : function(name){
    //save the name
    this.name = name;

    this.energy = 10;

    //increment the static count
    this.Class.count++;
  }
})

//create a monster
var dragon = new Monster('dragon');
```
