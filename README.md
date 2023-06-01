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

