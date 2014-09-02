# Detect if a Function is Native Code with JavaScript

Every once a while I'll test if a given function is native code -- it's an important part of feature testing whether a function was provided by the browser or via a third party shim which acts like the native feature. The best way to detect this, of course, is evaluating the `toString` return value of the function.

## The JavaScript

The code to accomplish this task is fairly basic:

    function isNative(fn) {
        return (/\{\s*\[native code\]\s*\}/).test('' + fn);
    }

Converting to the string representation of the function and performing a regex match on the string is how it's done. <span style="text-decoration: =line-through;">There isn't a better way of confirming a function is native code!</span>

## Update!

Lodash creator John-David Dalton has provided a [better solution][1]:

    ;(function() {

      // Used to resolve the internal `[[Class]]` of values
      var toString = Object.prototype.toString;
  
      // Used to resolve the decompiled source of functions
      var fnToString = Function.prototype.toString;
  
      // Used to detect host constructors (Safari > 4; really typed array specific)
      var reHostCtor = /^\[object .+?Constructor\]$/;

      // Compile a regexp using a common native method as a template.
      // We chose `Object#toString` because there's a good chance it is not being mucked with.
      var reNative = RegExp('^' +
        // Coerce `Object#toString` to a string
        String(toString)
        // Escape any special regexp characters
        .replace(/[.*+?^${}()|[\]\/\\]/g, '\\$&')
        // Replace mentions of `toString` with `.*?` to keep the template generic.
        // Replace thing like `for ...` to support environments like Rhino which add extra info
        // such as method arity.
        .replace(/toString|(function).*?(?=\\\()| for .+?(?=\\\])/g, '$1.*?') + '$'
      );
  
      function isNative(value) {
        var type = typeof value;
        return type == 'function'
          // Use `Function#toString` to bypass the value's own `toString` method
          // and avoid being faked out.
          ? reNative.test(fnToString.call(value))
          // Fallback to a host object check because some environments will represent
          // things like typed arrays as DOM methods which may not conform to the
          // normal native pattern.
          : (value && type == 'object' && reHostCtor.test(toString.call(value))) || false;
      }
  
      // export however you want
      module.exports = isNative;
    }());

So there you have it -- a better solution for detecting if a method is native. Of course you shouldn't use this as a form of security -- it's only to hint toward native support!

[1]: https://gist.github.com/jdalton/5e34d890105aca44399f