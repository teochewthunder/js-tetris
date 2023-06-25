# JS Tetris (in progress)
This is Tetris written in vanilla JavaScript. The buttons are rendered in CSS symbols, while the bulk of the action takes place within the methods of the JavaScript object, `game`.

## HTML/CSS
### General Layout
- In the head, a meta tag with viewport specified needs to be there because there is going to be a lot of tapping on a mobile platform and we don't want stuff to shift.
- A div that has the id `container`. It is meant to encapsulate all the visual elements of the game.
    - A div that has the id `header`.
        - A div that displays `stage` and `score` properties.
        - A div that displays the next block.
    - A div with the id `grid`.
        - A div that contains the current block. (JavaScript generated)
        - 20 divs (styled using `row`) of 10 divs each. Each of these sub-divs is styled with the CSS class `square`.
    - A div with the id `footer`.
        - Three divs, each with the CSS class `footer`.
        - The first and second div contain a round button for Rotate and Start/Stop functions.
        - The third div has another div styled using `pad`. This has Pause, Left, Right and Down functions.
### Grid and Blocks
- For `grid`. This caters for a 20 x 10 square grid.
    - `row`. 20 pixels height, fills entire width.
        - `square`. 20 pixels height and width. 10 of these should fit in `row`.
        - Each `square` potentially has a `before` psuedoselector if styled with a color. (`red`, `lime`, `purple`, `yellow`, `cyan`, `white`)
    - `pauseScreen` and `stopScreen`. Has a `before` pseudoselector with translucent black background and white text. Invisible by default.
- For `movingBlock`. This is a container with `overflow` property set to `hidden`.
    - A div with id `moveingBlockAspects`. It has 400% width of its parent.
        - 4 divs styled using `aspect`. Each of them have an unique id. Each of these cater for a 4 x 4 grid.
            - `square`. 20 pixels height and width.
            - Each `square` potentially has a `before` psuedoselector if styled with a color. (`red`, `lime`, `purple`, `yellow`, `cyan`, `white`)
 - For `gridNext`. This caters for a 4 x 4 square grid.
     - `square_small`. 10 pixels height and width.
     - Each `square` potentially has a `before` psuedoselector if styled with a color. (`red`, `lime`, `purple`, `yellow`, `cyan`, `white`)

## JavaScript
### Properties
- `grid`: A two-dimensional array which will be populated via JavaScript. It is supposed to have 20 elements of 10 elements each. Each element is either `null` or a string value from the `colors` array.
- `colors`: An array containing the following elements - "red", "purple", "cyan", "yellow", "lime".
- `currentBlock`: `null` by default, can be an object with two properties - `shape` and `color`.
- `nextBlock`: `null` by default, can be an object with two properties - `shape` and `color`.
- `currentPosition`: An object with properties `x`, `y` and `r`.
- `timer`: `null` by default, used to start and stop timer.
- `stage`: Integer to dictate current stage. Starts at 1.
- `score`: Integer to keep track of current score. Starts at 0.
- `paused`: Boolean that tracks if application is currently paused. `false` by default.
- `stopped`: Boolean that tracks if application is currently stopped. `false` by default
- `shapes`: A multi-dimensional array containing specifications for all the possible blocks and their possible rotations.
    - 5 elements, one for each shape.
    - Each shape has 4 elements, one for each rotation.
    - Each rotation has 4 rows...
    - ...and 4 columns. 

