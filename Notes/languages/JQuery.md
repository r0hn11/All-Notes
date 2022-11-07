**[Documentation](https://api.jquery.com/)**
## What is JQuery and why to use?
- A fast, small and feature rich Javascript library
- Easier DOM manipulation, traversal
- Efficient event handling, aimations
- Easier AJAX application

## Compressed vs Uncompressed JQuery versions
### Compressed
- A compressed version is used for production purpose when the application is going to be deployed on server and needs to be compact
- It is lighter and smaller in size
### Uncompressed
- Uncompressed version gives us the access to source code for debugging purpose
- It's bit larger in size

## How to use?
1. **Using a downloaded file**
	- A downloadable .js file can be downloaded from official jquery page [here](https://jquery.com/download/)
	- Put this .js file inside working directory
	- Use script tag for referencing the file just like any other .js file
	`<script src="js/jquery-3.6.1.min.js" charset="utf-8"></script>`
2. **Using CDN (content delivery network)**
	- JQuery can be accessed through different domains like **JQuery, Google, Microsoft** etc.
	- JQuery cdn : [click here](https://releases.jquery.com/)
	- Paste the tag with link provided on the website and we're good to go

## Syntax

### Selectors
#### Universal selector
- `*` denotes all elements
- `$(*)` will select all existing elements from the document
#### Element selectors
- Selects all elements having the specified selector
- `$('p')` will select all `p` tags
#### id selectors
- Selects an element with specified id
- `<div id="root"> </div>` is a tag with id `root` can be selected as `$('#root')`.
- JS provides default id accessing hence as a shorthand we can use id as a variable e.g. `$(root)`. This is not recommended as it can cause anamolies with other variables
#### class selectors
- Selects all elements with specified class
- `$('.odd')` will select all elements
#### Combinations
- **element + class** `$('div.odd')`
- **element + children count** `$('div:first')`, ``$('div:last')``, `$('div:nth-child(2n)')`, `$('div:nth-child(3n)')`

### Actions

- A general accessing of element and its action can done by
`$(selector).action()`
- Execute a  function if action is completed
```
$(selector).action(function(){
	// your code
})
```

#### ready()
- Triggers when element/document is ready to avoid any errors related to absence of that element
`$(doccument).ready(function(){ //your code; })`
- As a shorthand, only `$` can be used instead of `$(document).ready`

#### click()
- Triggers click action when called
`$('#root').click()`
- Custom function can be defined to execute when element is clicked.
`$('#root').click(function(){ //code })`

#### hide()
- Hides the selected element when called
`$(selector).hide()`
- If want to hide a specific element which is clicked then we can use `this` keyword
```
// suppose we have multiple elements under 'selector'
$(selector).action(function(){
	this.hide()
})
```
- hide can take upto 2 arguments
`$(selector).hide(1000)` will hide element after 1000ms
`$(selector).hide(500, callback)` will call the callback after element is hid

#### show()
- Unhides hidden elements
`$(selector).show()`
 If want to hide a specific element which is clicked then we can use `this` keyword
```
// suppose we have multiple elements under 'selector'
$(selector).action(function(){
	this.hide()
})
```
- hide can take upto 2 arguments
`$(selector).hide(1000)` will hide element after 1000ms
`$(selector).hide(500, callback)` will call the callback after element is shown

#### toggle()
- Toggles visibility of element
- Shows if hidden, hides if visible

#### Animation functions in visibility
- **fadeOut** - `$(selector).fadeOut(1000)` - fades out(hides) in given time
- **fadeIn** - `$(selector).fadeIn(1000)` - fades in(shows) in given time
- **fadeToggle** - `$(selector).fadeToggle(1000)` - fades out(hides) or fades in(shows) in given time
- **fadeTo** - `$(selector).fadeTo(1000, 0.5)` - fades to given opacity in given time `fadeTo(time,opacity)`
- **slideUp** - `$(selector).slideUp(1000)` - slides element up(hides) in given time
- **slideDown** `$(selector).slideUp(1000)` - slides element down(shows) in given time
- **slideToggle** `$(selector).slideToggle(1000)` - slides element down(shows) or slides element up(hides) in given time
**Note**: callbacks can be used with above animation functions. For these methods, speed and callback parameters are optional

#### animate()
- Used to animate an element to given properties
- Takes an object of properties as a parameter
- Second parameter is time for the animation. It can be optional.
```
$('#root').animate({
	opacity: '0.5',
	width: '50px',
	height: '100px'
})
```
- **Queues**: We can queue up the animations for one by one execution
```
$('#root').animate({opacity: '0.5'},2000);
$('#root').animate({opacity: '1'},2000);
$('#root').animate({opacity: '0.5'},2000);
```
- **stop()**: We can stop the animation at required point by stop() method. Callback can be used as parameter.

### Events
- Events are triggers that are executed when a action is performed by user while using the appllication
- **List of events:**
	- **Mouse events** : click, dblclick, mouseenter, mouseleave, mousedown, mouseup, hover
    - **Keyboard event** : keypress, keydown, MediaKeyStatusMap
    - **Form events** : submit, change, focus, blur
    - **Document/Window events** : load, resize, scroll, unload

### on method
- **on** method allows to use functions on particular event
- Advantage of **on** method is that, we can declare functionalities for multiple events. Method takes an object with key value pairs like `event:function`
```
$('#root').on({
	click: clickFunction,
	hover: hoverFunction
})
```

### DOM related methods
#### text()
- returns the innerText of element `$('#root').text();`
- Text can be passed to the method to update text of element `$('#root').text("This is new text");`
#### html()
- returns the html of element `$('#root').html();
- Text can be passed to the method to update text of element `$('#root').html("This is new text");`
#### val()
- **val()** method is used for manipulation/access of form elements
- `$('input').val()` will return value of input
- `$('input').val('New value')` will set value of input
#### empty()
- removes content of element `$('#root').empty()`
#### remove()
- deletes the element
#### addClass()
- adds a given class(string) to the given element `$('#root').addClass('myclass')`
#### removeClass()
- removes a given class(string) to the given element `$('#root').removeClass('myclass')`
#### toggleClass()
- toggles a given class(string) to the given element `$('#root').toggleClass('myclass')`

### CSS manipulation
- CSS of an element can be changed as required
- Fetching a property of element `$('#root').css('property')`
- Setting a property of element `$('#root').css('property', 'value')`

### AJAX
- Please refer various sources.