# JS Tetris (in progress)
This is Tetris written in vanilla JavaScript. The buttons are rendered in CSS symbols, while the bulk of the action takes place within the methods of the JavaScript object, `game`.

## HTML
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
## CSS
- For `grid`. This caters for a 20 x 10 square grid.
    - `row`. 20 pixels height, fills entire width.
        - `square`. 20 pixels height and width. 10 of these should fit in `row`.
        - Each `square` potentially has a `before` psuedoselector if styled with a color. (`red`, `lime`, `purple`, `yellow`, `cyan`, `white`)
    - `pauseScreen` and `stopScreen`. Has a `before` pseudoselector with translucent black background and white text. Invisible by default.
- For `movingBlock`.

## JavaScript
### Properties

### Methods

