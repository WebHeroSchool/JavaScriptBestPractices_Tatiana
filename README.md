# JavaScriptBestPractices_Tatiana
  1. Use === Instead of ==  
   JavaScript utilizes two different kinds of equality operators: === | !== and == | != It is considered best practice to always use the former set when comparing.

   "If two operands are of the same type and value, then === produces true and !== produces false." - JavaScript: The Good Parts

   However, when working with == and !=, you'll run into issues when working with different types. In these cases, they'll try to coerce the values, unsuccessfully.

  2. Eval = Bad  
   Executing JavaScript from a string is an enormous security risk. It is far too easy for a bad actor to run arbitrary code when you use eval(). The eval() function evaluates JavaScript code represented as a string. 
   
  3. Don't Use Short-Hand
Technically, you can get away with omitting most curly braces and semi-colons. Most browsers will correctly interpret the following:
```
   if(someVariableExists)
   x = false
```
However, consider this:
```
if(someVariableExists)
   x = false
   anotherFunctionCall();
```
One might think that the code above would be equivalent to:

```
   if(someVariableExists) {
   x = false;
   anotherFunctionCall();
   }
```
Unfortunately, he'd be wrong. In reality, it means:
```
   if(someVariableExists) {
   x = false;
   }
   anotherFunctionCall();
```
As you'll notice, the indentation mimics the functionality of the curly brace. Needless to say, this is a terrible practice that should be avoided at all costs. The only time that curly braces should be omitted is with one-liners, and even this is a highly debated topic.
```
if(2 + 2 === 4) return 'nicely done';
```

`Always Consider the Future`
What if, at a later date, you need to add more commands to this if statement. In order to do so, you would need to rewrite this block of code. Bottom line - tread with caution when omitting.

  4. Place Scripts at the Bottom of Your Page
Remember -- the primary goal is to make the page load as quickly as possible for the user. When loading a script, the browser can't continue on until the entire file has been loaded. Thus, the user will have to wait longer before noticing any progress.

If you have JS files whose only purpose is to add functionality -- for example, after a button is clicked -- go ahead and place those files at the bottom, just before the closing body tag. This is absolutely a best practice.

Better:
```
<p>And now you know my favorite kinds of corn. </p>
<script type="text/javascript" src="path/to/file.js"></script>
<script type="text/javascript" src="path/to/anotherFile.js"></script>
</body>
</html>
```

  5. Declare Variables Outside of the For Statement
When executing lengthy "for" statements, don't make the engine work any harder than it must. For example:

Bad:
```
for(let i = 0; i < someArray.length; i++) {
   let container = document.getElementById('container');
   container.innerHtml += 'my number: ' + i;
   console.log(i);
}
```
Notice how we must determine the length of the array for each iteration, and how we traverse the dom to find the "container" element each time -- highly inefficient!

Better:
```
let container = document.getElementById('container');
for(let i = 0, len = someArray.length; i < len;  i++) {
   container.innerHtml += 'my number: ' + i;
   console.log(i);
}
```

  7. The Fastest Way to Build a String
Don't always reach for your handy-dandy "for" statement when you need to loop through an array or object. Be creative and find the quickest solution for the job at hand.
```
let arr = ['item 1', 'item 2', 'item 3', ...];
let list = '<ul><li>' + arr.join('</li><li>') + '</li></ul>';
```

> Using native methods (like join()), regardless of what’s going on behind the abstraction layer, is usually much faster than any non-native alternative.
- James Padolsey, james.padolsey.com

  8. Reduce Globals
"By reducing your global footprint to a single name, you significantly reduce the chance of bad interactions with other applications, widgets, or libraries."
- Douglas Crockford

1
2
3
4
5
6
var name = 'Jeffrey';
var lastName = 'Way';
 
function doSomething() {...}
 
console.log(name); // Jeffrey -- or window.name
Better
1
2
3
4
5
6
var DudeNameSpace = {
   name : 'Jeffrey',
   lastName : 'Way',
   doSomething : function() {...}
}
console.log(DudeNameSpace.name); // Jeffrey
Notice how we've "reduced our footprint" to just the ridiculously named "DudeNameSpace" object.
9. Comment Your Code
It might seem unnecessary at first, but trust me, you WANT to comment your code as best as possible. What happens when you return to the project months later, only to find that you can't easily remember what your line of thinking was. Or, what if one of your colleagues needs to revise your code? Always, always comment important sections of your code.

1
2
3
4
// Cycle through array and echo out each name. 
for(var i = 0, len = array.length; i < len; i++) {
   console.log(array[i]);
}
10. Don't Pass a String to "SetInterval" or "SetTimeOut"
Consider the following code:

1
2
3
setInterval(
"document.getElementById('container').innerHTML += 'My new number: ' + i", 3000
);
Not only is this code inefficient, but it also functions in the same way as the "eval" function would. Never pass a string to SetInterval and SetTimeOut. Instead, pass a function name.

1
setInterval(someFunction, 3000);
11. Use {} Instead of New Object()
There are multiple ways to create objects in JavaScript. Perhaps the more traditional method is to use the "new" constructor, like so:

1
2
3
4
5
6
var o = new Object();
o.name = 'Jeffrey';
o.lastName = 'Way';
o.someFunction = function() {
   console.log(this.name);
}
However, this method receives the "bad practice" stamp without actually being so. Instead, I recommend that you use the much more robust object literal method.

Better
1
2
3
4
5
6
7
var o = {
   name: 'Jeffrey',
   lastName = 'Way',
   someFunction : function() {
      console.log(this.name);
   }
};
Note that if you simply want to create an empty object, {} will do the trick.

1
var o = {};
"Objects literals enable us to write code that supports lots of features yet still make it a relatively straightforward for the implementers of our code. No need to invoke constructors directly or maintain the correct order of arguments passed to functions, etc." - dyn-web.com

17. Always, Always Use Semicolons
Technically, most browsers will allow you to get away with omitting semi-colons.

1
2
3
4
var someItem = 'some string'
function doSomething() {
  return 'something'
}
Having said that, this is a very bad practice that can potentially lead to much bigger, and harder to find, issues.

Better
1
2
3
4
var someItem = 'some string';
function doSomething() {
  return 'something';
}
