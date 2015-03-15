## High School Learn to Hack Snake
### Educational Requirements:
* Must be in as few files as possible to reduce complexity and room for error.
    * Ideally, we can contain the entire project in a single `.js` file.
* Write no lines of code that cannot be explained at a high school, beginner level.
* Attempt to instill "good practice" concepts: no ugly hacks, programming (for something this simple) should be straightforward and make sense.

### General Structure:
* In HTML, set up HTML5 canvas, set JavaScript parameters (especially if using a canvas sized to window dimensions), set up `Play Game` button that calls JS function `playGame()`
* Pseudocode:
```
playGame() {
  // Initialization of game: setting snake direction, position

  setTarget()

  while(!gameOver()) {
    getInput()
    if (directionChanged()) updateDirectionHead()
    updateDirectionTail()

    if (hasEatenBlock()) {
      growSnake()
      incrementScore()
      setTarget()
    }
    else if (hasReachedEdge() || hasHitSelf()) gameOver()
    else moveSnake()

    sleep(sleepTime)
  }
}
```
* Score will be displayed on HTML element that gets overwritten on each `incrementScore()` call

### JavaScript Implementation:

Within each of these higher-level functions, we'll have functions that help with interacting with the "grid" itself

```
setLink(xPos, yPos) {}
eraseLink(xPos, yPos) {}
```

And in additional to globals specifying grid size, block size/color, etc., it'll probably be easiest to keep head/tail information as globals as well:

```javascript
var headPosX = 0; var headPosY = 0;
var headDirection = 0;
var tailPosX = 0; var tailPos Y = 0;
var tailDirection = 0;
```

* As we design we'll note potential opportunities for teaching new concepts
    * Stack/queue in direction changing
    * Algorithmic complexity in moving the snake

### JavaScript Technical Details:
* Queues
    * It appears that JS arrays have a `.push()` function to enqueue and a `.shift()` function to dequeue and return the top of the queue
    * Unfortunately, `.shift()` is not O(1), but the queue (probably) won't be large enough at any point in time for this to be an issue
* Getting keyboard input from user
    * Add an event listener to the document to register keypresses.
        * May have to be asynchonous/non-blocking to work in our "infinite" while loop. More on this [here](http://javascript.info/tutorial/keyboard-events).
    * On registering keypress, set certain global vars that are used in `playGame()` to change snake direction (possibly able to set up more elegantly without global state)
    * Example from Stack Overflow:
```javascript
document.addEventListener('keydown', function(event) {
    if(event.keyCode == 37) {
        alert('Left was pressed');
    }
    else if(event.keyCode == 39) {
        alert('Right was pressed');
    }
});
```
* Setting a target requires generating a random number
    * `Math.random()` genearates random decimal
    * `Math.floor((Math.random() * b) + a);` returns a random int between a and b (use this with canvas dimensions)
* Gameboard fitted to window dimensions (or fraction of window dimensions)
    * It appears that sizing relative to the window when creating the canvas object (i.e. `width = 70%`) is not supported
    * In the JS file, trying `c.width = window.innerWidth * 0.7` (and similar on the height aspect seemed to work well enough)
    * With dynamic sizing, will need to either keep set "grid" dimensions (thereby resizing the size/aspect ratio of the links/targets &mdash not ideal), or set the grid dimensions based on the canvas size (in pixels) to maintain square links
