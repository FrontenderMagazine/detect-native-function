<main id="main"><article>
Every once a while I'll test is a given function is native code -- it's an
important part of feature testing whether a function was provided by the browser
or via a third party shim which acts like the native feature.  The best way to 
detect this, of course, is evaluating the toString return value of the function.

## The JavaScript

The code to accomplish this task is fairly basic:

    function isNative(fn) {
    	return (/\{\s*\[native code\]\s*\}/).test('' + fn);
    }
    

Converting to the string representation of the function and performing a regex
match on the string is how it's done.  There isn't a better way of confirming a 
function is native code!</article>

![ydkjs-2.png][1]

## [Recent Features**][2]

*   ![Create a Sheen Logo Effect with&nbsp;CSS][3]
*   ![Vibration&nbsp;API][3]### [Vibration API][4]
    
    Many of the new APIs provided to us by browser vendors are more targeted
    toward the mobile user than the desktop user.  One of those simple APIs the
   [Vibration API][5].  The Vibration API allows developers to direct the
    device, using JavaScript, to vibrate in.
    ..

## [Incredible Demos**][6]

*   ![MooTools History&nbsp;Plugin][3]### [MooTools History Plugin][7]
    
    One of the reasons I love AJAX technology so much is because it allows us
    to avoid unnecessary page loads.  Why download the header, footer, and other 
    static data multiple times if that specific data never changes?  It's a waste of
    time, processing, and bandwidth.  Unfortunately,
    ...

*   ![AJAX Page Loads Using MooTools&nbsp;Fx.Explode][3]</main>

 [1]: img/ydkjs-2.png
 [2]: http://davidwalsh.name/tutorials/features
 [3]: img/
 [4]: http://davidwalsh.name/vibration-api
 [5]: http://www.w3.org/TR/vibration/
 [6]: http://davidwalsh.name/tutorials/demos
 [7]: http://davidwalsh.name/mootools-history