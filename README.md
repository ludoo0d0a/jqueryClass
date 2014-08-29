jqueryClass
===========

A Simple, Powerful, Lightweight Class for jQuery

The plugin to use OOP with jQuery.
Credits go to author, Sam Stephenson.
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

# A Basic Class

The following creates a Monster class with a
name (for introspection), static, and prototype members.
Every time a monster instance is created, the static
count is incremented.

```javascript
$.Class.extend('Monster',
/* @static */
{
  count: 0
},
/* @prototype */
{
  init: function( name ) {

    // saves name on the monster instance
    this.name = name;

    // sets the health
    this.health = 10;

    // increments count
    this.Class.count++;
  },
  eat: function( smallChildren ){
    this.health += smallChildren;
  },
  fight: function() {
    this.health -= 2;
  }
});

hydra = new Monster('hydra');

dragon = new Monster('dragon');

hydra.name        // -> hydra
Monster.count     // -> 2
Monster.shortName // -> 'Monster'

hydra.eat(2);     // health = 12

dragon.fight();   // health = 8
```


Notice that the prototype <b>init</b> function is called when a new instance of Monster is created.
 
 
## Inheritance
 
When a class is extended, all static and prototype properties are available on the new class.
If you overwrite a function, you can call the base class's function by calling
<code>this._super</code>.  Lets create a SeaMonster class.  SeaMonsters are less
efficient at eating small children, but more powerful fighters.

```javascript
Monster.extend("SeaMonster",{
 eat: function( smallChildren ) {
   this._super(smallChildren / 2);
 },
 fight: function() {
   this.health -= 1;
 }
});

lockNess = new SeaMonster('Lock Ness');
lockNess.eat(4);   //health = 12
lockNess.fight();  //health = 11
```

### Static property inheritance
 
You can also inherit static properties in the same way:

```javascript
$.Class.extend("First",
{
   staticMethod: function() { return 1;}
},{})

First.extend("Second",{
   staticMethod: function() { return this._super()+1;}
},{})

Second.staticMethod() // -> 2
```

## Namespaces
 
Namespaces are a good idea! We encourage you to namespace all of your code.
It makes it possible to drop your code into another app without problems.
Making a namespaced class is easy:
 
```javascript
$.Class.extend("MyNamespace.MyClass",{},{});

new MyNamespace.MyClass()
```

## Introspection
Often, it's nice to create classes whose name helps determine functionality.  Ruby on
Rails's [http://api.rubyonrails.org/classes/ActiveRecord/Base.html|ActiveRecord] ORM class
is a great example of this.  Unfortunately, JavaScript doesn't have a way of determining
an object's name, so the developer must provide a name.  Class fixes this by taking a String name for the class.

```javascript
$.Class.extend("MyOrg.MyClass",{},{})
MyOrg.MyClass.shortName //-> 'MyClass'
MyOrg.MyClass.fullName //->  'MyOrg.MyClass'
```

The fullName (with namespaces) and the shortName (without namespaces) are added to the Class's
static properties.


## Setup and initialization methods
Class provides static and prototype initialization functions.
These come in two flavors - setup and init.
Setup is called before init and
can be used to 'normalize' init's arguments.

PRO TIP: Typically, you don't need setup methods in your classes. Use Init instead.
Reserve setup methods for when you need to do complex pre-processing of your class before init is called.

```javascript
$.Class.extend("MyClass",
{
 setup: function() {} //static setup
 init: function() {} //static constructor
},
{
 setup: function() {} //prototype setup
 init: function() {} //prototype constructor
})
```

## Setup
Setup functions are called before init functions.  Static setup functions are passed
the base class followed by arguments passed to the extend function.
Prototype static functions are passed the Class constructor function arguments.
If a setup function returns an array, that array will be used as the arguments
for the following init method.  This provides setup functions the ability to normalize
arguments passed to the init constructors.  They are also excellent places
to put setup code you want to almost always run.

The following is similar to how [jQuery.Controller.prototype.setup]
makes sure init is always called with a jQuery element and merged options
even if it is passed a raw
HTMLElement and no second parameter.

```javascript
$.Class.extend("jQuery.Controller",{
 ...
},{
 setup: function( el, options ) {
   ...
   return [$(el),
           $.extend(true,
              this.Class.defaults,
              options || {} ) ]
 }
})
```
Typically, you won't need to make or overwrite setup functions.

## Init

Init functions are called after setup functions.
Typically, they receive the same arguments
as their preceding setup function.  The Foo class's <code>init</code> method
gets called in the following example:

```javascript
$.Class.Extend("Foo", {
 init: function( arg1, arg2, arg3 ) {
   this.sum = arg1+arg2+arg3;
 }
})
var foo = new Foo(1,2,3);
foo.sum //-> 6
```
## Callbacks
Similar to jQuery's proxy method, Class provides a [jQuery.Class.static.callback callback]
function that returns a callback to a method that will always have
<code>this</code> set to the class or instance of the class.

The following example uses this.callback to make sure
<code>this.name</code> is available in <code>show</code>

```javascript
$.Class.extend("Todo",{
 init: function( name ) { this.name = name }
 get: function() {
   $.get("/stuff",this.callback('show'))
 },
 show: function( txt ) {
   alert(this.name+txt)
 }
})
new Todo("Trash").get()
```

Callback is available as a static and prototype method.
 
