# noisify

A Ruby script that creates a "noisy_*.js" file for the provided JavaScript file that attempts to console.log most function calls with the function name and argument values. This way it has a chance to work even in strict mode, because the log statement that is injected is determined by parsing the provided javascript, not at runtime.

Use at your own risk! The modified file may not work.

## Usage

```
./noisify some_file.js
```

Specify one or more lines not to noisify (function definitions that occur in places that make it difficult for noisify to modify easily):

```
./noisify some_file.js 5,19,56
```

If you want to run [JSHint][jshint] before and after and diff (requires jshint and diff):

```
./noisify some_file.js 5,19,56 --jshint
```

## Example

Noisify [Angular][angular] v1.1.5:

```
wget https://ajax.googleapis.com/ajax/libs/angularjs/1.1.5/angular.js
./noisify angular.js
```
(note: was just hacking to try to get a working version of it, so all of those line#s might not need exclusion, or maybe many more need exclusion, or maybe it just won't work.)

Then just back your original angular.js in your project and replace it with noisy_angular.js. May not work properly, and line#s may need to be different (e.g. for other versions of Angular).

If you diff, among the many other changes, you'll see:

```
< function forEach(obj, iterator, context) {
---
> function forEach(obj, iterator, context) {console.log('function forEach(' + [].slice.call( arguments ) + ')');
```

And when you run in console.log, among the other many log statements, it might contain the following, indicating the forEach was called with an empty argument and a anonymous function that takes attr:

```
function forEach(,function (attr) {
            console.log('function(' + [].slice.call(arguments) + ')');
            if (!appElement && names[attr.name]) {
              appElement = element;
              module = attr.value;
            }
          }) 
```

## License

Copyright (c) 2013 Gary S. Weaver, under the [MIT license][lic].

[lic]: http://github.com/garysweaver/noisify/blob/master/LICENSE
[angular]: http://angularjs.org/
[jshint]: http://jshint.com/
