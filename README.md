# Project Summary

This project will help us better understand basic DOM manipulation by creating a paint program! Much like Microsoft Paint.

Our `index.html` has 5400 divs on it. These divs are little black squares. These squares will each act
as a pixel in our paint application. We shouldn't have to worry about our html or css in this project.
Just our JavaScript file named app.js


# Project Setup

1. Fork this repo.
2. Clone this repo to you local machine.


# Step 1 - Add a class with JavaScript

The primary thing our 'pixels' do is change color. These little guys will be the building blocks
of our works of art. With v1 of our app, let's go with just black and white pixels. Not because we
can't have colors, but we'll need to save some features for v2 right?

- First we need a way to select all the `.box` elements in the DOM
  - Notice in the our index.html file each `div` has the class of `.box`
  - We can use this `.box` class as our selector. This gives us a way to select elements in the DOM.

``` javascript
  let boxes = document.getElementsByClassName('box'); // This returns an HTMLCollection

  // You may expect "boxes" to be an Array, but it is not. It is only array-like,
  // this is an important distinction because we won't have all the nice methods that
  // a typical array has. Our best option is to turn this collection into an Array manually

  // rewrite the line above like so...

  const boxes = Array.from(document.getElementsByClassName('box'));

  // Now "boxes" is a normal Array with all the convenient methods
```

- Now we need a way to attach a function to the `click` event of the `.box` elements.

```javascript
// loop through each box in the "boxes" array
boxes.forEach(function(box) {
  // add an event listener for the 'click' event
  box.addEventListener('click', function(event) {
    // "this" is the box html element in this case
    this.classList.add('white');
  });
});
```

Our `style.css` file has a class named `.white`. All it does is change the background of the black boxes to white.

## Step 2 - Edits

Great, now we can create beautiful works of art. Black and white art, but art nonetheless. But what happens when we make a mistake? There is no way to edit our artwork. Let's fix that.

Make it so that when we double click, it changes back to black with `.remove()`

``` javascript
boxes.forEach(function(box) {
  box.addEventListener('click', function(event) {
    this.classList.add('white');
  });

  // here is the new block of code
  box.addEventListener('dblclick', function(event) {
    this.classList.remove('white');
  });
})
```

Now when you double-click a white square it should toggle back to black.

## Step 3 - Reset Button

It looks like we have a reset button. Let's make it work.

Make the reset button work

``` javascript
// select the "reset" button element
const reset = document.getElementById('reset');

// add event listener to the reset button
reset.addEventListener('click', function(event) {
  // loop over every box and remove the "white" class
  boxes.forEach(function(box) {
    box.classList.remove('white');
  });
})
```

## Step 4 - Color Pallette

Let's make our color buttons work!

Create a color variable and set the default to white. This color variable will be the class we add to things.

``` javascript
  let color = 'white';
```

Create a click event for each color which changes the color variable on click

``` javascript
  const red = document.getElementById('red');
  red.addEventListener('click', function() {
    color = 'red';
  });

  const green = document.getElementById('green');
  green.addEventListener('click', function() {
    color = 'green';
  });

  const blue = document.getElementById('blue');
  blue.addEventListener('click', function() {
    color = 'blue';
  });

  const yellow = document.getElementById('yellow');
  yellow.addEventListener('click', function() {
    color = 'yellow';
  });

  const white = document.getElementById('white');
  white.addEventListener('click', function() {
    color = 'white';
  })
```

Update the add class functionality to reflect our color variable rather than our actual class names

``` javascript
  boxes.forEach(function(box) {
    box.addEventListener('click', function(event) {
      this.classList.add(color);
    });

    box.addEventListener('dblclick', function(event) {
      this.classList.remove(color);
    });
  })
```

One last thing is a little messed up. Our reset and double click functions aren't working now.
Because we placed the 'color' variable in the place of the remove class action, it will only respect
the currently selected color.

Let's add a "colors" variable

```javascript
  const colors = ['white', 'red', 'green', 'blue', 'yellow'];
```

Now we can adjust this code block again.

``` javascript
  boxes.forEach(function(box) {
    box.addEventListener('click', function(event) {
      this.classList.add(color);
    });

    box.addEventListener('dblclick', function(event) {
      this.classList.remove.apply(this.classList, colors);
    });
  })
```

And also adjust this code block again

```javascript
const reset = document.getElementById('reset');

reset.addEventListener('click', function(event) {
  boxes.forEach(function(box) {
    box.classList.remove.apply(box.classList, colors);
  });
})
```

And we're all set! Feel free to play around and create some more features of your own!
