JavaScript, the winning style
=============================

_NOTE_: This 'research' was originally published as a blog article at http://seravo.fi/2013/javascript-the-winning-style and then transformed into a public Git repo on suggestion by [Eric Elliott](http://ericleads.com/).


If your code is easy to read, there will be fewer bugs, any remaining bugs will be easier to debug and new coders will have a lower barrier to participate in your project. Everybody agrees that investing a bit of time to following an agreed-upon coding style is worth all the benefits it yields. Unlike Python and some other languages, JavaScript does not have an authoritative style guide. Instead there are several popular ones:

* [Google JavaScript Style Guide](http://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml)
* [npm’s coding style](https://docs.npmjs.com/misc/coding-style)
* [Felix’s Node.js Style Guide](https://github.com/felixge/node-style-guide)
* [Idiomatic JavaScript](https://github.com/rwldrn/idiomatic.js/)
* [jQuery JavaScript Style Guide](http://contribute.jquery.org/style-guide/js/)
* [Douglas Crockford’s JavaScript coding style](http://javascript.crockford.com/code.html)

Then of course, there are also the default settings chosen in JavaScript syntax checkers JSLint and JSHint. The question is, what is the ultimate JavaScript style to rule them all? Let’s make a consensus based on these six style guides.

Indentation
-----------
Two spaces, not longer and no tabs: Google, npm, Node.js, Idiomatic

Tabs: jQuery

Four spaces: Crockford

Spaces between arguments and expressions
----------------------------------------
Use sparingly like below: Google, npm, Node.js
```js
project.MyClass = function(arg1, arg2) {
```
Use excessive white space like below: Idiomatic, jQuery
```js
for ( i = 0; i < length; i++ ) {
```
No expressed opinion: Crockford

Many guides remind not to have any trailing spaces!

Line length
-----------
Max 80 characters: Google, npm, Node.js, Crockford

When wrapping, other indentation than two spaces is allowed to for example align function arguments to the position where the first function argument was. Another option is to use four spaces instead of two when word wrapping long lines.

No expressed opinion: jQuery, Idiomatic

Semicolons
----------
Always use semicolons and don’t rely on implicit insertion: Google, Node.js, Crockford

Don’t use except in some situations: npm

No expressed opinion: jQuery, Idiomatic

Comments
--------
JSDoc conventions: Google, Idiomatic

In the Idiomatic Style Guide also simpler comments are allowed, but JSDoc encouraged.

No expressed opinion: npm, Node.js, jQuery, Crockford

Quotes
------
Prefer `'` over `"`: Google, Node.js

Double quotes: jQuery

No expressed opinion: npm, Idiomatic, Crockford

Variable declarations
---------------------
One per line and no commas: Node.js
```js
var foo = '';
var bar = '';
```
Multiple in one go with line ending commas like below: Idiomatic, jQuery
```js
var foo = "",
  bar = "",
  quux;
```
Multiple in one go, line ending commas and four spaces indentation: Crockford
```js
var foo = "",
    bar = "",
    quux;
```
Start with comma: npm
```js
var foo = ""
  , bar = ""
  , quux;
```
No expressed opinion: Google

Braces
------
Use opening brace on the same line: Google, npm, Node, Idiomatic,  jQuery, Crockford
```js
function thisIsBlock() {
```
These also imply that braces should be used in all cases.

The npm style guide states that braces should only be used if a block needs to wrap to the next line.

Globals
-------
Don’t use globals: Google, Crockford

Google says: “Global name conflicts are difficult to debug, and can cause intractable problems when two projects try to integrate. In order to make it possible to share common JavaScript code, we’ve adopted conventions to prevent collisions.”

Crockford states that implied global variables should never be used.

No expressed opinion: Idiomatic, jQuery, npm, Node

Naming
======

Variables
---------

Start first word lowercase and after that all words start with uppercase letter (Camel case): Google, npm, Node, Idiomatic
```js
var foo = "";
var fooName = "";
```
Constants
---------
Use uppercase with underscore: Google, npm, Node, Idiomatic
```js
var CONSTANT = 'VALUE';
var CONSTANT_NAME = 'VALUE';
```
No expressed opinion: jQuery, Crockford

Functions
---------
Start first word lowercase and after that all words start with uppercase letter (Camel case): Google, npm, Idiomatic, Node

Prefer longer, descriptive function names.
```js
function veryLongOperationName
function short()..
```
Use vocabulary like is, set, get:
```js
function isReady()
function setName()
function getName()
```
No expressed opinion: jQuery, Crockford

Arrays
------
Use plural forms: Idiomatic
```js
var documents = [];
```
No expressed opinion: Google, jQuery, npm, Node, Crockford

Objects and classes
-------------------

Use Pascal case (every word's first letter is capitalized): Google, npm, Node
```js
var ThisIsObject = new Date();
```
No expressed opinion: jQuery, Idiomatic, Crockford

Other
-----
Use all-lower-hyphen-css-case for multi-word filenames and config keys: npm

Suggested .jshintrc file
========================
[JSHint](http://www.jshint.com/) is a JavaScript syntax and style checker you can use to alert about code style issues. It integrates well into to many commonly used editors and is a nice way to enforce a common style. See JS Hint documentation for all available options: http://www.jshint.com/docs/#options Below we have created a .jshintrc file that follows the recommendations set above in this article. You can place it in the root folder of your project and JSHint-aware code editors will notice it and follow it though all code in your project.
```json
{
  "camelcase" : true,
  "indent": 2,
  "undef": true,
  "quotmark": "single",
  "maxlen": 80,
  "trailing": true,
  "curly": true
}
```
In addition to it you should add into your (browser-read) JavaScript files the following header:
```js
/* jshint browser:true, jquery:true */
```
In your Node.js files you should add:
```js
/*jshint node:true */
```
In any kind of JavaScript file it is also good to add the declaration:
```js
'use strict';
```
This will affect both JSHint and your JavaScript engine, which will become less compliant but run your JavaScript faster.

Automatically JSHint files before Git commit
If you want to make sure that all of your JS code stays compliant to the style defined in your .jshintrc, you can set the following contents to your .git/hooks/pre-commit file, which is then run each time you try to commit any new of modified files to the project:
```bash
#!/bin/bash
# Pre-commit Git hook to run JSHint on JavaScript files.
#
# If you absolutely must commit without testing,
# use: git commit --no-verify

filenames=($(git diff --cached --name-only HEAD))

which jshint &> /dev/null
if [ $? -ne 0 ];
then
  echo "error: jshint not found"
  echo "install with: sudo npm install -g jshint"
  exit 1
fi

for i in "${filenames[@]}"
do
  if [[ $i =~ \.js$ ]];
  then
    echo jshint $i
    jshint $i
    if [ $? -ne 0 ];
    then
      exit 1
    fi
  fi
done
```
Happy coding!



License
=======

Copyright (C) 2013 Seravo Oy

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
