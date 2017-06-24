# js-the-good-parts
Summary for the book

## Objects
Beside numbers,strings, true, false, null and undefined everything is an Object.
Numbers, strings and booleans are object-like but immutable.
arrays,functions,REG and objects are objects.
Objects are class-free: so every object can have any property name. 
Objects are passed by refrence. They are never copied.

```node
let foo = obj.bar || 5 //to set default value
let foo = obj.foo && obj.foo.bar //incase obj.foo is undefined
```
Every object is linked to a prototype object which it can inherit properties.
Objects created as literals are linked to Object.prototype which comes with JS.
 
You can choose the prototype of your new object like this:
```node
let myObj = Object.create(someOtherObj)
```
The prototype link is used only in retirval: If we try to retrieve `foo` from `myObj`  JS will try to retireve it from `myObj` and if lacks it, JS will try to get it from its prototype and so on.

## Functions
Functions are objects. They are linked to `Function.prototype` which itself linked to `Object.prototype`.
Function are created with `prototype` property: its value is an object with a `constructor` property whose value is a function.
In addition to the declared parameters of the function there are 2 more: `this` and `arguments`.
`arguments` is an array-like objects which contains all the args given to the function.
`this` value is depended on the way the function was invoked:
### The Method Invocation Pattern
When a function is a property of an object, we call it a method. When a method is invoked the `this` is bound to the object.
### The Function Invocation Pattern
If it's not a property of an object, then we say it is invoked as a function. Now `this` is bounded to the global object.
### The Constructor Invocation Pattern
If a function is invoked with the `new` prefix then `new Foo()` is an object with an hidden link to the value of the function prototype, and `this` will be bound to that new object (`new Foo()`)

This gives us the ability to do something like this:
```node
let Quo = function(string) { this.status=string}
Quo.prototype.getStatus = function(){return this.status}
let myQuo = new Quo("Single")
myQuo.getStatus() // Single
```
### The Apply Invocation Pattern
Any function has the method `apply` which gives the ability to invoke the function with an object that will be bounded to `this` and an array of arguments to send to the function.

This is useful because if we have an object with `status` property, and we want to print it we can use out `getStatus` method this way:
```node
Quo.prototype.getStatus.apply({status:'in a relationship'}); //in a relationship
```
### Exceptions
```node
throw {name:'MyError', description: 'need this and that'} //to throw one

//to catch errors
try { 
      //something
    }
catch (error) {
      //error should have name property 
    }
```

### Augmenting Types
Because of the nature of the prototypes we can easly augment types:

```node 
String.prototype.trim = function(){return this.replace(/^\s+|\s+$/g,'');}
```
Now all objects with a String prototype will have the trim property.
### Cascade
By returning `this` we can enable cascades:
```node
getElement('myDom').move(300,400).color('red').on('click',doThat).later(2000, function(){ this.color('green')});
```
Very expressive and can help control the tendency to make interfaces that try to do too much at once.
### Curry
Suppose we have a function `f(a,b)` and we want to make a function `g(b)` which equals `f(CONST,b)`:
```node
Function.prototype.curry = function(){
                             let slice = Array.prototype.slice;
                             let args=arguments;
                             let that=this;
                             return function(){return that.apply(null, args.contact(slice.apply(arguments)));};
                             });
                           
```
Now we can do:
```node
let f=function(a,b){return a+b});
let plus5 = f.curry(5);
plus5(5) //10
```

## Inheritance
JS being a loosely typed language never casts: The lineage of an object is irrelevant. What matters about an object is what it can do, not what it is descended from.
In classical languages, objects are instances of classes, and a class can inherit from another class. JS is a prototypical language which means objects inherit directly from other objects.
### Pseudoclassical
```node
//create a "class"
let Mammal = function (name){
                      this.name=name
             }
Mammel.prototype.getName = function(){return this.name}
Mammel.prototype.say = function(){return this.saying || ''}
```

```node
let myMammel = new Mammal('cutie');
myMammel.getName() // cutie
```

Now we want to inherit from the `Mammel`:

```node
let Cat = function(name){this.name=name; this.saying = 'mooo'}
Cat.prototype = new Mammel();
``` 
This should look like a class-like inheritance but it is looking quite alien.
We can do:
```node
Function.prototype.inherits = function(Parent){ this.prototype = new Parent(); return this}
```

This is useful for programmers who are unfamilier with JS. JS provides better code reuse patterns:
### Object Specifiers
It's cleaner to pass to a constructor a config obj rather than arguments. This way you don't have to commit to their order.
```node
let obj = maker(a,b,c,d,e,f); //instead of this
let obj = maker({a,b,c,d}); //do this
```
### Prototypal
We forget about class: an object can inherits the properties of other one
```node
let myMammel = {name,get_name,says};
let myCat = Object.create(myMammel);
myCat.purr = function(){console.log('grrrr')};
```
This is differential inheritance. We spcifiy the differences from the object on which it is based.
### Functional
What we've seen till now doesn't allow us privacy.
```node
//TODO
```
### Parts
```node
//TODO
```
## Arrays
Arrays are not really arrays, they are just objects.
So
```node
let a=['a','b','c']
```
is really like
```node
{'0':'a', '1':'b', '2':'c'}
```
The difference is the arrays inherits from Array.prototype so it comes with more methods. and has a secret `length` property.

So use arrays when you need objects with 0,1,2,3 properties...

## Regular Expressions
```node
//TODO
```
