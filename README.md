# javascript-decorators

[![npm version](https://badge.fury.io/js/javascript-decorators.svg)](https://badge.fury.io/js/javascript-decorators)

[![Build Status](https://travis-ci.org/AvraamMavridis/javascript-decorators.svg?branch=master)](https://travis-ci.org/AvraamMavridis/javascript-decorators)

Common helpers using es7 decorators. 

You can see the [Changelog](#Changelog) for the version specific changes.

![Splitline](http://www.centrosanisidoro.es/wp-content/themes/simplegridtheme/images/banner.png "Splitline")

## How to use

[![NPM](https://nodei.co/npm/javascript-decorators.png?compact=true)](https://nodei.co/npm/javascript-decorators/)

`npm i --save javascript-decorators`

And then to use a decorator just import it:

```js
import { log } from 'javascript-decorators'

class Person{
  @log()
  doSomething(){
    ...
  }
}
```

## Decorators

###Method decorators:

#### Validation related decorators

`@validateSchema( schema )` :  Executes the function only if the schema is valid, 

*schema: object*

`@acceptsObject( positions, failSilent )`      :  Executes the function only if the passed arg is an object 

*positions: number|array (optional, default 0), failSilent: boolean(optional, default false)*

`@acceptsArray( positions, failSilent )` :  Executes the function only if the passed arg is an array


`@acceptsInteger( positions, failSilent )`:   Executes the function only if the passed arg is an integer


`@acceptsNumber( positions, failSilent )`:  Executes the function only if the passed arg is a number


`@acceptsBoolean( positions, failSilent )`:  Executes the function only if the passed arg is a boolean



`@acceptsFunction( positions, failSilent )`:  Executes the function only if the passed arg is a function


`@acceptsString( positions, failSilent )`:  Executes the function only if the passed arg is a string


`@acceptsPromise( positions, failSilent )`:  Executes the function only if the passed arg is a promise


#### Immutability related decorators

`@immutable()`:  Makes a deepcopy of the passed arguments and executes the method with the copy to ensure that the initial parameters are not mutated

`@doesNotMutate()` :  Executes the method only if it doesnt mutate the passed arguments. Useful when the class extends another class and/or calls methods from the parent.

#### Performance related decorators

`@memoization()` :  Use the [memoization](https://en.wikipedia.org/wiki/Memoization) technique to prevent expensive function calls.

#### Debugging & Developing decorators

`@log()`: Logs the passed values and the returned result.

`@loglocalstorage()` : Logs the size of the localstorage before and after the function call.

`@donotlog()` : Do not log messages, errors and warnings.

`@donotlogmessages()` : Do not log messages.

`@donotlogwarnings()` : Do not log warnings.

`@donotlogerrors()` : Do not log errors.

### Time related decorators

`@timeout( ms )` (alias: `@debounce( ms )`)  : Executes the function after the specified wait (default 300ms)

*ms: number*

`@defer()`  : Executes the function when the current call stack has cleared

### Execution related decorators

`@once()` : Executes the function only once. Repeat calls return the value of the first call.

`@times(n)` : Executes the function at most `n` times. Repeat calls return the value of the **nth** call.

*n: number*

`@timesCalled()` : Attaches a `timesCalled` property to the function that indicates how many times the function has been called.

###Method & Parameter decorators:
`@readony()`,  `@enumerable()`, `@nonenumerable()`, `@configurable()`, `@nonconfigurable()`

![Splitline](http://www.centrosanisidoro.es/wp-content/themes/simplegridtheme/images/banner.png "Splitline")

## <a name="Changelog"></a>Changelog 

2016-01-08 Version 0.3.1
==========

  * Fix typo on package.json
  * Update Readme
  * Helper to validate that isFunction for method decorators
  * Fix export alias for @debounce
  * @defer decorator


2016-01-07 Version 0.3.0
==========

  * Update README
  * Update package.json
  * Eslintignore file
  * Accepts a parameter in validation decorators indicating if they will fail silently
  * @readonly, @configurable, @nonconfigurable, @enumerable, @nonenumerable decorators
  * Small refactor on export


2016-01-06 Version 0.2.2
==========

  * @timesCalled decorator
  * @once and @times decorators
  * Fix color on console.log of @loglocalstorage
  * Always return the descriptor to be able to chain decorators
  * Fix package.json
  * Update package.json
  * Update README
  * npmignore, index entry file, update package
  * Small refactor
  * @timeout, @debounce decorators
  
![Splitline](http://www.centrosanisidoro.es/wp-content/themes/simplegridtheme/images/banner.png "Splitline")

## Examples

### <a name="@validateSchema"></a>@validateSchema

Executes the method only if the passed values are valid according to the provided **schema**. Example:

```js

class Person{
  @validateSchema( { age: 'number', childs: 'object', jobs: 'array', name: 'string' } );
  setPersonDetails( obj ) { ... }
}

  ```

### <a name="@acceptsObject"></a>@acceptsObject

Executes the method only if the passed value is an object otherwise throws an exception, accepts position (default=0) or array of positions. Example:

  ```js

class Person{
  @acceptsObject( [0,2], true );
  doSomething( obj, str, obj ) { ... }
}

  ```

### <a name="@immutable"></a>@immutable

Makes a deepcopy of the passed arguments and executes the method with the copy to ensure that the initial parameters are not mutated. Example:

  ```js

class Person{
  @immutable();
  doSomething( arg1 ) {
     arg1.test = 10;
  }
}

var obj = { test: 5 };
var p = new Person();
p.dosomething( obj );
console.log( obj.test ); // 5;
  ```

### <a name="@doesNotMutate"></a>@doesNotMutate

Executes the method only if it doesnt mutate the passed arguments. Useful when the class extends another class and/or calls methods from the parent. Example:

  ```js

class Person{
  @doesNotMutate();
  doSomething( arg1 ) {
     arg1.test = 10;
  }
}

var obj = { test: 5 };
var p = new Person();
p.dosomething( obj ); // throws an exception because doSomething mutates the passed object.
  ```

### <a name="@memoization"></a>@memoization

  ```js

class Person{
  @memoization();
  doSomethingTimeConsuming( arg1 ) {
     arg1.test = 10;
  }
}

var obj = { test: 5 };
var p = new Person();
p.doSomethingTimeConsuming( obj ); // calls the function
p.doSomethingTimeConsuming( obj ); // immediately returns the result without calling the function
  ```

### <a name="@log"></a>@log

  ```js

class Person{
  @log();
  doSomething(...args){
    return args.join('-');
  }
}

var p = new Person();
p.doSomething( 2 , 3 );
  ```
Will log in the console:

![Logged on console](http://oi64.tinypic.com/120ta1c.jpg "Logged on console")


### <a name="@loglocalstorage"></a>@loglocalstorage

  ```js

class Person{
  @loglocalstorage()
  doSomething(){
    localStorage.setItem('somethingonlocalstorage', "Lorem ipsum dol'or sit amet, consectetur adipiscing elit. Morbi congue urna id enim lobortis placerat. Integer commodo nulla in ipsum pharetra, ac malesuada tellus viverra. Integer aliquam semper nisi vehicula lobortis. Ut vel felis nec sem sollicitudin eleifend. Ut sem magna, vehicula in dui in, sodales ultrices arcu. Aliquam suscipit sagittis aliquet. Phasellus convallis faucibus massa, quis tincidunt nisl accumsan sed.")
  }
}

var p = new Person();
p.doSomething( );
  ```
Will log in the console something like:

![Logged on console](http://oi68.tinypic.com/n33tzo.jpg "Logged on console")


### <a name="@timeout"></a>@timeout (alias: @debounce)

  ```js

class Person{
  @timeout( 2000 );
  doSomething(){
    ...
  }
}

var p = new Person();
p.doSomething(); // Executed after 2secs
  ```


### <a name="@once()"></a>@once()

  ```js

class Person{
  @once();
  doSomething(a, b){
    return a + b;
  }
}

var p = new Person();
p.doSomething(1,2); // returns 3
p.doSomething(21,2); // keeps returning 3
p.doSomething(14,42); // keeps returning 3
  ```

### <a name="@times(n)"></a>@times(n)

  ```js

class Person{
  @times(2);
  doSomething(a, b){
    return a + b;
  }
}

var p = new Person();
p.doSomething(1,2); // returns 3
p.doSomething(21,2); // returns 23
p.doSomething(14,42); // keeps returning 23
p.doSomething(14,542); // keeps returning 23
  ```

### <a name="@timesCalled()"></a>@timesCalled()

  ```js

class Person{
  @timesCalled();
  doSomething(a, b){
    return a + b;
  }
}

var p = new Person();
p.doSomething(1,2); // returns 3
p.doSomething(21,2); // returns 23
p.doSomething(14,42); // keeps returning 23
console.log( p.doSomething.timesCalled ); // 3
  ```

