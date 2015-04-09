# JavaScript Style Guide() {

*A mostly reasonable approach to JavaScript*


## <a name='TOC'>Table of Contents</a>

  1. [Tabs vs Spaces](#tabs)
  1. [General Code Layout](#code-layout)
  1. [`if else` blocks](#if-else-blocks)
  1. [Types](#types)
  1. [Objects](#objects)
  1. [Arrays](#arrays)
  1. [Strings](#strings)
  1. [Functions](#functions)
  1. [Properties](#properties)
  1. [Variables](#variables)
  1. [Hoisting](#hoisting)
  1. [Conditional Expressions & Equality](#conditionals)
  1. [Blocks](#blocks)
  1. [Comments](#comments)
  1. [Whitespace](#whitespace)
  1. [Leading Commas](#leading-commas)
  1. [Semicolons](#semicolons)
  1. [Type Casting & Coercion](#type-coercion)
  1. [Naming Conventions](#naming-conventions)
  1. [Accessors](#accessors)
  1. [Constructors](#constructors)
  1. [Modules](#modules)
  1. [jQuery](#jquery)
  1. [ES5 Compatibility](#es5)
  1. [Testing](#testing)
  1. [Performance](#performance)
  1. [Resources](#resources)
  1. [In the Wild](#in-the-wild)
  1. [Translation](#translation)
  1. [The JavaScript Style Guide Guide](#guide-guide)
  1. [Contributors](#contributors)
  1. [License](#license)

## <a name='tabs'>Tabs vs Spaces</a>

  - **Set the editor to use tabs instead of spaces.** Not everyone likes their indentation the same width. Some people like it at 2 spaces, some like it at 4, others like it at you get the point. Using tabs allows for uniform code formatting while allowing the person editing the code to set the width to whatever they prefer instead of what the author of the code prefers. Everyone is happy this way!

    **[[Back to top]](#TOC)**

## <a name='code-layout'>General Code Layout</a>

  - Use newlines to group logically related pieces of code. **Always** have a newline before and after new function declarations!
    ```javascript
    // bad
    function andThen(x) {
      console.log(x);
    }
    for (var x = 0; x < list.length; x++) {
      doSomethingTo(x);
      doSomethingElseTo(x);
      andThen(x);
    }
    for (var y = 0; y < otherList.length; y++) {
      nowDoSomethingWith(y);
    }

    // good
    /**
    * Outputs x to the log.
    * @param {Number} x The number to output to the console
    * @return {Number} The incremented value of x
    */
    function andThen(x) {
      console.log(x);
      return ++x;
    }
    
    // Do lots of wonderful things to x.
    for (var x = 0; x < list.length; x++) {
      doSomethingTo(x);
      doSomethingElseTo(x);
      andThen(x);
    }

    // Even better, I explain what this for loop does!
    for (var y = 0; y < otherList.length; y++) {
      nowDoSomethingWith(y);
    }
    ```

    **[[Back to top]](#TOC)**

## <a name='if-else-blocks'>`if else` blocks</a>

  - Use the "stroustrup" style ([See ESLint Docs](http://eslint.org/docs/rules/brace-style.html)) of `if else` blocks. The reason is it makes the keywords `else` and `else if` stand out more. It also allows for added a comment before the statement explaining what that block does.
    ```javascript
    // bad
    function foo()
    {
      return true;
    }

    if (foo)
    {
      bar();
    }

    try
    {
      somethingRisky();
    } catch(e)
    {
      handleError();
    }

    if (foo) {
      bar();
    } else {
      baz();
    }
    
    // good
    function foo() {
      return true;
    }

    if (foo) {
      bar();
    }

    // even better!
    // Show the bar text on the screen.
    if (foo) {
      bar();
    }
    // Don't show any text to the user but cancel the timeout.
    // Could be anything here but at least we know what this
    // else block does.
    else {
      baz();
    }

    try {
      somethingRisky();
    }
    catch(e) {
      handleError();
    }
    ```

    **[[Back to top]](#TOC)**

## <a name='types'>Types</a>

  - **Primitives**: When you access a primitive type you work directly on its value

    + `string`
    + `number`
    + `boolean`
    + `null`
    + `undefined`

    ```javascript
    var foo = 1,
        bar = foo;

    bar = 9;

    console.log(foo, bar); // => 1, 9
    ```
  - **Complex**: When you access a complex type you work on a reference to its value

    + `object`
    + `array`
    + `function`

    ```javascript
    var foo = [1, 2],
        bar = foo;

    bar[0] = 9;

    console.log(foo[0], bar[0]); // => 9, 9
    ```

    **[[Back to top]](#TOC)**

## <a name='objects'>Objects</a>

  - Use the literal syntax for object creation.

    ```javascript
    // bad
    var item = new Object();

    // good
    var item = {};
    ```

  - Don't use [reserved words](https://developer.mozilla.org/en-US/docs/JavaScript/Reference/Reserved_Words) as keys.

    ```javascript
    // bad
    var superman = {
      class: 'superhero',
      default: { clark: 'kent' },
      private: true
    };

    // good
    var superman = {
      klass: 'superhero',
      defaults: { clark: 'kent' },
      hidden: true
    };
    ```
    **[[Back to top]](#TOC)**

## <a name='arrays'>Arrays</a>

  - Use the literal syntax for array creation

    ```javascript
    // bad
    var items = new Array();

    // good
    var items = [];
    ```

  - If you don't know array length use Array#push.

    ```javascript
    var someStack = [];


    // bad
    someStack[someStack.length] = 'abracadabra';

    // good
    someStack.push('abracadabra');
    ```

  - When you need to copy an array use Array#slice. [jsPerf](http://jsperf.com/converting-arguments-to-an-array/7)

    ```javascript
    var len = items.length,
        itemsCopy = [],
        i;

    // bad
    for (i = 0; i < len; i++) {
      itemsCopy[i] = items[i];
    }

    // good
    itemsCopy = Array.prototype.slice.call(items);
    ```

    **[[Back to top]](#TOC)**


## <a name='strings'>Strings</a>

  - Use single quotes `''` for strings

    ```javascript
    // bad
    var name = "Bob Parr";

    // good
    var name = 'Bob Parr';

    // bad
    var fullName = "Bob " + this.lastName;

    // good
    var fullName = 'Bob ' + this.lastName;
    ```

  - Strings longer than 80 characters should be written across multiple lines using string concatenation.
  - Note: If overused, long strings with concatenation could impact performance. [jsPerf](http://jsperf.com/ya-string-concat) & [Discussion](https://github.com/airbnb/javascript/issues/40)

    ```javascript
    // bad
    var errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';

    // bad
    var errorMessage = 'This is a super long error that \
    was thrown because of Batman. \
    When you stop to think about \
    how Batman had anything to do \
    with this, you would get nowhere \
    fast.';


    // good
    var errorMessage = 'This is a super long error that ' +
      'was thrown because of Batman.' +
      'When you stop to think about ' +
      'how Batman had anything to do ' +
      'with this, you would get nowhere ' +
      'fast.';
    ```

  - **The prefered method is the "bad" method since it is more readable and clear as to what is happening.** This is an issue of readability vs performance. In areas where performance is critical, the "good" method should be used. Here is the performance reason which again, should only be used in a performance critical area of code: Use Array#join instead of string concatenation. Mostly for IE: [jsPerf](http://jsperf.com/string-vs-array-concat/2).

    ```javascript
    var items,
        messages,
        length, i;

    messages = [{
        state: 'success',
        message: 'This one worked.'
    },{
        state: 'success',
        message: 'This one worked as well.'
    },{
        state: 'error',
        message: 'This one did not work.'
    }];

    length = messages.length;

    // bad but preferred due to readability!
    function inbox(messages) {
      items = '<ul>';

      for (i = 0; i < length; i++) {
        items += '<li>' + messages[i].message + '</li>';
      }

      return items + '</ul>';
    }

    // good
    function inbox(messages) {
      items = [];

      for (i = 0; i < length; i++) {
        items[i] = messages[i].message;
      }

      return '<ul><li>' + items.join('</li><li>') + '</li></ul>';
    }
    ```

    **[[Back to top]](#TOC)**


## <a name='functions'>Functions</a>

  - Local functions should be declared using the `function` keyword and not assigned to a `var`. This is more clear and the function can still be referenced by name later.
    ```javascript
    // bad - named function expression
    var named = function named() {
      return true;
    };

    // good
    function named() {
      return true;
    }
    ```

  - Anonymous and named functions. While javascript allows for anonymous functions, it is prefered that functions be named. This gives more info when profiling or viewing a call stack:

    ```javascript
    var list = ['a', 'b', 'c'];
    
    // ok
    list.sort(function(a, b) {
      return a - b;
    });
    
    // better
    list.sort(function myListSorter(a, b) {
      return a - b;
    });
    
    // immediately-invoked function expression (IIFE)
    (function() {
      console.log('Welcome to the Internet. Please follow me.');
    })();
    ```

  - Immediately invoked functions
    ```javascript
    // immediately-invoked function expression (IIFE)
    (function welcomeToTheInternet() {
      console.log('Welcome to the Internet. Please follow me.');
    })();
    ```

  - **Never declare a function in a non-function block (if, while, etc).** Browsers will allow you to do it, but they all interpret it differently, which is bad news bears. **Note:** ECMA-262 defines a `block` as a list of statements. A function declaration is not a statement. [Read ECMA-262's note on this issue](http://www.ecma-international.org/publications/files/ECMA-ST/Ecma-262.pdf#page=97).

    ```javascript
    // bad
    for (var i = 0; i < list.length; i++) {
      function test() {
        console.log('Nope.');
      }
      
      test();
    }

    // good
    function test() {
      console.log('Nope.');
    }
    
    for (var i = 0; i < list.length; i++) {
      test();
    }
    ```

  - Never name a parameter `arguments`, this will take precedence over the `arguments` object that is given to every function scope.

    ```javascript
    // bad
    function nope(name, options, arguments) {
      // ...stuff...
    }

    // good
    function yup(name, options, args) {
      // ...stuff...
    }
    ```

    **[[Back to top]](#TOC)**



## <a name='properties'>Properties</a>

  - Use dot notation when accessing properties.

    ```javascript
    var luke = {
      jedi: true,
      age: 28
    };

    // bad
    var isJedi = luke['jedi'];

    // good
    var isJedi = luke.jedi;
    ```

  - Use subscript notation `[]` when accessing properties with a variable.

    ```javascript
    var luke = {
      jedi: true,
      age: 28
    };

    function getProp(prop) {
      return luke[prop];
    }

    var isJedi = getProp('jedi');
    ```

    **[[Back to top]](#TOC)**


## <a name='variables'>Variables</a>

  - Always use `var` to declare variables. Not doing so will result in global variables. We want to avoid polluting the global namespace. Captain Planet warned us of that.

    ```javascript
    // bad
    superPower = new SuperPower();

    // good
    var superPower = new SuperPower();
    ```

  - Use a `var` declaration for multiple variables and declare each variable on a newline. The reason for this is because of the use of tabs for indenting ([See Tabs vs Spaces](#tabs)). With the tab size potentially set to anything, it is very hard to line up the variables below the initial `var` declaration.

    ```javascript
    // bad
    var items = getItems(),
        goSportsTeam = true,
        dragonball = 'z';
    
    // good
    var items = getItems();
    var goSportsTeam = true;
    var dragonball = 'z';
    ```

  - Declare unassigned variables last. This is helpful when later on you might need to assign a variable depending on one of the previous assigned variables.

    ```javascript
    // bad
    var i, len, dragonball,
    var items = getItems(),
    var goSportsTeam = true;

    // bad
    var i, items = getItems(),
    var dragonball,
    var goSportsTeam = true,
    var len;

    // good
    var items = getItems(),
    var goSportsTeam = true,
    var dragonball,
    var length,
    var i;
    ```

  - Do not use variables outside of their scope even thought JavaScript will allow it. If you need to use the variable outside of the scope, declare it outside the scope.

    ```javascript
    // bad
    for (var i = 0; i < list.length; i++) {
      // ..do something..
    }
    
    console.log(i);

    // good
    var i;
    for (i = 0; i < list.length; i++) {
      // ..do something..
    }
    
    console.log(i);
    ```

    **[[Back to top]](#TOC)**


## <a name='hoisting'>Hoisting</a>

  - Variable declarations get hoisted to the top of their scope, their assignment does not.

    ```javascript
    // we know this wouldn't work (assuming there
    // is no notDefined global variable)
    function example() {
      console.log(notDefined); // => throws a ReferenceError
    }

    // creating a variable declaration after you
    // reference the variable will work due to
    // variable hoisting. Note: the assignment
    // value of `true` is not hoisted.
    function example() {
      console.log(declaredButNotAssigned); // => undefined
      var declaredButNotAssigned = true;
    }

    // The interpreter is hoisting the variable
    // declaration to the top of the scope.
    // Which means our example could be rewritten as:
    function example() {
      var declaredButNotAssigned;
      console.log(declaredButNotAssigned); // => undefined
      declaredButNotAssigned = true;
    }
    ```

  - Anonymous function expressions hoist their variable name, but not the function assignment.

    ```javascript
    function example() {
      console.log(anonymous); // => undefined

      anonymous(); // => TypeError anonymous is not a function

      var anonymous = function() {
        console.log('anonymous function expression');
      };
    }
    ```

  - Named function expressions hoist the variable name, not the function name or the function body.

    ```javascript
    function example() {
      console.log(named); // => undefined

      named(); // => TypeError named is not a function

      superPower(); // => ReferenceError superPower is not defined

      var named = function superPower() {
        console.log('Flying');
      };


      // the same is true when the function name
      // is the same as the variable name.
      function example() {
        console.log(named); // => undefined

        named(); // => TypeError named is not a function

        var named = function named() {
          console.log('named');
        };
      }
    }
    ```

  - Function declarations hoist their name and the function body.

    ```javascript
    function example() {
      superPower(); // => Flying

      function superPower() {
        console.log('Flying');
      }
    }
    ```

  - For more information refer to [JavaScript Scoping & Hoisting](http://www.adequatelygood.com/2010/2/JavaScript-Scoping-and-Hoisting) by [Ben Cherry](http://www.adequatelygood.com/)

    **[[Back to top]](#TOC)**



## <a name='conditionals'>Conditional Expressions & Equality</a>

  - Use `===` and `!==` over `==` and `!=`.
  - Conditional expressions are evaluated using coercion with the `ToBoolean` method and always follow these simple rules:

    + **Objects** evaluate to **true**
    + **Undefined** evaluates to **false**
    + **Null** evaluates to **false**
    + **Booleans** evaluate to **the value of the boolean**
    + **Numbers** evalute to **false** if **+0, -0, or NaN**, otherwise **true**
    + **Strings** evaluate to **false** if an empty string `''`, otherwise **true**

    ```javascript
    if ([0]) {
      // true
      // An array is an object, objects evaluate to true
    }
    ```

  - Use shortcuts.

    ```javascript
    // bad
    if (name !== '') {
      // ...stuff...
    }

    // good
    if (name) {
      // ...stuff...
    }

    // bad
    if (collection.length > 0) {
      // ...stuff...
    }

    // good
    if (collection.length) {
      // ...stuff...
    }
    ```

  - For more information see [Truth Equality and JavaScript](http://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/#more-2108) by Angus Croll

    **[[Back to top]](#TOC)**


## <a name='blocks'>Blocks</a>

  - Use braces with all multi-line blocks.

    ```javascript
    // bad
    if (test)
      return false;

    // good
    if (test) return false;

    // good
    if (test) {
      return false;
    }

    // bad
    function() { return false; }

    // good
    function() {
      return false;
    }
    ```

    **[[Back to top]](#TOC)**


## <a name='comments'>Comments</a>

  - In general, comments are a great way to explain your code. I agree with writing self documenting code, but that is not enough on large projects and with multiple team members. Comments help explain the "what" and the "why" of your code.
    - "What" is happening in the code that follows the comment. Think of the comment before a block of code as a topic sentence of a paragraph. It helps divide ideas and give a general overview of what is happening next.
        ```javascript
        // Make the face very angry.
        openMouth();
        turnFaceRed();
        blowSmokeOutOfEars();
        
        // After being angry, make the face represent acceptance.
        closeMouth();
        smile();
        turnFaceNormalColor();
        ```

    - "Why" is this code here. This can be for a complicated bug fix that required a one line change. But that one line is **very** important. Instead of just putting in the one line fix and expecting people to understand why that line is there, explain it!
        ```javascript
        // We need to reset here because it resets this object
        // after some really crazy stuff happened in our app and
        // if we don't reset it here some other crazy stuff will happen.
        appState.reset();
        ```
  - Use `/** ... */` for `function` comments. Include a description, specify types and values for all parameters and return values.

    ```javascript
    // bad
    // make() returns a new element
    // based on the passed in tag name
    //
    // @param <String> tag
    // @return <Element> element
    function make(tag) {

      // ...stuff...

      return element;
    }

    // good
    /**
     * make() returns a new element
     * based on the passed in tag name
     *
     * @param <String> tag
     * @return <Element> element
     */
    function make(tag) {

      // ...stuff...

      return element;
    }
    ```

  - The exception to the above rule is when declaring a function as a callback.

    ```javascript
    // good
    // Sort the array in ascending order.
    myArray.sort(function myArraySort(a, b) {
      return b - a;
    });
    ```
   
  - Use `//` for non-function comments. Place single line comments on a newline above the subject of the comment. Put an emptyline before the comment and a space after the `//`.

    ```javascript
    // bad
    var active = true;  // is current tab

    // good
    // is current tab
    var active = true;

    // bad
    function getType() {
      console.log('fetching type...');
      // set the default type to 'no type'
      var type = this._type || 'no type';

      return type;
    }

    // good
    function getType() {
      console.log('fetching type...');

      // set the default type to 'no type'
      var type = this._type || 'no type';

      return type;
    }
    ```

    **[[Back to top]](#TOC)**


## <a name='whitespace'>Whitespace</a>

  - Place a space after **all** keywords. This helps differentiate them from user function calls.

    ```javascript
    // bad
    if(isSet) {
      doSomething();
    }
    
    // good
    if (isSet) {
      doSomething();
    }
    ```
  
  - Place a space after **all** operators.

    ```javascript
    // bad
    var fullName = 'First: '+firstName+' Last: '+lastName;
    
    // good
    var fullName = 'First: ' + firstName + ' Last: ' + lastName;
    ```
  
  - Do not place a space after a function name and it's parentheses.

    ```javascript
    // bad
    function foo (bar) {
      doSomething();
    }
    
    // good
    function foo(bar) {
      doSomething();
    }
    ```
  
  - Place 1 space before any leading brace.

    ```javascript
    // bad
    function test(){
      console.log('test');
    }
    
    if (isSet){
      test();
    }

    // good
    function test() {
      console.log('test');
    }
    
    if (isSet) {
	  test()
	}

    // bad
    dog.set('attr',{
      age: '1 year',
      breed: 'Bernese Mountain Dog'
    });

    // good
    dog.set('attr', {
      age: '1 year',
      breed: 'Bernese Mountain Dog'
    });
    ```
  - Place an empty newline at the end of the file.

    ```javascript
    // bad
    (function(global) {
      // ...stuff...
    })(this);
    ```

    ```javascript
    // good
    (function(global) {
      // ...stuff...
    })(this);

    ```

  - Use indentation when making long method chains.

    ```javascript
    // bad
    $('#items').find('.selected').highlight().end().find('.open').updateCount();

    // good
    $('#items')
      .find('.selected')
        .highlight()
        .end()
      .find('.open')
        .updateCount();

    // bad
    var leds = stage.selectAll('.led').data(data).enter().append("svg:svg").class('led', true)
        .attr('width',  (radius + margin) * 2).append("svg:g")
        .attr("transform", "translate(" + (radius + margin) + "," + (radius + margin) + ")")
        .call(tron.led);

    // good
    var leds = stage.selectAll('.led')
        .data(data)
      .enter().append("svg:svg")
        .class('led', true)
        .attr('width',  (radius + margin) * 2)
      .append("svg:g")
        .attr("transform", "translate(" + (radius + margin) + "," + (radius + margin) + ")")
        .call(tron.led);
    ```

    **[[Back to top]](#TOC)**

## <a name='leading-commas'>Leading Operators or Commas</a>

  - **Nope.**

    ```javascript
    // bad
    var isSet = (condition1 && condition2)
      || (condition3 && condition4);

    // good
    var isSet = (condition1 && condition2) ||
      (condition3 && condition4);

    // bad
    var longString = 'This is a very long string that I could not'
      + 'Fit on to one line because I just have so much to say!';
  
    // good
    var longString = 'This is a very long string that I could not' +
      'Fit on to one line because I just have so much to say!';

    // bad
    var hero = {
        firstName: 'Bob'
      , lastName: 'Parr'
      , heroName: 'Mr. Incredible'
      , superPower: 'strength'
    };

    // good
    var hero = {
      firstName: 'Bob',
      lastName: 'Parr',
      heroName: 'Mr. Incredible',
      superPower: 'strength'
    };
    ```

    **[[Back to top]](#TOC)**


## <a name='semicolons'>Semicolons</a>

  - **Yup.**

    ```javascript
    // bad
    (function() {
      var name = 'Skywalker'
      return name
    })()

    // good
    (function() {
      var name = 'Skywalker';
      return name;
    })();

    // good
    ;(function() {
      var name = 'Skywalker';
      return name;
    })();
    ```

    **[[Back to top]](#TOC)**


## <a name='type-coercion'>Type Casting & Coercion</a>

  - Perform type coercion at the beginning of the statement.
  - Strings:

    ```javascript
    //  => this.reviewScore = 9;

    // bad
    var totalScore = this.reviewScore + '';

    // good
    var totalScore = '' + this.reviewScore;

    // bad
    var totalScore = '' + this.reviewScore + ' total score';

    // good
    var totalScore = this.reviewScore + ' total score';
    ```

  - Use `parseInt` for Numbers and always with a radix for type casting.
  - If for whatever reason you are doing something wild and `parseInt` is your bottleneck and need to use Bitshift for [performance reasons](http://jsperf.com/coercion-vs-casting/3), leave a comment explaining why and what you're doing.

    ```javascript
    var inputValue = '4';

    // bad
    var val = new Number(inputValue);

    // bad
    var val = +inputValue;

    // bad
    var val = inputValue >> 0;

    // bad
    var val = parseInt(inputValue);

    // good
    var val = Number(inputValue);

    // good
    var val = parseInt(inputValue, 10);

    // good
    /**
     * parseInt was the reason my code was slow.
     * Bitshifting the String to coerce it to a
     * Number made it a lot faster.
     */
    var val = inputValue >> 0;
    ```

  - Booleans:

    ```javascript
    var age = 0;

    // bad
    var hasAge = new Boolean(age);

    // good
    var hasAge = Boolean(age);

    // good
    var hasAge = !!age;
    ```

    **[[Back to top]](#TOC)**


## <a name='naming-conventions'>Naming Conventions</a>

  - Avoid single letter names. Be descriptive with your naming.

    ```javascript
    // bad
    function q() {
      // ...stuff...
    }

    // good
    function query() {
      // ..stuff..
    }
    ```

  - Use camelCase when naming objects, functions, and instances

    ```javascript
    // bad
    var OBJEcttsssss = {};
    var this_is_my_object = {};
    var this-is-my-object = {};
    function c() {};
    var u = new user({
      name: 'Bob Parr'
    });

    // good
    var thisIsMyObject = {};
    function thisIsMyFunction() {};
    var user = new User({
      name: 'Bob Parr'
    });
    ```

  - Use PascalCase when naming constructors or classes

    ```javascript
    // bad
    function user(options) {
      this.name = options.name;
    }

    var bad = new user({
      name: 'nope'
    });

    // good
    function User(options) {
      this.name = options.name;
    }

    var good = new User({
      name: 'yup'
    });
    ```

  - Use a leading underscore `_` when naming private properties

    ```javascript
    // bad
    this.__firstName__ = 'Panda';
    this.firstName_ = 'Panda';

    // good
    this._firstName = 'Panda';
    ```

  - When saving a reference to `this` use `_this`.

    ```javascript
    // bad
    function() {
      var self = this;
      return function() {
        console.log(self);
      };
    }

    // bad
    function() {
      var that = this;
      return function() {
        console.log(that);
      };
    }

    // good
    function() {
      var _this = this;
      return function() {
        console.log(_this);
      };
    }
    ```

  - Name your functions. This is helpful for stack traces.

    ```javascript
    // bad
    var log = function(msg) {
      console.log(msg);
    };

    // good
    var log = function log(msg) {
      console.log(msg);
    };
    ```

    **[[Back to top]](#TOC)**


## <a name='accessors'>Accessors</a>

  - Accessor functions for properties are not required
  - If you do make accessor functions use getVal() and setVal('hello')

    ```javascript
    // bad
    dragon.age();

    // good
    dragon.getAge();

    // bad
    dragon.age(25);

    // good
    dragon.setAge(25);
    ```

  - If the property is a boolean, use isVal() or hasVal()

    ```javascript
    // bad
    if (!dragon.age()) {
      return false;
    }

    // good
    if (!dragon.hasAge()) {
      return false;
    }
    ```

  - It's okay to create get() and set() functions, but be consistent.

    ```javascript
    function Jedi(options) {
      options || (options = {});
      var lightsaber = options.lightsaber || 'blue';
      this.set('lightsaber', lightsaber);
    }

    Jedi.prototype.set = function(key, val) {
      this[key] = val;
    };

    Jedi.prototype.get = function(key) {
      return this[key];
    };
    ```

    **[[Back to top]](#TOC)**


## <a name='constructors'>Constructors</a>

  - Assign methods to the prototype object, instead of overwriting the prototype with a new object. Overwriting the prototype makes inheritance impossible: by resetting the prototype you'll overwrite the base!

    ```javascript
    function Jedi() {
      console.log('new jedi');
    }

    // bad
    Jedi.prototype = {
      fight: function fight() {
        console.log('fighting');
      },

      block: function block() {
        console.log('blocking');
      }
    };

    // good
    Jedi.prototype.fight = function fight() {
      console.log('fighting');
    };

    Jedi.prototype.block = function block() {
      console.log('blocking');
    };
    ```

  - Methods can return `this` to help with method chaining.

    ```javascript
    // bad
    Jedi.prototype.jump = function() {
      this.jumping = true;
      return true;
    };

    Jedi.prototype.setHeight = function(height) {
      this.height = height;
    };

    var luke = new Jedi();
    luke.jump(); // => true
    luke.setHeight(20) // => undefined

    // good
    Jedi.prototype.jump = function() {
      this.jumping = true;
      return this;
    };

    Jedi.prototype.setHeight = function(height) {
      this.height = height;
      return this;
    };

    var luke = new Jedi();

    luke.jump()
      .setHeight(20);
    ```


  - It's okay to write a custom toString() method, just make sure it works successfully and causes no side effects.

    ```javascript
    function Jedi(options) {
      options || (options = {});
      this.name = options.name || 'no name';
    }

    Jedi.prototype.getName = function getName() {
      return this.name;
    };

    Jedi.prototype.toString = function toString() {
      return 'Jedi - ' + this.getName();
    };
    ```

    **[[Back to top]](#TOC)**


## <a name='modules'>Modules</a>

  - The module should start with a `!`. This ensures that if a malformed module forgets to include a final semicolon there aren't errors in production when the scripts get concatenated.
  - The file should be named with camelCase, live in a folder with the same name, and match the name of the single export.
  - Add a method called noConflict() that sets the exported module to the previous version and returns this one.
  - Always declare `'use strict';` at the top of the module.

    ```javascript
    // fancyInput/fancyInput.js

    !function(global) {
      'use strict';

      var previousFancyInput = global.FancyInput;

      function FancyInput(options) {
        this.options = options || {};
      }

      FancyInput.noConflict = function noConflict() {
        global.FancyInput = previousFancyInput;
        return FancyInput;
      };

      global.FancyInput = FancyInput;
    }(this);
    ```

    **[[Back to top]](#TOC)**


## <a name='jquery'>jQuery</a>

  - Prefix jQuery object variables with a `$`.

    ```javascript
    // bad
    var sidebar = $('.sidebar');

    // good
    var $sidebar = $('.sidebar');
    ```

  - Cache jQuery lookups.

    ```javascript
    // bad
    function setSidebar() {
      $('.sidebar').hide();

      // ...stuff...

      $('.sidebar').css({
        'background-color': 'pink'
      });
    }

    // good
    function setSidebar() {
      var $sidebar = $('.sidebar');
      $sidebar.hide();

      // ...stuff...

      $sidebar.css({
        'background-color': 'pink'
      });
    }
    ```

  - For DOM queries use Cascading `$('.sidebar ul')` or parent > child `$('.sidebar > .ul')`. [jsPerf](http://jsperf.com/jquery-find-vs-context-sel/16)
  - Use `find` with scoped jQuery object queries.

    ```javascript
    // bad
    $('.sidebar', 'ul').hide();

    // bad
    $('.sidebar').find('ul').hide();

    // good
    $('.sidebar ul').hide();

    // good
    $('.sidebar > ul').hide();

    // good (slower)
    $sidebar.find('ul');

    // good (faster)
    $($sidebar[0]).find('ul');
    ```

    **[[Back to top]](#TOC)**


## <a name='es5'>ECMAScript 5 Compatibility</a>

  - Refer to [Kangax](https://twitter.com/kangax/)'s ES5 [compatibility table](http://kangax.github.com/es5-compat-table/)

  **[[Back to top]](#TOC)**


## <a name='testing'>Testing</a>

  - **Yup.**

    ```javascript
    function() {
      return true;
    }
    ```

    **[[Back to top]](#TOC)**


## <a name='performance'>Performance</a>

  - [On Layout & Web Performance](http://kellegous.com/j/2013/01/26/layout-performance/)
  - [String vs Array Concat](http://jsperf.com/string-vs-array-concat/2)
  - [Try/Catch Cost In a Loop](http://jsperf.com/try-catch-in-loop-cost)
  - [Bang Function](http://jsperf.com/bang-function)
  - [jQuery Find vs Context, Selector](http://jsperf.com/jquery-find-vs-context-sel/13)
  - [innerHTML vs textContent for script text](http://jsperf.com/innerhtml-vs-textcontent-for-script-text)
  - [Long String Concatenation](http://jsperf.com/ya-string-concat)
  - Loading...

  **[[Back to top]](#TOC)**


## <a name='resources'>Resources</a>


**Read This**

  - [Annotated ECMAScript 5.1](http://es5.github.com/)

**Other Styleguides**

  - [Google JavaScript Style Guide](http://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml)
  - [jQuery Core Style Guidelines](http://docs.jquery.com/JQuery_Core_Style_Guidelines)
  - [Principles of Writing Consistent, Idiomatic JavaScript](https://github.com/rwldrn/idiomatic.js/)

**Other Styles**

  - [Naming this in nested functions](https://gist.github.com/4135065) - Christian Johansen
  - [Conditional Callbacks](https://github.com/airbnb/javascript/issues/52)

**Books**

  - [JavaScript: The Good Parts](http://www.amazon.com/JavaScript-Good-Parts-Douglas-Crockford/dp/0596517742) - Douglas Crockford
  - [JavaScript Patterns](http://www.amazon.com/JavaScript-Patterns-Stoyan-Stefanov/dp/0596806752) - Stoyan Stefanov
  - [Pro JavaScript Design Patterns](http://www.amazon.com/JavaScript-Design-Patterns-Recipes-Problem-Solution/dp/159059908X)  - Ross Harmes and Dustin Diaz
  - [High Performance Web Sites: Essential Knowledge for Front-End Engineers](http://www.amazon.com/High-Performance-Web-Sites-Essential/dp/0596529309) - Steve Souders
  - [Maintainable JavaScript](http://www.amazon.com/Maintainable-JavaScript-Nicholas-C-Zakas/dp/1449327680) - Nicholas C. Zakas
  - [JavaScript Web Applications](http://www.amazon.com/JavaScript-Web-Applications-Alex-MacCaw/dp/144930351X) - Alex MacCaw
  - [Pro JavaScript Techniques](http://www.amazon.com/Pro-JavaScript-Techniques-John-Resig/dp/1590597273) - John Resig
  - [Smashing Node.js: JavaScript Everywhere](http://www.amazon.com/Smashing-Node-js-JavaScript-Everywhere-Magazine/dp/1119962595) - Guillermo Rauch

**Blogs**

  - [DailyJS](http://dailyjs.com/)
  - [JavaScript Weekly](http://javascriptweekly.com/)
  - [JavaScript, JavaScript...](http://javascriptweblog.wordpress.com/)
  - [Bocoup Weblog](http://weblog.bocoup.com/)
  - [Adequately Good](http://www.adequatelygood.com/)
  - [NCZOnline](http://www.nczonline.net/)
  - [Perfection Kills](http://perfectionkills.com/)
  - [Ben Alman](http://benalman.com/)
  - [Dmitry Baranovskiy](http://dmitry.baranovskiy.com/)
  - [Dustin Diaz](http://dustindiaz.com/)
  - [nettuts](http://net.tutsplus.com/?s=javascript)

  **[[Back to top]](#TOC)**

## <a name='in-the-wild'>In the Wild</a>

  This is a list of organizations that are using this style guide. Send us a pull request or open an issue and we'll add you to the list.

  - **Airbnb**: [airbnb/javascript](https://github.com/airbnb/javascript)
  - **American Insitutes for Research**: [AIRAST/javascript](https://github.com/AIRAST/javascript)
  - **ExactTarget**: [ExactTarget/javascript](https://github.com/ExactTarget/javascript)
  - **GoodData**: [gooddata/gdc-js-style](https://github.com/gooddata/gdc-js-style)
  - **How About We**: [howaboutwe/javascript](https://github.com/howaboutwe/javascript)
  - **MinnPost**: [MinnPost/javascript](https://github.com/MinnPost/javascript)
  - **National Geographic**: [natgeo/javascript](https://github.com/natgeo/javascript)
  - **Razorfish**: [razorfish/javascript-style-guide](https://github.com/razorfish/javascript-style-guide)
  - **Shutterfly**: [shutterfly/javascript](https://github.com/shutterfly/javascript)

## <a name='translation'>Translation</a>

  This style guide is also available in other languages:


## <a name='guide-guide'>The Original JavaScript Style Guide Guide</a>

  - [Reference](https://github.com/airbnb/javascript/wiki/The-JavaScript-Style-Guide-Guide)

## <a name='authors'>Contributors</a>

  - [View Contributors](https://github.com/airbnb/javascript/graphs/contributors)


## <a name='license'>License</a>

(The MIT License)

Copyright (c) 2012 Airbnb

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

**[[Back to top]](#TOC)**

# };

