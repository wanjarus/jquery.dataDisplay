# [jQuery.dataDisplay.js](https://github.com/assetinfo/jquery.dataDisplay)  

> A jQuery plugin that aids the developer in writing concise conditions against elements within a form based setting, in order to control the display of those elements based on the state of the form.

--------

[![Build Status](https://img.shields.io/travis/assetinfo/jquery.dataDisplay/master.svg)](https://travis-ci.org/assetinfo/jquery.dataDisplay)
[![GitHub issues](https://img.shields.io/github/issues/assetinfo/jquery.dataDisplay.svg)](https://github.com/assetinfo/jquery.dataDisplay/issues)
[![GitHub issues](https://img.shields.io/github/issues-closed/assetinfo/jquery.dataDisplay.svg)](https://github.com/assetinfo/jquery.dataDisplay/issues?utf8=%E2%9C%93&q=is%3Aissue%20is%3Aclosed%20)
[![Download this repo](https://img.shields.io/github/downloads/assetinfo/jquery.dataDisplay/total.svg)](https://github.com/assetinfo/jquery.dataDisplay/archive/master.zip)
[![License](https://img.shields.io/github/license/assetinfo/jquery.dataDisplay.svg)](https://github.com/assetinfo/jquery.dataDisplay/blob/master/LICENSE)
[![Watch this repo](https://img.shields.io/github/watchers/assetinfo/jquery.dataDisplay.svg?label=Watch)](http://github.com/assetinfo/jquery.dataDisplay/subscription)
[![Star this repo](https://img.shields.io/github/stars/assetinfo/jquery.dataDisplay.svg?label=Star)](http://github.com/assetinfo/jquery.dataDisplay/stargazers)
[![Fork this repo](https://img.shields.io/github/forks/assetinfo/jquery.dataDisplay.svg?label=Fork)](http://github.com/assetinfo/jquery.dataDisplay/fork)

<h2 id="Demo">Demo</h2>


* [Demo on jsfiddle](https://jsfiddle.net/graydixon/tchmmn27/).

<h2 id="QuickStart">Quick start</h2>

[$.fn.dataDisplay](https://assetinfo.github.io/jquery.dataDisplay/$.fn.dataDisplay.html) (``` $(...).dataDisplay() ```) is invoked against a container which contains both the inputs and the elements beings controlled.

For each conditionally displayed element, all conditions are to be defined against a "data-display" tag, using a [syntax](#Syntax) that will be compiled into appropriate javascript closures each time the referenced inputs' state changes.

Each time the state changes on any of the inputs used in a condition, the rules defined inside the "data-display-resets" tag will be called prior to the "data-display" closures being executed, to ensure only the appropriate side-effects are applied for the given state.

* HTML
```HTML
...
    <div id="container">
        <input name="input" type="value">
        <div data-display="{input} == 'test';"
            <a>Test</a>
        </div>
    </div>
...
```

* Javascript
```javascript
...
    var dataDisplay = $('#container').dataDisplay({...options...});
...
```

<h2 id="Options">Options</h2>

Name | Type | Default | Description
----|------|---- | ----
funcs | array | {} | an array of func methods used to extend the built-in helper methods
eventName | string | '.dataDisplay' | name to bind the events against
dataAttr | string | 'dataDisplay' | data-attribute to bind DataDisplay instance to (against the $el)
condsAttr | string | 'data-display' | attribute holding the conditions on first-load
resetsAttr | string | 'data-display-resets' | attribute holding resets on first load (defined as jquery statements against $this)
initFire | bool | true | should the conditions be applied when the DataDisplay instance is created?
keyEventsFire | bool | true | should DataDisplay recheck conditions on keyup events?

<h2 id="Documentation">Documentation</h2>

* [jQuery.dataDisplay's documentation](https://assetinfo.github.io/jquery.dataDisplay/) has been generated using [jsDoc 3.5.3](https://github.com/jsdoc3/jsdoc) and a modified version of Nijiko Yonskai's [minami 1.2.3](https://github.com/gdixon/minami/) theme.

<h2 id="Installing">Installing, Building and Testing</h2>

1. [Download](https://github.com/assetinfo/jquery.dataDisplay/archive/master.zip) or clone this repo...
   ```
   $ git clone git@github.com:assetinfo/jquery.dataDisplay.git
   ```

2. Navigate to the directory containing jquery.dataDisplay and run...

   ```
   $ npm install
   ```

3. Followed by...

   ```
   $ bower install
   ```

4. Then to build run...

   ```
   $ grunt
   ```

5. And to test run...

   ```
   $ grunt test
   ```

6. To use in your own project...

   * include dependencies and minified version of jquery.dataDisplay.js

   ```HTML
   <link rel="stylesheet" href="./src/css/jquery.dataDisplay.css"/>
   <script src="./bower_components/jquery/dist/jquery.js"></script>
   <script src="./dist/jquery.dataDisplay.min.js"></script>
   ```

   * or include the optimised versions from ./dist/:

   ```HTML
   <link rel="stylesheet" href="./dist/jquery.dataDisplay.optimised.css"/>
   <script src="./dist/jquery.dataDisplay.optimised.js"></script>
   ```

   * alternatively, [require](http://requirejs.org/) jquery.dataDisplay in a [module](https://github.com/assetinfo/jquery.dataDisplay/blob/master/main.js) with the following in your config file...

   ```javascript
   require.config({
       "deps": ["main"],
       "paths": {
           "jquery": "bower_components/jquery/dist/jquery",
           "dataDisplay": "src/js/jquery.dataDisplay"
       },
       "shim": {
           "jquery": { "exports": "$" }
       }
   });
   ```

<h2 id="Syntax">Writing conditions and statements</h2>

<!-- allow for multiple side-effects in single conditions
 ... data-display="
  {val} > 0; &#124;&#124;
  {val} < 1 :: $(var).css('background','#bdbdbd'); &#124;&#124;
  {val} > 1 && {val} < 2 :: $(var).css('background','#bdbdbd');
 " ...
 -->
#### Syntax

* A condition consists of {variables}, optional [helper method calls](#Helpers) (eg length()) and optional side-effects (any statements following a double-colon (::) after a condition).

    ```HTML
    ...
        data-display="{input} == 'test' ::
            $this.css('background', '#000');"
    ...
    ```

* Any side-effects should be defined after the condition, separated by a double-colon (::).<br/>
Each condition may hold one or more side-effects where each side-effect is separated by a semicolon ';'.<br/>
Each side-effect should call a function against the $element which is passed to the closures scope as $this ($this.css(...), $this.data(...), $this.scrollTop(...)... etc).

    ```HTML
    ...
        data-display="{input} == 'test' ::
            $this.css('background', '#000'); $this.css('font-size', '16px');"
    ...
    ```

* The data-display-resets attribute should undo any actions performed by the data-display condition(s).

    ```HTML
    ...
        data-display="{input} == 'test' ::
            $this.css('background', '#000'); $this.css('font-size', '16px');"
        data-display-resets="$this.css('background', '#fff'); $this.css('font-size', '12px');"
    ...
    ```

* Logical conditions can be grouped in brackets ({input} == "value" &#124;&#124; {input} == "value2")

    ```HTML
    ...
        data-display="{input} == 'value' && ({input2} == 'test' || {input2} == 'testing') ::
            $this.css('background', '#000');"
        data-display-resets="$this.css('background', '#fff');"
    ...
    ```

* If the condition does not define any side-effects, then it will instead control the display state (display: block) - the reverse action (\*-resets) is implied.

    ```HTML
    ...
        data-display="({input} == "test" || {input} == "testing");"
    ...
    ```

* Multiple conditions may be defined against the same data-display attr by separating each condition with a double pipe (&#124;&#124;)

    ```HTML
    ...
        data-display="
            {input} == 'test' ::
                $this.css('background', '#000'); ||
            {input} == 'testing' ::
                $this.css('background', '#ddd');"
        data-display-resets="$this.css('background', '#fff');"
    ...
    ```

<h2 id="Helpers">Built-in helper methods</h2>

* **{...}** - Any value wrapped inside of '{...}' will be replaced with the value associated with the input that has the corresponding name='...' tag.

    ```HTML
    ...
       data-display="{input} == "value";"
    ...
    ```

* **!empty({...})** - returns "true" (=="true") if the given variable is NOT empty, else "false" (=="true")

    ```HTML
    ...
       data-display="!empty({input});"
    ...
    ```

* **empty({...})** - returns "true" (=="true") if the given variable is empty, else "false" (=="true")

    ```HTML
    ...
       data-display="empty({input});"
    ...
    ```

* **length({...})** - returns the length of the given variable

    ```HTML
    ...
       data-display="length({input}) > 6;"
    ...
    ```

* **is greater than or equal to** - returns the ">=" symbol (to avoid reserved characters in tags)

    ```HTML
    ...
       data-display="{input} is greater than or equal to 6;"
    ...
    ```

* **is less than or equal to** - returns the "<=" symbol (to avoid reserved characters in tags)

    ```HTML
    ...
       data-display="{input} is less than or equal to 6;"
    ...
    ```

* **is greater than** - returns the ">" symbol (to avoid reserved characters in tags)

    ```HTML
    ...
       data-display="{input} is greater than 6;"
    ...
    ```

* **is less than** - returns the "<" symbol (to avoid reserved characters in tags)

    ```HTML
    ...
       data-display="{input} is less than 6;"
    ...
    ```

* **is equal to** - returns the "==" symbols (in order to cover all operators)

    ```HTML
    ...
       data-display="{input} is equal to 6;"
    ...
    ```

#### Extending the helper methods object:

* The helper methods may be extended by providing an array of helpers in the options provided to $.fn.dataDisplay at init...

    ```javascript
    ...
       $('#container').dataDisplay({
           'funcs': {
               'sum': {
                   // define a regex pattern to match the desired expression...
                   rgx: 'sum\\({([^}]+)}\\)',
                   // and a function which will return the desired process as a string which will replace any matching groups
                   exec: function (field, ctx) {

                       var str = ... sum all values against the given field ...

                       return str;
                   }
               }
           }
       });
    ...
    ```

<h2 id="Contribute">Contribute</h2>

* Fork, make the appropriate changes, then create a pull request.
* If you're planning on adding a feature, please create an issue first detailing what you think is missing.

Otherwise your changes may be rejected.

<h2 id="License">License</h2>

The MIT License (MIT)

Copyright (c) 2017 Assetinfo (a trading style of Money Marketplace LTD)

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
