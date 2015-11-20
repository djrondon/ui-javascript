# Maple Inside UI JavaScript Style Guide

> Efficient and readable JavaScript adapted to UI businesses.

Other style guides:
 - [React](react)
 - [CSS & Sass](https://github.com/mapleinside/css)
 
This style guide is inspired by the very complete [Airbnb JavaScript style guide](https://github.com/airbnb/javascript).
 
## Table of Contents

  1. [Types](#types)
  1. [References](#references)
  1. [Objects](#objects)
  1. [Arrays](#arrays)
  1. [Destructuring](#destructuring)
  1. [Strings](#strings)
  1. [Functions](#functions)
  1. [Arrow Functions](#arrow-functions)
  1. [Constructors](#constructors)
  1. [Modules](#modules)
  1. [Iterators and Generators](#iterators-and-generators)
  1. [Properties](#properties)
  1. [Variables](#variables)
  1. [Hoisting](#hoisting)
  1. [Comparison Operators & Equality](#comparison-operators--equality)
  1. [Blocks](#blocks)
  1. [Comments](#comments)
  1. [Whitespace](#whitespace)
  1. [Commas](#commas)
  1. [Semicolons](#semicolons)
  1. [Type Casting & Coercion](#type-casting--coercion)
  1. [Naming Conventions](#naming-conventions)
  1. [Accessors](#accessors)
  1. [Events](#events)
  1. [ECMAScript 5 Compatibility](#ecmascript-5-compatibility)
  1. [ECMAScript 6 Styles](#ecmascript-6-styles)
  1. [Testing](#testing)
  1. [License](#license)
  1. [Amendments](#amendments)

## Types

  - [1.1](#1.1) <a name="1.1"></a> **Primitives**: when you access a primitive type you work directly on its value.

    + `string`;
    + `number`;
    + `boolean`;
    + `null`;
    + `undefined`.

    ```javascript
    const foo = 1;
    
    let bar = foo;

    bar = 9;

    console.log(foo, bar); // 1, 9
    ```
    
  - [1.2](#1.2) <a name="1.2"></a> **Complex**: when you access a complex type you work on a reference to its value.

    + `object`;
    + `array`;
    + `function`.

    ```javascript
    const foo = [1, 2];
    const bar = foo;

    bar[0] = 9;

    console.log(foo[0], bar[0]); // 9, 9
    ```

**[Back to top](#table-of-contents)**

## References

  - [2.1](#2.1) <a name="2.1"></a> Use `const` for all of your references; avoid using `var`.

    > Why? This ensures that you can't reassign your references, which can lead to bugs and difficult to comprehend code.
    
    ```javascript
    // Bad.
    
    var a = 1;
    var b = 2;
    
    // Good.
    
    const a = 1;
    const b = 2;
    ```

  - [2.2](#2.2) <a name="2.2"></a> If you must reassign references, use `let` instead of `var`.

    > Why? `let` is block-scoped rather than function-scoped like `var`.

    ```javascript
    // Bad.
    
    var count = 1;
    
    if (true) {
      count += 1;
    }

    // Good, use let.
    
    let count = 1;
    
    if (true) {
      count += 1;
    }
    ```

  - [2.3](#2.3) <a name="2.3"></a> Note that both `let` and `const` are block-scoped.

    ```javascript
    // Const and let only exist in the blocks they are defined in.
    
    {
      let a = 1;
      const b = 1;
    }
    
    console.log(a); // ReferenceError
    console.log(b); // ReferenceError
    ```

**[Back to top](#table-of-contents)**

## Objects

  - [3.1](#3.1) <a name="3.1"></a> Use the literal syntax for object creation.

    ```javascript
    // Bad.
    const item = new Object();

    // Good.
    const item = {};
    ```

  - [3.2](#3.2) <a name="3.2"></a> If your code will be executed in browsers in script context, don't use [reserved words](http://es5.github.io/#x7.6.1) as keys. It won't work in IE8. [More info](https://github.com/airbnb/javascript/issues/61). It’s OK to use them in ES6 modules and server-side code.

    ```javascript
    // Bad.
    const hero = {
      default: {sylvanas: 'windrunner'},
      private: true,
    };

    // Good.
    const hero = {
      defaults: {sylvanas: 'windrunner'},
      hidden: true,
    };
    ```

  - [3.3](#3.3) <a name="3.3"></a> Use readable synonyms in place of reserved words.

    ```javascript
    // Bad.
    const sylvanas = {class: 'ranger',};

    // Bad.
    const sylvanas = {klass: 'ranger',};

    // Good.
    const sylvanas = {type: 'ranger',};
    ```

  <a name="es6-computed-properties"></a>
  - [3.4](#3.4) <a name="3.4"></a> Use computed property names when creating objects with dynamic property names.

    > Why? They allow you to define all the properties of an object in one place.

    ```javascript
    function getKey(k) {
      return `a key named ${k}`;
    }

    // Bad.
    
    const obj = {
      id: 5,
      name: 'Dun Morogh',
    };
    obj[getKey('enabled')] = true;

    // Good.
    
    const obj = {
      id: 5,
      name: 'Dun Morogh',
      [getKey('enabled')]: true,
    };
    ```

  <a name="es6-object-shorthand"></a>
  - [3.5](#3.5) <a name="3.5"></a> Use object method shorthand.

    ```javascript
    // Bad.
    const atom = {
      value: 1,
      addValue: function(value) {
        return atom.value + value;
      },
    };

    // Good.
    const atom = {
      value: 1,
      addValue(value) {
        return atom.value + value;
      },
    };
    ```

  <a name="es6-object-concise"></a>
  - [3.6](#3.6) <a name="3.6"></a> Use property value shorthand.

    > Why? It is shorter to write and descriptive.

    ```javascript
    const thrall = 'Thrall';

    // Bad.
    const obj = {thrall: thrall,};

    // Good.
    const obj = {thrall};
    ```

  - [3.7](#3.7) <a name="3.7"></a> Group your shorthand properties at the beginning of your object declaration.

    > Why? It's easier to tell which properties are using the shorthand.

    ```javascript
    const thrall = 'Thrall';
    const malfurion = 'Malfurion';

    // Bad.
    const obj = {
      region: 'Kalimdor',
      twoHeroesWalkIntoDarnassus: 2,
      illidan,
      artefact: 'Gul\'dan Crane',
      theDemonInside: true,
      stormrage,
    };

    // Good.
    const obj = {
      illidan,
      stormrage,
      region: 'Kalimdor',
      twoHeroesWalkIntoDarnassus: 2,
      artefact: 'Gul\'dan Crane',
      theDemonInside: true,
    };
    ```
    
  - [3.8](#3.8) <a name="3.8"></a> It's possible to inline and single-property object, an object shorthand, or an object destructuring.
  
    > Why? Under some cases, it's easier to read.
   
    ```javascript
    // Bad.
    const obj = {region: 'Kalimdor', twoHeroesWalkIntoDarnassus: 2, artefact: 'Gul\'dan Crane',};
    
    // Good.
    const obj = {illidan, stormrage, medhiv};
    
    // Also good.
    const obj = {
      illidan,
      stormrage,
      medhiv,
    };
    
    // Good.
    const {illidan, stormrage, medhiv,} = obj;
    
    // Good.
    const obj = {aShaman: 'Thrall',};
    ```

**[Back to top](#table-of-contents)**

## Arrays

  - [4.1](#4.1) <a name="4.1"></a> Use the literal syntax for array creation.

    ```javascript
    // Bad.
    const items = new Array();

    // Good.
    const items = [];
    ```

  - [4.2](#4.2) <a name="4.2"></a> Use Array#push instead of direct assignment to add items to an array.

    ```javascript
    const someStack = [];

    // Bad.
    someStack[someStack.length] = 'abracadabra';

    // Good.
    someStack.push('abracadabra');
    ```

  <a name="es6-array-spreads"></a>
  - [4.3](#4.3) <a name="4.3"></a> Use array spreads `...` to copy arrays.

    ```javascript
    // Bad.
    
    const len = items.length;
    const itemsCopy = [];
    
    let i;

    for (i = 0; i < len; i++) {
      itemsCopy[i] = items[i];
    }

    // Good.
    const itemsCopy = [...items];
    ```
    
  - [4.4](#4.4) <a name="4.4"></a> To convert an array-like object to an array, use Array#from.

    ```javascript
    const foo = document.querySelectorAll('.foo');
    const nodes = Array.from(foo);
    ```
    
  - [4.5](#4.5) <a name="4.5"></a> It's possible to inline an array when needed.
    
    > Why? Under some cases, it's easier to read.
   
    ```javascript
    // Good.
    const arr = ['Nefarian', 'Vaelastraz', 'Broodlord',];
    
    // Also good.
    const arr = [
      'Nefarian',
      'Vaelastraz',
      'Broodlord',
    ];
    ```

**[Back to top](#table-of-contents)**

## Destructuring

  - [5.1](#5.1) <a name="5.1"></a> Use object destructuring when accessing and using multiple properties of an object.

    > Why? Destructuring saves you from creating temporary references for those properties.

    ```javascript
    // Bad.
    function getFullName(user) {
      const firstName = user.firstName;
      const lastName = user.lastName;

      return `${firstName} ${lastName}`;
    }

    // Good.
    function getFullName(obj) {
      const {firstName, lastName} = obj;
      
      return `${firstName} ${lastName}`;
    }

    // Best.
    function getFullName({firstName, lastName}) {
      return `${firstName} ${lastName}`;
    }
    ```

  - [5.2](#5.2) <a name="5.2"></a> Use array destructuring.

    ```javascript
    const arr = [1, 2, 3, 4];

    // Bad.
    
    const first = arr[0];
    const second = arr[1];

    // Good.
    const [first, second] = arr;
    ```

  - [5.3](#5.3) <a name="5.3"></a> Use object destructuring for multiple return values, not array destructuring.

    > Why? You can add new properties over time or change the order of things without breaking call sites.

    ```javascript
    // Bad.
    
    function processInput(input) {
    
      // Then a miracle occurs.
      return [left, right, top, bottom];
    }

    // The caller needs to think about the order of return data.
    const [left, __, top] = processInput(input);

    // Good.
    
    function processInput(input) {
    
      // Then a miracle occurs.
      return {left, right, top, bottom};
    }

    // The caller selects only the data they need.
    const {left, right} = processInput(input);
    ```

**[Back to top](#table-of-contents)**

## Strings

  - [6.1](#6.1) <a name="6.1"></a> Use single quotes `''` for strings.

    ```javascript
    // Bad.
    const name = "Uther the Lightbringer";

    // Good.
    const name = 'Uther the Lightbringer';
    ```

  - [6.2](#6.2) <a name="6.2"></a> Strings longer than 100 characters should be written across multiple lines using string concatenation.
  - [6.3](#6.3) <a name="6.3"></a> Note: if overused, long strings with concatenation could impact performance.

    ```javascript
    // Bad.
    const errorMessage = 'Jaina Proudmoore is the founder and former Lady of Theramore Isle (as well as its only leader during its brief existence), the Alliance\'s major port in southern Kalimdor.';

    // Bad.
    const errorMessage = 'Jaina Proudmoore is the founder and former\
      Lady of Theramore Isle (as well as its only\
      leader during its brief existence), the Alliance\'s\
      major port in southern Kalimdor.';

    // Good.
    const errorMessage = 'Jaina Proudmoore is the founder and former ' +
      'Lady of Theramore Isle (as well as its only ' +
      'leader during its brief existence), the Alliance\'s ' +
      'major port in southern Kalimdor.';
    ```

  <a name="es6-template-literals"></a>
  - [6.4](#6.4) <a name="6.4"></a> When programmatically building up strings, use template strings instead of concatenation.

    > Why? Template strings give you a readable, concise syntax with proper newlines and string interpolation features.

    ```javascript
    // Bad.
    function sayHi(name) {
      return 'How are you, ' + name + '?';
    }

    // Bad.
    function sayHi(name) {
      return ['How are you, ', name, '?'].join();
    }

    // Good.
    function sayHi(name) {
      return `How are you, ${name}?`;
    }
    ```
    
  - [6.5](#6.5) <a name="6.5"></a> Never use `eval()` on a string, it opens too many vulnerabilities.

**[Back to top](#table-of-contents)**

## Functions

  - [7.1](#7.1) <a name="7.1"></a> Use function declarations instead of function expressions.

    > Why? Function declarations are named, so they're easier to identify in call stacks. Also, the whole body of a function declaration is hoisted, whereas only the reference of a function expression is hoisted. This rule makes it possible to always use [Arrow Functions](#arrow-functions) in place of function expressions.

    ```javascript
    // Bad.
    const foo = function() {
    };

    // Good.
    function foo() {
    }
    ```

  - [7.2](#7.2) <a name="7.2"></a> Function expressions:

    ```javascript
    // Immediately-invoked function expression (IIFE).
    (() => {
      console.log('Welcome to the Internet. Please follow me.');
    })();
    ```

  - [7.3](#7.3) <a name="7.3"></a> Never declare a function in a non-function block (if, while, etc). Assign the function to a variable instead. Browsers will allow you to do it, but they all interpret it differently, which is bad news bears.
  - [7.4](#7.4) <a name="7.4"></a> **Note**: ECMA-262 defines a `block` as a list of statements. A function declaration is not a statement. [Read ECMA-262's note on this issue](http://www.ecma-international.org/publications/files/ECMA-ST/Ecma-262.pdf#page=97).

    ```javascript
    // Bad.
    
    if (currentUser) {
      function test() {
        console.log('Nope.');
      }
    }

    // Good.
    
    let test;
    
    if (currentUser) {
      test = () => {
        console.log('Yup.');
      };
    }
    ```

  - [7.5](#7.5) <a name="7.5"></a> Never name a parameter `arguments`. This will take precedence over the `arguments` object that is given to every function scope.

    ```javascript
    // Bad.
    function nope(name, options, arguments) {
    
      // ...stuff...
    }

    // Good.
    function yup(name, options, args) {
      
      // ...stuff...
    }
    ```

  <a name="es6-rest"></a>
  - [7.6](#7.6) <a name="7.6"></a> Never use `arguments`, opt to use rest syntax `...` instead.

    > Why? `...` is explicit about which arguments you want pulled. Plus rest arguments are a real Array and not Array-like like `arguments`.

    ```javascript
    // Bad.
    function concatenateAll() {
      const args = Array.prototype.slice.call(arguments);
      
      return args.join('');
    }

    // Good.
    function concatenateAll(...args) {
      return args.join('');
    }
    ```

  <a name="es6-default-parameters"></a>
  - [7.7](#7.7) <a name="7.7"></a> Use default parameter syntax rather than mutating function arguments.

    ```javascript
    // Really bad.
    function handleThings(opts) {
    
      // No! We shouldn't mutate function arguments.
      // Double bad: if opts is falsy it'll be set to an object which may
      // be what you want but it can introduce subtle bugs.
      opts = opts || {};
      
      // ...
    }

    // Still bad.
    function handleThings(opts) {
      if (void 0 === opts) {
        opts = {};
      }
      
      // ...
    }

    // Good.
    function handleThings(opts = {}) {
      
      // ...
    }
    ```

  - [7.8](#7.8) <a name="7.8"></a> Avoid side effects with default parameters.

    > Why? They are confusing to reason about.
  
    ```javascript
    var b = 1;
    
    // Bad.
    function count(a = b++) {
      console.log(a);
    }
    
    count();  // 1
    count();  // 2
    count(3); // 3
    count();  // 3
    ```

  - [7.9](#7.9) <a name="7.9"></a> Always put default parameters last.

    ```javascript
    // Bad.
    function handleThings(opts = {}, name) {
      
      // ...
    }

    // Good.
    function handleThings(name, opts = {}) {
      
      // ...
    }
    ```

  - [7.10](#7.10) <a name="7.10"></a> Never use the Function constructor to create a new function.

    > Why? Creating a function in this way evaluates a string similarly to eval(), which opens vulnerabilities.

    ```javascript
    // Bad.
    var add = new Function('a', 'b', 'return a + b');

    // Still bad.
    var subtract = Function('a', 'b', 'return a - b');
    ```

**[Back to top](#table-of-contents)**

## Arrow Functions

  - [8.1](#8.1) <a name="8.1"></a> When you must use function expressions (as when passing an anonymous function), use arrow function notation.

    > Why? It creates a version of the function that executes in the context of `this`, which is usually what you want, and is a more concise syntax.

    > Why not? If you have a fairly complicated function, you might move that logic out into its own function declaration.

    ```javascript
    // Bad.
    [1, 2, 3].map(function(x) {
      const y = x + 1;
      
      return x * y;
    });

    // Good.
    [1, 2, 3].map((x) => {
      const y = x + 1;
      
      return x * y;
    });
    ```

  - [8.2](#8.2) <a name="8.2"></a> If the function body consists of a single expression, feel free to omit the braces and use the implicit return. Otherwise use a `return` statement.

    > Why? Syntactic sugar. It reads well when multiple functions are chained together.

    > Why not? If you plan on returning an object.

    ```javascript
    // Good.
    [1, 2, 3].map(number => `A string containing the ${number}.`);

    // Bad.
    [1, 2, 3].map(number => {
      const nextNumber = number + 1;
      
      `A string containing the ${nextNumber}.`;
    });

    // Good.
    [1, 2, 3].map(number => {
      const nextNumber = number + 1;
      
      return `A string containing the ${nextNumber}.`;
    });
    ```

  - [8.3](#8.3) <a name="8.3"></a> In case the expression spans over multiple lines, wrap it in parentheses for better readability.

    > Why? It shows clearly where the function starts and ends.

    ```javascript
    // Bad.
    [1, 2, 3].map(number => 'As time went by, the string containing the ' +
      `${number} became much longer. So we needed to break it over multiple ` +
      'lines.'
    );

    // Good.
    [1, 2, 3].map(number => (
      `As time went by, the string containing the ${number} became much ` +
      'longer. So we needed to break it over multiple lines.'
    ));
    ```

  - [8.4](#8.4) <a name="8.4"></a> If your function only takes a single argument, feel free to omit the parentheses.

    > Why? Less visual clutter.

    ```javascript
    // Good.
    [1, 2, 3].map(x => x * x);

    // Good.
    [1, 2, 3].reduce((y, x) => x + y);
    ```

**[Back to top](#table-of-contents)**

## Constructors

  - [9.1](#9.1) <a name="9.1"></a> Always use `class`. Avoid manipulating `prototype` directly.

    > Why? `class` syntax is more concise and easier to reason about.

    ```javascript
    // Bad.
    
    function Queue(contents = []) {
      this._queue = [...contents];
    }
    
    Queue.prototype.pop = function() {
      const value = this._queue[0];
      
      this._queue.splice(0, 1);
      
      return value;
    }

    // Good.
    class Queue {
      constructor(contents = []) {
        this._queue = [...contents];
      }
      
      pop() {
        const value = this._queue[0];
        
        this._queue.splice(0, 1);
        
        return value;
      }
    }
    ```

  - [9.2](#9.2) <a name="9.2"></a> Use `extends` for inheritance.

    > Why? It is a built-in way to inherit prototype functionality without breaking `instanceof`.

    ```javascript
    // Bad.
    
    const inherits = require('inherits');
    
    function PeekableQueue(contents) {
      Queue.apply(this, contents);
    }
    
    inherits(PeekableQueue, Queue);
    
    PeekableQueue.prototype.peek = function() {
      return this._queue[0];
    }

    // Good.
    class PeekableQueue extends Queue {
      peek() {
        return this._queue[0];
      }
    }
    ```

  - [9.3](#9.3) <a name="9.3"></a> Methods can return `this` to help with method chaining.

    ```javascript
    // Bad.
    
    Troll.prototype.jump = function() {
      this.jumping = true;
      
      return true;
    };

    Troll.prototype.setHeight = function(height) {
      this.height = height;
    };

    const volJin = new Throll();
    
    volJin.jump(); // true
    volJin.setHeight(20); // undefined

    // Good.
    
    class Throll {
      jump() {
        this.jumping = true;
        
        return this;
      }

      setHeight(height) {
        this.height = height;
        
        return this;
      }
    }

    const volJin = new Throll();

    volJin.jump()
      .setHeight(20);
    ```

  - [9.4](#9.4) <a name="9.4"></a> It's okay to write a custom toString() method, just make sure it works successfully and causes no side effects.

    ```javascript
    class Throll {
      constructor(options = {}) {
        this.name = options.name || 'no name';
      }

      getName() {
        return this.name;
      }

      toString() {
        return `Throll - ${this.getName()}`;
      }
    }
    ```

**[Back to top](#table-of-contents)**

## Modules

  - [10.1](#10.1) <a name="10.1"></a> Always use modules (`import`/`export`) over a non-standard module system.

    > Why? Modules are the future, let's start using the future now.

    ```javascript
    // Bad.
    
    const MapleInsideStyleGuide = require('./MapleInsideStyleGuide');
    
    module.exports = MapleInsideStyleGuide.es6;

    // Ok.
    
    import MapleInsideStyleGuide from './MapleInsideStyleGuide';
    
    export default MapleInsideStyleGuide.es6;

    // Best.
    
    import {es6} from './MapleInsideStyleGuide';
    
    export default es6;
    ```

  - [10.2](#10.2) <a name="10.2"></a> Do not use wildcard imports.

    > Why? This makes sure you have a single default export.

    ```javascript
    // Bad.
    import * as MapleInsideStyleGuide from './MapleInsideStyleGuide';

    // Good.
    import MapleInsideStyleGuide from './MapleInsideStyleGuide';
    ```

  - [10.3](#10.3) <a name="10.3"></a> And do not export directly from an import.

    > Why? Although the one-liner is concise, having one clear way to import and one clear way to export makes things consistent.

    ```javascript
    // Bad.
    
    // Filename es6.js.
    export {es6 as default} from './MapleInsideStyleGuide';

    // Good.
    
    // Filename es6.js.
    import {es6} from './MapleInsideStyleGuide';
    
    export default es6;
    ```

**[Back to top](#table-of-contents)**

## Iterators and Generators

  - [11.1](#11.1) <a name="11.1"></a> Don't use iterators. Prefer JavaScript's higher-order functions like `map()` and `reduce()` instead of loops like `for-of`.

    > Why? This enforces our immutable rule. Dealing with pure functions that return values is easier to reason about than side-effects.

    ```javascript
    const numbers = [1, 2, 3, 4, 5];

    // Bad.
    
    let sum = 0;
    
    for (let num of numbers) {
      sum += num;
    }

    15 === sum;

    // Good.
    
    let sum = 0;
    
    numbers.forEach((num) => sum += num);
    15 === sum;

    // Best (use the functional force).
    
    const sum = numbers.reduce((total, num) => total + num, 0);
    
    15 === sum;
    ```

  - [11.2](#11.2) <a name="11.2"></a> Don't use generators for now.

    > Why? They don't transpile well to ES5.

**[Back to top](#table-of-contents)**

## Properties

  - [12.1](#12.1) <a name="12.1"></a> Use dot notation when accessing properties.

    ```javascript
    const volJin = {
      troll: true,
      age: 78,
    };

    // Bad.
    const isTroll = volJin['troll'];

    // Good.
    const isTroll = volJin.troll;
    ```

  - [12.2](#12.2) <a name="12.2"></a> Use subscript notation `[]` when accessing properties with a variable.

    ```javascript
    const volJin = {
      troll: true,
      age: 78,
    };

    function getProp(prop) {
      return volJin[prop];
    }

    const isTroll = getProp('troll');
    ```

**[Back to top](#table-of-contents)**

## Variables

  - [13.1](#13.1) <a name="13.1"></a> Always use `const` to declare variables. Not doing so will result in global variables. We want to avoid polluting the global namespace.

    ```javascript
    // Bad.
    frostbolt = new Frostbolt();

    // Good.
    const frostbolt = new Frostbolt();
    ```

  - [13.2](#13.2) <a name="13.2"></a> Use one `const` declaration per variable.

    > Why? It's easier to add new variable declarations this way, and you never have to worry about swapping out a `;` for a `,` or introducing punctuation-only diffs.

    ```javascript
    // Bad.
    const items = getItems(),
      lordaeronArmy = true,
      footmans = 4500;

    // Bad.
    
    // Compare to above, and try to spot the mistake.
    const items = getItems(),
      lordaeronArmy = true;
      footmans = 4500;

    // Good.
    
    const items = getItems();
    const lordaeronArmy = true;
    const footmans = 4500
    ```

  - [13.3](#13.3) <a name="13.3"></a> Group all your `const`s and then group all your `let`s.

    > Why? This is helpful when later on you might need to assign a variable depending on one of the previous assigned variables.

    ```javascript
    // Bad.
    let i, len, lordaeron,
      items = getItems(),
      lordaeronArmy = true;

    // Bad.
    
    let i;
    
    const items = getItems();
    
    let lordaeron;
    
    const lordaeronArmy = true;
    
    let len;

    // Good.
    
    const lordaeronArmy = true;
    const items = getItems();
    
    let lordaeron;
    let i;
    let length;
    ```

  - [13.4](#13.4) <a name="13.4"></a> Assign variables where you need them, but place them in a reasonable place.

    > Why? `let` and `const` are block scoped and not function scoped.

    ```javascript
    // Good.
    function() {
      test();
      console.log('doing stuff..');

      // ...other stuff...

      const name = getName();

      if (name === 'test') {
        return false;
      }

      return name;
    }

    // Bad — unnecessary function call.
    function(hasName) {
      const name = getName();

      if (!hasName) {
        return false;
      }

      this.setFirstName(name);

      return true;
    }

    // Good.
    function(hasName) {
      if (!hasName) {
        return false;
      }

      const name = getName();
      
      this.setFirstName(name);

      return true;
    }
    ```

**[Back to top](#table-of-contents)**

## Hoisting

  - [14.1](#14.1) <a name="14.1"></a> `var` declarations get hoisted to the top of their scope, their assignment does not. `const` and `let` declarations are blessed with a new concept called [Temporal Dead Zones (TDZ)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let#Temporal_dead_zone_and_errors_with_let). It's important to know why [typeof is no longer safe](http://es-discourse.com/t/why-typeof-is-no-longer-safe/15).

    ```javascript
    // We know this wouldn't work (assuming there
    // is no notDefined global variable).
    function example() {
      console.log(notDefined); // Throws a ReferenceError.
    }

    // Creating a variable declaration after you
    // reference the variable will work due to
    // variable hoisting. Note: the assignment
    // value of `true` is not hoisted.
    function example() {
      console.log(declaredButNotAssigned); // undefined
      
      var declaredButNotAssigned = true;
    }

    // The interpreter is hoisting the variable
    // declaration to the top of the scope,
    // which means our example could be rewritten as:
    function example() {
      let declaredButNotAssigned;
      
      console.log(declaredButNotAssigned); // undefined
      declaredButNotAssigned = true;
    }

    // Using const and let.
    function example() {
      console.log(declaredButNotAssigned); // Throws a ReferenceError.
      console.log(typeof declaredButNotAssigned); // Throws a ReferenceError.
      
      const declaredButNotAssigned = true;
    }
    ```

  - [14.2](#14.2) <a name="14.2"></a> Anonymous function expressions hoist their variable name, but not the function assignment.

    ```javascript
    function example() {
      console.log(anonymous); // undefined

      anonymous(); // TypeError anonymous is not a function.

      var anonymous = function() {
        console.log('anonymous function expression');
      };
    }
    ```

  - [14.3](#14.3) <a name="14.3"></a> Named function expressions hoist the variable name, not the function name or the function body.

    ```javascript
    function example() {
      console.log(named); // undefined

      named(); // TypeError named is not a function.
      frostbolt(); // ReferenceError superPower is not defined.

      var named = function frostbolt() {
        console.log('Pew pew!');
      };
    }

    // The same is true when the function name
    // is the same as the variable name.
    function example() {
      console.log(named); // undefined

      named(); // TypeError named is not a function.

      var named = function named() {
        console.log('named');
      }
    }
    ```

  - [14.4](#14.4) <a name="14.4"></a> Function declarations hoist their name and the function body.

    ```javascript
    function example() {
      frostbolt(); // Flying

      function frostbolt() {
        console.log('Pew pew!');
      }
    }
    ```

  - For more information refer to [JavaScript Scoping & Hoisting](http://www.adequatelygood.com/2010/2/JavaScript-Scoping-and-Hoisting/) by [Ben Cherry](http://www.adequatelygood.com/).

**[Back to top](#table-of-contents)**

## Comparison Operators & Equality

  - [15.1](#15.1) <a name="15.1"></a> Use `===` and `!==` over `==` and `!=`.
  - [15.2](#15.2) <a name="15.2"></a> Conditional statements such as the `if` statement evaluate their expression using coercion with the `ToBoolean` abstract method and always follow these simple rules:

    + **Objects** evaluate to **true**;
    + **Undefined** evaluates to **false**;
    + **Null** evaluates to **false**;
    + **Booleans** evaluate to **the value of the boolean**;
    + **Numbers** evaluate to **false** if **+0, -0, or NaN**, otherwise **true**;
    + **Strings** evaluate to **false** if an empty string `''`, otherwise **true**.

    ```javascript
    if ([0]) {
    
      // true
      // An array is an object, objects evaluate to true.
    }
    ```

  - [15.3](#15.3) <a name="15.3"></a> Use shortcuts.

    ```javascript
    // Bad.
    if (name !== '') {
    
      // ...stuff...
    }

    // Good.
    if (name) {
    
      // ...stuff...
    }

    // Bad.
    if (collection.length > 0) {
    
      // ...stuff...
    }

    // Good.
    if (collection.length) {
    
      // ...stuff...
    }
    ```
    
  - [15.4](#15.4) <a name="15.4"></a> Do not use Yoda conditions.
  
    > Why? Historically, Yoda conditions were used to prevent programmer to assign a value by mistake, because you cannot assign to a literal value. But today, tooling has made us better programmers because tools will catch the mistaken use of `=` instead of `==`. The utility of the pattern doesn't outweigh the readability hit the code takes while using Yoda conditions.
  
    ```javascript
    // Bad.
    if ('Illidan' !== name) {
    
      // ...stuff...
    }
  
    // Good.
    if (name !== 'Illidan') {
    
      // ...stuff...
    }
  
    // Bad.
    if (5 < collection.length) {
    
      // ...stuff...
    }
  
    // Good.
    if (collection.length > 5) {
    
      // ...stuff...
    }
    ```

  - [15.5](#15.5) <a name="15.5"></a> For more information see [Truth Equality and JavaScript](http://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/#more-2108) by Angus Croll.

**[Back to top](#table-of-contents)**

## Blocks

  - [16.1](#16.1) <a name="16.1"></a> Use braces with all multi-line blocks.

    ```javascript
    // Bad.
    if (test)
      return false;

    // Good.
    if (test) return false;

    // Good.
    if (test) {
      return false;
    }

    // Bad.
    function() { return false; }

    // Good.
    function() {
      return false;
    }
    ```

  - [16.2](#16.2) <a name="16.2"></a> If you're using multi-line blocks with `if` and `else`, put `else` on the same line as your
    `if` block's closing brace.

    ```javascript
    // Bad.
    if (test) {
      thing1();
      thing2();
    }
    else {
      thing3();
    }

    // Good.
    if (test) {
      thing1();
      thing2();
    } else {
      thing3();
    }
    ```

**[Back to top](#table-of-contents)**

## Comments

  - [17.1](#17.1) <a name="17.1"></a> Use `/** ... */` for multi-line comments. Include a description, specify types and values for all parameters and return values.

    ```javascript
    // Bad.
    
    // make() returns a new element
    // based on the passed in tag name.
    //
    // @param {String} tag
    // @return {Element} element
    function make(tag) {
    
      // ...stuff...
    
      return element;
    }
    
    // Good.
    
    /**
     * make() returns a new element
     * based on the passed in tag name.
     *
     * @param {String} tag
     * @return {Element} element
     */
    function make(tag) {
    
      // ...stuff...
    
      return element;
    }
    ```

  - [17.2](#17.2) <a name="17.2"></a> Use `//` for single line comments. Place single line comments on a newline above the subject of the comment. Put an empty line before the comment.

    ```javascript
    // Bad.
    const active = true;  // Is current tab.

    // Good.
    
    // Is current tab.
    const active = true;

    // Bad.
    function getType() {
      console.log('fetching type...');
      // Set the default type to 'no type'.
      const type = this._type || 'no type';

      return type;
    }

    // Good.
    function getType() {
      console.log('fetching type...');

      // Set the default type to 'no type'.
      const type = this._type || 'no type';

      return type;
    }

    // Also good.
    function getType() {
    
      // Set the default type to 'no type'.
      const type = this._type || 'no type';

      return type;
    }
    ```

  - [17.3](#17.3) <a name="17.3"></a> Prefixing your comments with `FIXME` or `TODO` helps other developers quickly understand if you're pointing out a problem that needs to be revisited, or if you're suggesting a solution to the problem that needs to be implemented. These are different than regular comments because they are actionable. The actions are `FIXME: need to figure this out.` or `TODO: need to implement.`.

  - [17.4](#17.4) <a name="17.4"></a> Use `// FIXME:` to annotate problems.

    ```javascript
    class Thrall extends Durotan {
      constructor() {
        super();

        // FIXME: shouldn't use a global here.
        total = 0;
      }
    }
    ```

  - [17.5](#17.5) <a name="17.5"></a> Use `// TODO:` to annotate solutions to problems.

    ```javascript
    class Thrall extends Durotan {
      constructor() {
        super();

        // TODO: total should be configurable by an options param.
        this.total = 0;
      }
    }
    ```
    
  - [17.6](#17.6) <a name="17.6"></a> A comment is an English sentence, and it should follows the corresponding grammar rules.
  
    ```javascript
    // Bad.
    
    // i'm a comment but i don't follow grammar rules
    class Thrall extends Durotan {
      ...
    }
    
    // Good.
        
    // Following grammar rules is my only way of life.
    class Thrall extends Durotan {
      ...
    }
    ```

**[Back to top](#table-of-contents)**

## Whitespace

  - [18.1](#18.1) <a name="18.1"></a> Use soft tabs set to 2 spaces.

    ```javascript
    // Bad.
    function() {
    ∙∙∙∙const name;
    }

    // Bad.
    function() {
    ∙const name;
    }

    // Good.
    function() {
    ∙∙const name;
    }
    ```

  - [18.2](#18.2) <a name="18.2"></a> Place 1 space before the leading brace.

    ```javascript
    // Bad.
    function test(){
      console.log('test');
    }

    // Good.
    function test() {
      console.log('test');
    }

    // Bad.
    arthas.set('attr',{
      race: 'undead',
      weapon: 'Frostmourne',
    });

    // Good.
    arthas.set('attr', {
      race: 'undead',
      weapon: 'Frostmourne',
    });
    ```

  - [18.3](#18.3) <a name="18.3"></a> Place 1 space before the opening parenthesis in control statements (`if`, `while`, etc.). Place no space before the argument list in function calls and declarations.

    ```javascript
    // Bad.
    if(isTroll) {
      fight ();
    }

    // Good.
    if (isTroll) {
      fight();
    }

    // Bad.
    function fight () {
      console.log ('Pew pew!');
    }

    // Good.
    function fight() {
      console.log('Pew pew!');
    }
    ```

  - [18.4](#18.4) <a name="18.4"></a> Set off operators with spaces.

    ```javascript
    // Bad.
    const x=y+5;

    // Good.
    const x = y + 5;
    ```

  - [18.5](#18.5) <a name="18.5"></a> End files with a single newline character.

    ```javascript
    // Bad.
    (function(global) {
    
      // ...stuff...
    })(this);
    ```

    ```javascript
    // Bad.
    (function(global) {
    
      // ...stuff...
    })(this);↵
    ↵
    ```

    ```javascript
    // Good.
    (function(global) {
    
      // ...stuff...
    })(this);↵
    ```

  - [18.6](#18.6) <a name="18.6"></a> Use indentation when making long method chains. Use a leading dot, which
    emphasizes that the line is a method call, not a new statement.

    ```javascript
    // Bad.
    document.getElementById('item').method1().getElement1().method2().getElement2().method3();

    // Bad.
    document.
      getElementById('item').
        doSomething1().
      getElement1().
        doSomething2().
        doSomething3().
      getElement2().
        doSomething4();

    // Good.
    document
      .getElementById('item')
        .doSomething1()
      .getElement1()
        .doSomething2()
        .doSomething3()
      .getElement2()
        .doSomething4();

    // Bad.
    const item = document.getElementById('item').doSomething1().getElement1()
      .doSomething2()
      .doSomething3()
      .getElement2();

    // Good.
    const item = document
      .getElementById('item')
        .doSomething1()
      .getElement1()
        .doSomething2()
        .doSomething3()
      .getElement2();
    ```

  - [18.7](#18.7) <a name="18.7"></a> Leave a blank line after blocks and before the next statement. But don't do it on a object or an array.

    ```javascript
    // Bad.
    
    if (foo) {
      return bar;
    }
    return baz;

    // Good.
    
    if (foo) {
      return bar;
    }

    return baz;

    // Bad.
    
    const obj = {
      foo() {
      },
      bar() {
      }
    };
    return obj;

    // Bad.
    
    const obj = {
      foo() {
      },
      
      bar() {
      }
    };

    return obj;
    
    // Good.
    
    const obj = {
      foo() {
      },
      bar() {
      }
    };

    return obj;

    // Bad.
    
    const arr = [
      function foo() {
      },
      function bar() {
      }
    ];
    return arr;

    // Good.
    
    const arr = [
      function foo() {
      },
      function bar() {
      }
    ];

    return arr;
    ```

  - [18.8](#18.8) <a name="18.8"></a> Do not pad your blocks with blank lines.

    ```javascript
    // Bad.
    function bar() {

      console.log(foo);

    }

    // Also bad.
    if (baz) {

      console.log(qux);
    } else {
      console.log(foo);

    }

    // Good.
    function bar() {
      console.log(foo);
    }

    // Good.
    if (baz) {
      console.log(qux);
    } else {
      console.log(foo);
    }
    ```

  - [18.9](#18.9) <a name="18.9"></a> Leave a blank line before `return`. But don't do it when it begins a block.
  
    > Why? To improve readability.
    
    ```javascript
    // Bad.
    function polishFrostmourne() {
      const frostmourne = new Frostmourne();
      return frostmourne.polish();
    }
    
    // Good.
    function polishFrostmourne() {
      const frostmourne = new Frostmourne();
      
      return frostmourne.polish();
    }
    
    // Bad.
    function polishFrostmourne() {
      const frostmourne = new Frostmourne();
      
      if (!frostmourne.isSharp()) {
      
        return frostmourne.polish();
      } else {
      
        return false;
      }
    }
    
    // Good.
    function polishFrostmourne() {
      const frostmourne = new Frostmourne();
      
      if (!frostmourne.isSharp()) {
        return frostmourne.polish();
      } else {
        return false;
      }
    }
    ```

**[Back to top](#table-of-contents)**

## Commas

  - [19.1](#19.1) <a name="19.1"></a> Leading commas: **Nope**.

    ```javascript
    // Bad.
    const story = [
        once
      , upon
      , aTime
    ];

    // Good.
    const story = [
      once,
      upon,
      aTime,
    ];

    // Bad.
    const hero = {
        firstName: 'Arthas'
      , lastName: 'Menethil'
      , birthYear: 785
      , skill: 'Death Coil'
    };

    // Good.
    const hero = {
      firstName: 'Arthas',
      lastName: 'Menethil',
      birthYear: 785,
      superPower: 'Death Coil',
    };
    ```

  - [19.2](#19.2) <a name="19.2"></a> Additional trailing comma: **Yup**.

    > Why? This leads to cleaner git diffs. Also, transpilers like Babel will remove the additional trailing comma in the transpiled code which means you don't have to worry about the [trailing comma problem](es5/README.md#commas) in legacy browsers.

    ```javascript
    // Bad: git diff without trailing comma.
    const hero = {
         firstName: 'Illidan',
    -    lastName: 'Stormrag'
    +    lastName: 'Stormrage',
    +    skills: ['Immolation', 'Mana Burn']
    };

    // Good: git diff with trailing comma.
    const hero = {
         firstName: 'Illidan',
         lastName: 'Stormrage',
    +    skills: ['Immolation', 'Mana Burn'],
    };

    // Bad.
    
    const hero = {
      firstName: 'Kael\'Thas',
      lastName: 'Sunstrider'
    };

    const heroes = [
      'Kael\'Thas',
      'Illidan'
    ];

    // Good.
    
    const hero = {
      firstName: 'Kael\'Thas',
      lastName: 'Sunstrider',
    };

    const heroes = [
      'Kael\'Thas',
      'Illidan',
    ];
    ```

**[Back to top](#table-of-contents)**

## Semicolons

  - [20.1](#20.1) <a name="20.1"></a> **Yup**.

    ```javascript
    // Bad.
    (function() {
      const name = 'Thrall'
      
      return name
    })()

    // Good.
    (() => {
      const name = 'Thrall';
      
      return name;
    })();

    // Good (guards against the function becoming an argument when two files with IIFEs are concatenated).
    ;(() => {
      const name = 'Thrall';
      
      return name;
    })();
    ```

    [Read more](http://stackoverflow.com/questions/7365172/semicolon-before-self-invoking-function/7365214%237365214).

**[Back to top](#table-of-contents)**

## Type Casting & Coercion

  - [21.1](#21.1) <a name="21.1"></a> Perform type coercion at the beginning of the statement.
  - [21.2](#21.2) <a name="21.2"></a> Strings:

    ```javascript
    // this.reviewScore = 9;

    // Bad.
    const totalScore = this.reviewScore + '';

    // Good.
    const totalScore = String(this.reviewScore);
    ```

  - [21.3](#21.3) <a name="21.3"></a> Numbers: use `Number` for type casting and `parseInt` always with a radix for parsing strings.

    ```javascript
    const inputValue = '4';

    // Bad.
    const val = new Number(inputValue);

    // Bad.
    const val = +inputValue;

    // Bad.
    const val = inputValue >> 0;

    // Bad.
    const val = parseInt(inputValue);

    // Good.
    const val = Number(inputValue);

    // Good.
    const val = parseInt(inputValue, 10);
    ```

  - [21.4](#21.4) <a name="21.4"></a> If for whatever reason you are doing something wild and `parseInt` is your bottleneck and need to use Bitshift for [performance reasons](http://jsperf.com/coercion-vs-casting/3), leave a comment explaining why and what you're doing.

    ```javascript
    // Good.
    
    // parseInt was the reason my code was slow.
    // Bitshifting the String to coerce it to a
    // Number made it a lot faster.
    const val = inputValue >> 0;
    ```

  - [21.5](#21.5) <a name="21.5"></a> **Note**: be careful when using bitshift operations. Numbers are represented as [64-bit values](http://es5.github.io/#x4.3.19), but Bitshift operations always return a 32-bit integer ([source](http://es5.github.io/#x11.7)). Bitshift can lead to unexpected behavior for integer values larger than 32 bits. [Discussion](https://github.com/airbnb/javascript/issues/109). Largest signed 32-bit Int is 2,147,483,647:

    ```javascript
    2147483647 >> 0 // 2147483647
    2147483648 >> 0 // -2147483648
    2147483649 >> 0 // -2147483647
    ```

  - [21.6](#21.6) <a name="21.6"></a> Booleans:

    ```javascript
    const age = 0;

    // Bad.
    const hasAge = new Boolean(age);

    // Good.
    const hasAge = Boolean(age);

    // Good.
    const hasAge = !!age;
    ```

**[Back to top](#table-of-contents)**

## Naming Conventions

  - [22.1](#22.1) <a name="22.1"></a> Avoid single letter names. Be descriptive with your naming.

    ```javascript
    // Bad.
    function q() {
    
      // ...stuff...
    }

    // Good.
    function query() {
    
      // ...stuff...
    }
    ```

  - [22.2](#22.2) <a name="22.2"></a> Use camelCase when naming objects, functions, and instances.

    ```javascript
    // Bad.
    
    const OBJEcttsssss = {};
    const this_is_my_object = {};
    
    function c() {}

    // Good.
    
    const thisIsMyObject = {};
    
    function thisIsMyFunction() {}
    ```

  - [22.3](#22.3) <a name="22.3"></a> Use PascalCase when naming constructors or classes.

    ```javascript
    // Bad.
    
    function user(options) {
      this.name = options.name;
    }

    const bad = new user({
      name: 'nope',
    });

    // Good.
    
    class User {
      constructor(options) {
        this.name = options.name;
      }
    }

    const good = new User({
      name: 'yup',
    });
    ```

  - [22.4](#22.4) <a name="22.4"></a> Use a leading underscore `_` when naming private properties.

    ```javascript
    // Bad.
    
    this.__firstName__ = 'Pandaren';
    this.firstName_ = 'Pandaren';

    // Good.
    this._firstName = 'Pandaren';
    ```

  - [22.5](#22.5) <a name="22.5"></a> Don't save references to `this`. Use arrow functions or Function#bind.

    ```javascript
    // Bad.
    function foo() {
      const self = this;
      
      return function() {
        console.log(self);
      };
    }

    // Bad.
    function foo() {
      const that = this;
      
      return function() {
        console.log(that);
      };
    }

    // Good.
    function foo() {
      return () => {
        console.log(this);
      };
    }
    ```

  - [22.6](#22.6) <a name="22.6"></a> If your file exports a single class, your filename should be exactly the name of the class.
    ```javascript
    // File contents.
    
    class CheckBox {
    
      // ...
    }
    
    export default CheckBox;

    // In some other file.
    
    // Bad.
    import CheckBox from './checkBox';

    // Bad.
    import CheckBox from './check_box';

    // Good.
    import CheckBox from './CheckBox';
    ```

  - [22.7](#22.7) <a name="22.7"></a> Use camelCase when you export-default a function. Your filename should be identical to your function's name.

    ```javascript
    function makeStyleGuide() {
    }

    export default makeStyleGuide;
    ```

  - [22.8](#22.8) <a name="22.8"></a> Use PascalCase when you export a singleton/function library/bare object.

    ```javascript
    const MapleInsideStyleGuide = {
      es6: {
      }
    };

    export default MapleInsideStyleGuide;
    ```

**[Back to top](#table-of-contents)**

## Accessors

  - [23.1](#23.1) <a name="23.1"></a> Accessor functions for properties are not required.
  - [23.2](#23.2) <a name="23.2"></a> If you do make accessor functions use getVal() and setVal('hello').

    ```javascript
    // Bad.
    dragon.age();

    // Good.
    dragon.getAge();

    // Bad.
    dragon.age(25);

    // Good.
    dragon.setAge(25);
    ```

  - [23.3](#23.3) <a name="23.3"></a> If the property is a `boolean`, use `isVal()` or `hasVal()`.

    ```javascript
    // Bad.
    if (!dragon.age()) {
      return false;
    }

    // Good.
    if (!dragon.hasAge()) {
      return false;
    }
    ```

  - [23.4](#23.4) <a name="23.4"></a> It's okay to create get() and set() functions, but be consistent.

    ```javascript
    class NightElf {
      constructor(options = {}) {
        const arrows = options.arrows || 30;
        
        this.set('arrows', arrows);
      }

      set(key, val) {
        this[key] = val;
      }

      get(key) {
        return this[key];
      }
    }
    ```

**[Back to top](#table-of-contents)**

## Events

  - [24.1](#24.1) <a name="24.1"></a> When attaching data payloads to events (whether DOM events or something more proprietary), pass a hash instead of a raw value. This allows a subsequent contributor to add more data to the event payload without finding and updating every handler for the event.

**[Back to top](#table-of-contents)**

## ECMAScript 5 Compatibility

  - [25.1](#25.1) <a name="25.1"></a> Refer to [Kangax](https://twitter.com/kangax/)'s ES5 [compatibility table](http://kangax.github.io/es5-compat-table/).

**[Back to top](#table-of-contents)**

## ECMAScript 6 Styles

  - [26.1](#26.1) <a name="26.1"></a> This is a collection of links to the various ES6 features.

1. [Arrow Functions](#arrow-functions)
1. [Classes](#constructors)
1. [Object Shorthand](#es6-object-shorthand)
1. [Object Concise](#es6-object-concise)
1. [Object Computed Properties](#es6-computed-properties)
1. [Template Strings](#es6-template-literals)
1. [Destructuring](#destructuring)
1. [Default Parameters](#es6-default-parameters)
1. [Rest](#es6-rest)
1. [Array Spreads](#es6-array-spreads)
1. [Let and Const](#references)
1. [Iterators and Generators](#iterators-and-generators)
1. [Modules](#modules)

**[Back to top](#table-of-contents)**

## Testing

  - [27.1](#27.1) <a name="27.1"></a> **Yup**.

    ```javascript
    function() {
      return true;
    }
    ```

  - [27.2](#27.2) <a name="27.2"></a> **No, but seriously**:
   - Whichever testing framework you use, you should be writing tests!
   - Strive to write many small pure functions, and minimize where mutations occur;
   - Be cautious about stubs and mocks, they can make your tests more brittle;
   - 100% test coverage is a good goal to strive for, even if it's not always practical to reach it;
   - Whenever you fix a bug, _write a regression test_. A bug fixed without a regression test is almost certainly going to break again in the future.

**[Back to top](#table-of-contents)**

## License

(The MIT License)

Copyright (c) 2015 Maple Inside

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
'Software'), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

**[Back to top](#table-of-contents)**

## Amendments

We encourage you to fork this guide and change the rules to fit your team's style guide.
