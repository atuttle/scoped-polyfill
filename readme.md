# Scoped polyfill for HTML5 &lt;style scoped&gt; elements

This code changes the actual &lt;style&gt; node in the DOM instead of computing the styles certain nodes might get and applying them on them inline.  
This is usefull when adding new nodes to a container which is "scoped".  
It does so by prepending the styleRules selectorText with id's to syntheticaly up the styleRules' specificity and thus overriding (if defined in the scoped &lt;style&gt; node) the styles which are inherited from the main HTML.  

[an example](http://pm5544.eu/scoped/ "clicky for example")  
[an example with jQuery](http://pm5544.eu/scoped/indexjquery.htm "clicky for example")  

# Tested (and working) on:
* IE 6, 7, 8, 9 (Yes also in all the FrankensteinModes&trade;)  
* Firefox latest Mac/Windows/Linux(Ubuntu)  
* Google Chrome latest Mac/Windows/Linux(Ubuntu)  
* Opera latest Mac/Windows  
* Safari latest Mac/Windows  

# Usage:
* include scoped.js  

This will polyfill all scoped &lt;style&gt; nodes in the DOM at that point, when new ones are added later, you can call the function the constructor returns to convert that &lt;style scoped&gt; as well.  
The position of scoped.js should be after all the &lt;style scoped&lt; nodes in the document, in the case you want to initialize it yourself the position doesn't matter much (as long as it is defined before you call it ofcourse...)  

If you're a jQuery user you can also use it as follows:
Pass in a jQuery object with &lt;style scoped&gt; nodes and it will polyfill them all.  

# Features:
Tests if scoped is supported and if it is it does nothing.  
If scoped is not supported by the browser visiting that page, an empty &lt;style&gt; element is made and appended to the body.  
This is because some browsers don't have a sheet property until appended to the DOM.  
Than it sorts out the different names browsers use to get to the same properties (like rule/cssRule and sheet/getSheet/styleSheet) and returns them into the closurre to only have to check those things once, after that you can just use the compat[ name ] to access it.  
It also checks if the renaming of selectortext is prohibited or not because when changing the &lt;style&gt; its easier to just edit the selectorText instead of copying the rule, changing it and appending it to the &lt;style&gt; and removing the old one.  
 
# Notes:
This does not completely sandbox the CSS for the HTML inside the &lt;style scoped&gt;'s parentNode, it is meant to work the way scoped is going to work.  
This way you can inherit styles and override the ones you want to override without leaking those styles to the whole DOM.  
@import and other non-normal style rules are not changed so do not use them in the inline &lt;style&gt;, IE does not expose them via the DOM so to prevent incompatilbilities they are not touched by this code.  
It is possible (though the chance is very small) that an inherited styleRule's CSS selector after polyfilling still is more specific than the scoped styleRule's selector. In that case please just up the specificity of the scoped styleRule's selector and not use !important since this make using user stylesheets a lot more diffucult (for instance for visually impared people who want to override background-color, font-size and color)

# Settings
### If you would like to keep your Global scope as clean as possible you could do the following:
* open scoped.js  
* make sure your cursor is at the beginning of the file  
* press [Del] 20 times  
* save the file  
this will disable the use via jQuery since that way of working needs a reference from the global scope, but it works without it as well, ill wrap it as a jQuery plugin when there's a need but I like the fact that it works with the same code in and out of jQuery now.

