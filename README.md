# JavaScriptBestPractices_Tatiana
##  1. Read, Read, Read...
There really isn't a substitute for a book when grabbing some lunch, or just before you go to bed. Always keep a web development book on your bedside table. Here are some good JavaScript's books:

[Object-Oriented JavaScript](https://www.packtpub.com/product/object-oriented-javascript/9781847194145)

  [JavaScript: The Good Parts](https://www.oreilly.com/library/view/javascript-the-good/9780596517748/)
  
  [Learning jQuery 1.3](https://www.packtpub.com/product/learning-jquery-1-3/9781847196705)
  
  [Learning JavaScript](https://www.oreilly.com/library/view/learning-javascript/0596527462/)
  
    (*nothing good will come of it without reading professional literature)

##  2. Always, Always Use Semicolons
Technically, most browsers will allow you to get away with omitting semi-colons.
```
const someItem = 'some string'
function doSomething() {
  return 'something'
}
````
**This is a very bad practice that can potentially lead to much bigger, and harder to find, issues.**

Better:
```
const someItem = 'some string';
function doSomething() {
  return 'something';
};
```
  (*there are cases in JavaScript where a line break is not interpreted as a semicolon, which can lead to errors)  
   
##  3. Don't Use Short-Hand
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
  What if, at a later date, you need to add more commands to this `if` statement. In order to do so, you would need to rewrite this block of code. Bottom line - tread with caution when omitting.

##  4. Place Scripts at the Bottom of Your Page
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

##  5. Declare Variables Outside of the For Statement
  
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

##  6. The Fastest Way to Build a String
  
  Don't always reach for your handy-dandy "for" statement when you need to loop through an array or object. Be creative and find the quickest solution for the job at hand.
```
let arr = ['item 1', 'item 2', 'item 3', ...];
let list = '<ul><li>' + arr.join('</li><li>') + '</li></ul>';
```

> Using native methods (like join()), regardless of what’s going on behind the abstraction layer, is usually much faster than any non-native alternative.
> - James Padolsey, james.padolsey.com

##  7. Reduce Globals

> "By reducing your global footprint to a single name, you significantly reduce the chance of bad interactions with other applications, widgets, or libraries."
> - Douglas Crockford
```
let name = 'Jeffrey';
let lastName = 'Way';
function doSomething() {...}
console.log(name); // Jeffrey -- or window.name
```

Better:
```
let DudeNameSpace = {
   name : 'Jeffrey',
   lastName : 'Way',
   doSomething : function() {...}
}
console.log(DudeNameSpace.name); // Jeffrey
```

##  8. Comment Your Code

  It might seem unnecessary at first,but you WANT to comment your code as best as possible. What happens when you return to the project months later, only to find that you can't easily remember what your line of thinking was. Or, what if one of your colleagues needs to revise your code? Always, always comment important sections of your code.
```
// Cycle through array and echo out each name. 
for(let i = 0, len = array.length; i < len; i++) {
   console.log(array[i]);
};
```

##  9. Don't Pass a String to "SetInterval" or "SetTimeOut"

  Consider the following code:

```
setInterval(
"document.getElementById('container').innerHTML += 'My new number: ' + i", 3000
);
```
Not only is this code inefficient, but it also functions in the same way as the "eval" function would. Never pass a string to SetInterval and SetTimeOut. Instead, pass a function name.

```
setInterval(someFunction, 3000);
```

##  10. Use {} Instead of New Object()

  There are multiple ways to create objects in JavaScript. Perhaps the more traditional method is to use the "new" constructor, like so:

```
const o = new Object();
o.name = 'Jeffrey';
o.lastName = 'Way';
o.someFunction = function() {
   console.log(this.name);
};
```

However, this method receives the "bad practice" stamp without actually being so. Instead, you use the much more robust object literal method.

Better:
```
const o = {
   name: 'Jeffrey',
   lastName = 'Way',
   someFunction : function() {
      console.log(this.name);
   }
};
```
Note that if you simply want to create an empty object, {} will do the trick.

```
let o = {};
``` 
> "Objects literals enable us to write code that supports lots of features yet still make it a relatively straightforward for the implementers of our code. No need to invoke
>  constructors directly or maintain the correct order of arguments passed to functions, etc." - dyn-web.com

##  11. Utilize JS Lint
 
  JSLint is a debugger written by Douglas Crockford. Simply paste in your script, and it'll quickly scan for any noticeable issues and errors in your code.
    The great thing about them is that style checking can also find programming errors, such as typos in variable or function names.
  
  >"JSLint takes a JavaScript source and scans it. If it finds a problem, it returns a message describing the problem and an approximate location within the source. The problem  is not necessarily a syntax error, although it often is. JSLint looks at some style conventions as well as structural problems. It does not prove that your program is correct. It just provides another set of eyes to help spot problems."
  > - JSLint Documentation
    
    Before signing off on a script, run it through JSLint just to be sure that you haven't made any mindless mistakes.