### Methods
- `reset()`: This method is used to reset properties to opening defaults.
- `renderGrid()`: This method is run at least once after every turn. It renders the grid after clearing the container. Since the moving block container, as well as the Pause and Stop screens are inside the grid as well, these are also re-rendered. *POSSIBLE OPTIMIZATION POINT: Keep the grid squares and moving block and Pause and Stop screens in different continers within the gris so that less re-rendering is required.*
- `getRandomBlock()`: Gets a random element from the `shapes` and `colors` arrays, and uses these to create an object to return. This will be either the value of `currentBlock` or `nextBlock`.
- `populateAspect(id, shape, color, className)`: This method populates a container, denoted by `id`. The `shape` object is a one element in the `shapes` array in a particular rotation (which makes it a two-dimensional array). `color` is the color used to render the blocks, and `className` is the class that is used to style each block ("square" or "square_small" depending on `id`).
- `positionBlock()`: Uses the `currentPosition` object to determine the `margin-left` and `margin-top` properties of `movingBlock`. It also uses `currentPosition` to determine the `margin-left` property of `movingBlockAspects` (for rotating the blocks).
- `testPosition(x, y, r)`: Uses `x`, `y` and `r` to see if that particular configuration, with the current shape, is viable (does not clash with existing blocks in the grid).
- `isFullyInFrame()`: Uses `currentPosition` and `currentShape` to determine if the block is fully visible within the grid. Returns `true` by default. This will be used by other methods - `rotateBlock()`, `moveLeft()`, `moveRight()`, `drop()`.
- `rotateBlock()`: Exits early if `stopped` or `paused` is `true`, or `isFullyInFrame()` returns `false`. Uses a "proposed" new rotation and runs it through `testPOsition()`. If `true`, update `r` property of `currentPosition` and run `positionBlock()`.
- `moveLeft()`: Exits early if `stopped` or `paused` is `true`, or `isFullyInFrame()` returns `false`. Uses a "proposed" new position and runs it through `testPOsition()`. If `true`, update `x` property of `currentPosition` and run `positionBlock()`.
- `moveRight()`: Exits early if `stopped` or `paused` is `true`, or `isFullyInFrame()` returns `false`. Uses a "proposed" new position and runs it through `testPOsition()`. If `true`, update `x` property of `currentPosition` and run `positionBlock()`.
- `moveDown()`: Exits early if `stopped` or `paused` is `true`. Uses a "proposed" new position and runs it through `testPOsition()`. If `true`, update `y` property of `currentPosition` and run `positionBlock()`.
- `drop()`: Exits early if `stopped` or `paused` is `true`, or `isFullyInFrame()` returns `false`. Uses a "proposed" new position and runs it through `testPOsition()`. Pretty much repeats the functionality of `moveDown()` until `false` is returned from `testPosition`, moving the block down one row at a time. Runs `stopBlock()` at the end.
- `stopBlock()`: The main point of this method is to "copy" the colors from the block over to the grid. During the process, however...
    - completed rows are turned white.
    - completed rows "disappear" and the grid is compacted.
    - scores are calculated.
- `setPause(isAuto)`: This toggles the `paused` property between `true` and `false`. In addition, if `isAuto` is `false`, it will display the PAUSED screen if `paused` is `true`. Timer functions are disabled if `paused` is `true`, and enabled otherwise.
- `setStop()`: This toggles the `stopped` property between `true` and `false`. It will display the GAME OVER screen if `paused` is `true`. `reset()` is called if `stopped` is `true`, and timer functions enabled otherwise.
- `setScore()`: This populates the HTML placeholder with the value of the `score` property.
- `setStage()`: This populates the HTML placeholder with the value of the `stage` property.
- `addToScore()`: Determines the points total from the number of completed rows, increments `score` then updates `stage` based on `score`. Displays are updated as well.
- `getInterval()`: Determines the interval in milliseconds based on `stage`.
- `setCompletedRows(color)`: Goes through `grid`. If the row is "completed", the `completed` variable is incremented, and the entire row is set to `color`. This is not hardcoded because of the animation - we are going to set it to white, then nothing ("x"), then call `compactRows`.
- `compactRows()`: Traverses `grid`, and any row that does *not* contain "x" is pushed to a new temp `grid`. The temp `grid` is then padded (using `unshft()`) with "blank" rows to make up the full 20 rows before the original `grid` is overwritten by the temp `grid`.

