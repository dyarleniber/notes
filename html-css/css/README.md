[Back](../README.md)

# CSS

## Basic CSS

Cascading Style Sheets (CSS) tell the browser how to display the text and other content that you write in HTML.

Note that CSS is case-sensitive so be careful with your capitalization.

CSS has been adopted by all major browsers and allows you to control:

- Color
- Fonts
- Positioning
- Spacing
- Sizing
- Decorations
- Transitions

The idea behind CSS is that you can use a selector to target an HTML element in the DOM (Document Object Model) and then apply a variety of attributes to that element to change the way it is displayed on the page.

> When a web page is loaded, the browser creates the DOM (Document Object Model) of the page. The HTML DOM is a W3C (World Wide Web Consortium) standard for how to get, change, add, or delete HTML elements.

```html
<link href="https://fonts.googleapis.com/css?family=Lobster" rel="stylesheet" type="text/css">
<style>
  .red-text {
    color: red;
  }

  h2 {
    font-family: Lobster, monospace;
  }

  p {
    font-size: 16px;
    font-family: monospace;
  }

  .thick-green-border {
    border-color: green;
    border-width: 10px;
    border-style: solid;
    border-radius: 50%;
  }

  .smaller-image {
    width: 100px;
  }

  .silver-background {
    background-color: silver;
  }

  [type='checkbox'] {
    margin-top: 10px;
    margin-bottom: 15px;
  }
</style>

<h2 class="red-text">CatPhotoApp</h2>
<main>
  <p class="red-text">Click here to view more <a href="#">cat photos</a>.</p>

  <a href="#"><img class="smaller-image thick-green-border" src="https://bit.ly/fcc-relaxing-cat" alt="A cute orange cat lying on its back."></a>

  <div class="silver-background">
    <p>Things cats love:</p>
    <ul>
      <li>cat nip</li>
      <li>laser pointers</li>
      <li>lasagna</li>
    </ul>
    <p>Top 3 things cats hate:</p>
    <ol>
      <li>flea treatment</li>
      <li>thunder</li>
      <li>other cats</li>
    </ol>
  </div>

  <form action="https://freecatphotoapp.com/submit-cat-photo" id="cat-photo-form">
    <label><input type="radio" name="indoor-outdoor" checked> Indoor</label>
    <label><input type="radio" name="indoor-outdoor"> Outdoor</label><br>
    <label><input type="checkbox" name="personality" checked> Loving</label>
    <label><input type="checkbox" name="personality"> Lazy</label>
    <label><input type="checkbox" name="personality"> Energetic</label><br>
    <input type="text" placeholder="cat photo URL" required>
    <button type="submit">Submit</button>
  </form>
</main>
```

### CSS Selectors

In CSS, selectors are patterns used to select DOM elements.

[CSS Selectors Cheat Sheet](https://www.freecodecamp.org/news/css-selectors-cheat-sheet/)

### Box Model

All HTML elements are essentially little rectangles. Three important properties control the space that surrounds each HTML element: padding, margin, and border.

An element's padding controls the amount of space between the element's content and its border.

An element's margin controls the amount of space between an element's border and surrounding elements.

> If you set an element's margin to a negative value, the element will grow larger.

#### Clockwise Notation

Instead of specifying an element's padding-top, padding-right, padding-bottom, and padding-left or element's margin-top, margin-right, margin-bottom, and margin-left properties individually, you can specify them all in one line using Clockwise Notation, like this:

```css
padding: 10px 20px 10px 20px;
margin: 10px 20px 10px 20px;
```

These four values work like a clock: top, right, bottom, left, and will produce the exact same result as using the side-specific padding instructions.

### CSS Units

CSS has several different units for expressing a length. Length is a number followed by a length unit, such as 10px, 2em, etc.

Pixels for instance, are a type of length unit, which is what tells the browser how to size or space an item. In addition to px, CSS has a number of different length unit options that you can use.

> A whitespace cannot appear between the number and the unit. However, if the value is 0, the unit can be omitted.

There are two types of length units: absolute and relative. Absolute Lengths and Relative Lengths.

- [CSS Units](https://www.w3schools.com/cssref/css_units.asp)

#### Absolute Lengths

The absolute length units are fixed and a length expressed in any of these will appear as exactly that size. Absolute length units approximate the actual measurement on a screen, but there are some differences depending on a screen's resolution.

> Absolute length units are not recommended for use on screen, because screen sizes vary so much. However, they can be used if the output medium is known, such as for print layout.

The most used absolute lengths is the Pixel (px). Pixels are relative to the viewing device. For low-dpi devices, 1px is one device pixel (dot) of the display. For printers and high resolution screens 1px implies multiple device pixels.

#### Relative Lengths

Relative length units specify a length relative to another length property. Relative length units scale better between different rendering medium.

The most used relative lengths:

Unit | Description
--- | ---
em | Relative to the font-size of the element (2em means 2 times the size of the current font)
rem | Relative to font-size of the root element
% | Relative to the parent element
vw | Relative to 1% of the width of the viewport
vh | Relative to 1% of the height of the viewport

> The em and rem units are practical in creating perfectly scalable layout. Viewport = the browser window size. If the viewport is 50cm wide, 1vw = 0.5cm.

### display Property

The display property specifies the display behavior (the type of rendering box) of an element.

Some examples:

Value | Description
--- | ---
inline | Displays an element as an inline element (like `<span>`). Any height and width properties will have no effect.
block | Displays an element as a block element (like `<p>`). It starts on a new line, and takes up the whole width.
contents | Makes the container disappear, making the child elements children of the element the next level up in the DOM.
flex | Displays an element as a block-level flex container.
grid | Displays an element as a block-level grid container.
inline-block | Displays an element as an inline-level block container. The element itself is formatted as an inline element, but you can apply height and width values.
inline-flex | Displays an element as an inline-level flex container.
inline-grid | Displays an element as an inline-level grid container.
none | The element is completely removed.
initial | Sets this property to its default value.
inherit | Inherits this property from its parent element.

- [All property values](https://www.w3schools.com/cssref/pr_class_display.asp)

### position Property

The position property specifies the type of positioning method used for an element (static, relative, absolute, fixed, or sticky).

#### position: static;

HTML elements are positioned static by default.

Static positioned elements are not affected by the top, bottom, left, and right properties.

An element with `position: static;` is not positioned in any special way. It is always positioned according to the normal flow of the page.

#### position: relative;

An element with `position: relative;` is positioned relative to its normal position.

Setting the top, right, bottom, and left properties of a relatively-positioned element will cause it to be adjusted away from its normal position. Other content will not be adjusted to fit into any gap left by the element.

#### position: fixed;

An element with `position: fixed;` is positioned relative to the viewport, which means it always stays in the same place even if the page is scrolled. The top, right, bottom, and left properties are used to position the element.

A fixed element does not leave a gap in the page where it would normally have been located.

#### position: absolute;

An element with `position: absolute;` is positioned relative to the nearest positioned ancestor (instead of positioned relative to the viewport, like fixed).

However; if an absolute positioned element has no positioned ancestors, it uses the document body, and moves along with page scrolling.

Note: A "positioned" element is one whose position is anything except static.

#### position: sticky;

An element with `position: sticky;` is positioned based on the user's scroll position.

A sticky element toggles between relative and fixed, depending on the scroll position. It is positioned relative until a given offset position is met in the viewport - then it "sticks" in place (like position:fixed).

> Note: Internet Explorer, Edge 15 and earlier versions do not support sticky positioning. Safari requires a -webkit- prefix (see example below). You must also specify at least one of top, right, bottom or left for sticky positioning to work.

#### position: initial;

Sets this property to its default value.

#### position: inherit;

Inherits this property from its parent element.

#### Overlapping Elements

When elements are positioned, they can overlap other elements.

The `z-index` property specifies the stack order of an element (which element should be placed in front of, or behind, the others).

An element can have a positive or negative stack order.

- [CSS Positioning](https://www.w3schools.com/css/css_positioning.asp)

### box-sizing Property

The CSS box-sizing property allows us to include the padding and border in an element's total width and height. It defines how the width and height of an element are calculated: should they include padding and borders, or not.

Value	| Description
--- | ---
content-box |	Default. The width and height properties (and min/max properties) includes only the content. Border and padding are not included.
border-box | The width and height properties (and min/max properties) includes content, padding and border.
initial | Sets this property to its default value.
inherit | Inherits this property from its parent element.

- [Box Sizing](https://www.w3schools.com/css/css3_box-sizing.asp)

#### box-sizing Reset Method

```css
html {
  box-sizing: border-box;
}
*, *:before, *:after {
  box-sizing: inherit;
}
```

### float and clear

The CSS `float` property specifies how an element should float.

The CSS `clear` property specifies on which sides of an element floating elements are not allowed to float.

> Note: Absolutely positioned elements ignore the float property.
> Note: Elements after a floating element will flow around it. To avoid this, use the clear property.

float Value | Description
--- | ---
none | The element does not float, (will be displayed just where it occurs in the text). This is default.
left | The element floats to the left of its container.
right | The element floats the right of its container.
initial | Sets this property to its default value.
inherit | Inherits this property from its parent element.

clear Value	| Description
--- | ---
none | Default. Allows floating elements on both sides.
left | No floating elements allowed on the left side.
right | No floating elements allowed on the right side.
both | No floating elements allowed on either the left or the right side.
initial | Sets this property to its default value.
inherit | Inherits this property from its parent element.

- [float](https://www.w3schools.com/css/css_float.asp)

### line-height Property

The `line-height` property specifies the height of a line.

> Note: Negative values are not allowed.

line-height Value	| Description
--- | ---
normal | A normal line height. This is default.
number | A number that will be multiplied with the current font-size to set the line height.
length | A fixed line height in px, pt, cm, etc.
% | A line height in percent of the current font size.
initial | Sets this property to its default value.
inherit | Inherits this property from its parent element.

- [line-height](https://www.w3schools.com/cssref/pr_dim_line-height.asp)

### font-weight Property

The `font-weight` property sets how thick or thin characters in text should be displayed.

font-weight Value	| Description
--- | ---
normal | Defines normal characters. This is default.
bold | Defines thick characters.
bolder | Defines thicker characters.
lighter | Defines lighter characters.
100 or 200 or 300 or 400 or 500 or 600 or 700 or 800 or 900	| Defines from thin to thick characters. 400 is the same as normal, and 700 is the same as bold.
initial	| Sets this property to its default value.
inherit	| Inherits this property from its parent element.

- [font-weight](https://www.w3schools.com/cssref/pr_font_weight.asp)

### Background

#### background Property

The background property is a shorthand property for:

- background-color
- background-image
- background-position
- background-size
- background-repeat
- background-origin
- background-clip
- background-attachment

It does not matter if one of the values above are missing, e.g. `background: #ff0000 url(smiley.gif);` is allowed.

- [background Property](https://www.w3schools.com/cssref/css3_pr_background.asp)

#### Multiple Backgrounds

We can actually set multiple background images if we want. We do that by separating the values with commas.

```css
body {
  background-image: 
    url(image-one.jpg),
    url(image-two.jpg);
  background-position:
    top right, /* this positions the first image */
    bottom left; /* this positions the second image */
  background-repeat:
    no-repeat; /* this applies to both images */
}
```

The stacking order of multiple background is “first is on top.”.

Gradients are applied through background-image, so they can be used as part of all this. For example, you could set a transparent gradient over a raster image.

```css
.tinted-image {
  
  width: 300px;
  height: 200px;
  
  background: 
    /* top, transparent red */ 
    linear-gradient(
      rgba(255, 0, 0, 0.45), 
      rgba(255, 0, 0, 0.45)
    ),
    /* bottom, image */
    url(https://s3-us-west-2.amazonaws.com/s.cdpn.io/3/owl1.jpg);
}
```

### filter Property

The `filter` property defines visual effects (like blur and saturation) to an element (often `<img>`).

```css
filter: none | blur() | brightness() | contrast() | drop-shadow() | grayscale() | hue-rotate() | invert() | opacity() | saturate() | sepia() | url();
```

Example:

```css
img {
  filter: grayscale(100%);
}
```

### content Property

The content property is used with the ::before and ::after pseudo-elements, to insert generated content.

```css
content: normal|none|counter|attr|string|open-quote|close-quote|no-open-quote|no-close-quote|url|initial|inherit;
```

```html
<!DOCTYPE html>
<html>
<head>
  <style>
    a::after {
      content: " (" attr(href) ")";
    }
  </style>
</head>
<body>
  <h1>The content Property</h1>
  <p>The content property is used to insert generated content.</p>
  <p>Look at our:</p>
  <p>
    <a href="https://www.w3schools.com/css/">CSS Tutorial</a>
    <br>
    <a href="https://www.w3schools.com/cssref/">CSS Reference</a>
  </p>
</body>
</html>
```

- [content Documentation](https://www.w3schools.com/cssref/pr_gen_content.asp)

### Entities

Example:

All `<h2>` elements will be displayed with this character at the end.

```css
<style>
  h2:after {
    content: ' \00A7';
  }
</style>
```

- [entity table](https://www.w3schools.com/cssref/css_entities.asp)

### word-break Property

The `word-break` property specifies how words should break when reaching the end of a line.

Value	| Description
--- | ---
normal | Default value. Uses default line break rules.
break-all | To prevent overflow, word may be broken at any character.
keep-all | Word breaks should not be used for Chinese/Japanese/Korean (CJK) text. Non-CJK text behavior is the same as value "normal".
break-word | To prevent overflow, word may be broken at arbitrary points.
initial | Sets this property to its default value.
inherit | Inherits this property from its parent element.

- [word-break](https://www.w3schools.com/cssref/css3_pr_word-break.asp)

### Transitions

CSS transitions allows you to change property values smoothly, over a given duration.

To create a transition effect, you must specify two things:

- The CSS property you want to add an effect to.
- The duration of the effect.

> Note: If the duration part is not specified, the transition will have no effect, because the default value is 0.

The transition property is a shorthand property used to represent up to four transition-related longhand properties:

```css
.example {
    transition: [transition-property] [transition-duration] [transition-timing-function] [transition-delay];
}
```

```css
div {
  transition: background-color 0.5s ease;
  background-color: red;
}
div:hover {
  background-color: green;
}
```

You can specify a particular property as we have above, or use a value of “all” to refer to transition properties.

```css
div {
  transition: all 0.5s ease;
  background: red;
  padding: 10px;
}
div:hover {
  background: green;
  padding: 20px;
}
```

In this above example, both background and padding will transition.

You can change several property values, the following example adds a transition effect for both the width and height property, with a duration of 2 seconds for the width and 4 seconds for the height:

```css
div {
  transition: width 2s, height 4s;
}
```

The transition-timing-function property can have the following values:

`ease` - Specifies a transition effect with a slow start, then fast, then end slowly (this is default).
`linear` - Specifies a transition effect with the same speed from start to end.
`ease-in` - Specifies a transition effect with a slow start.
`ease-out` - Specifies a transition effect with a slow end.
`ease-in-out` - Specifies a transition effect with a slow start and end.
`cubic-bezier(n,n,n,n)` - Lets you define your own values in a cubic-bezier function.

- [Transitions](https://www.w3schools.com/css/css3_transitions.asp)

### Override Class Declarations

We just proved that browsers read CSS from top to bottom in order of their declaration. That means that, in the event of a conflict, the browser will use whichever CSS declaration came last. 

Declarations override class declarations, regardless of where they are declared in your style element CSS.

Inline styles will override all the CSS declarations in your style element.

In many situations, you will use CSS libraries. These may accidentally override your own CSS. So when you absolutely need to be sure that an element has specific CSS, you can use `!important`.

### Use Hex Code for Specific Colors

In CSS, we can use 6 hexadecimal digits to represent colors, two each for the red (R), green (G), and blue (B) components. For example, `#000000` is black and is also the lowest possible value.

From these three pure colors (red, green, and blue), we can vary the amounts of each to create over 16 million other colors! For example, orange is pure red, mixed with some green, and no blue. In hex code, this translates to being `#FFA500`.

The digit 0 is the lowest number in hex code, and represents a complete absence of color.

The digit F is the highest number in hex code, and represents the maximum possible brightness.

You can shorten it. For example, red's hex code `#FF0000` can be shortened to `#F00`. This shortened form gives one digit for red, one digit for green, and one digit for blue.

This reduces the total number of possible colors to around 4,000. But browsers will interpret `#FF0000` and `#F00` as exactly the same color.

### Use RGB values to Color Elements

The RGB value for black looks like this: `rgb(0, 0, 0)`.

The RGB value for white looks like this: `rgb(255, 255, 255)`.

Instead of using six hexadecimal digits like you do with hex code, with RGB you specify the brightness of each color with a number between 0 and 255.

If you do the math, the two digits for one color equal 16 times 16, which gives us 256 total values. So RGB, which starts counting from zero, has the exact same number of possible values as hex code.

Just like with hex code, you can mix colors in RGB by using combinations of different values.

### Use CSS Variables to change several elements at once

CSS Variables are a powerful way to change many CSS style properties at once by changing only one value.

#### Create a custom CSS Variable

To create a CSS variable, you just need to give it a name with two hyphens in front of it and assign it a value like this:

```css
--penguin-skin: gray;
```

This will create a variable named `--penguin-skin` and assign it the value of gray. Now you can use that variable elsewhere in your CSS to change the value of other elements to gray.

After you create your variable, you can assign its value to other CSS properties by referencing the name you gave it.

```css
background: var(--penguin-skin);
```

This will change the background of whatever element you are targeting to gray because that is the value of the `--penguin-skin` variable. Note that styles will not be applied unless the variable names are an exact match.

#### Attach a Fallback value to a CSS Variable

When using your variable as a CSS property value, you can attach a fallback value that your browser will revert to if the given variable is invalid.

Note: This fallback is not used to increase browser compatibility, and it will not work on IE browsers. Rather, it is used so that the browser has a color to display if it cannot find your variable.

Here's how you do it:

```css
background: var(--penguin-skin, black);
```

This will set background to black if your variable wasn't set. Note that this can be useful for debugging.

#### Improve Compatibility with Browser Fallbacks

When working with CSS you will likely run into browser compatibility issues at some point. This is why it's important to provide browser fallbacks to avoid potential problems.

When your browser parses the CSS of a webpage, it ignores any properties that it doesn't recognize or support. For example, if you use a CSS variable to assign a background color on a site, Internet Explorer will ignore the background color because it does not support CSS variables. In that case, the browser will use whatever value it has for that property. If it can't find any other value set for that property, it will revert to the default value, which is typically not ideal.

This means that if you do want to provide a browser fallback, it's as easy as providing another more widely supported value immediately before your declaration. That way an older browser will have something to fall back on, while a newer browser will just interpret whatever declaration comes later in the cascade.

#### Inherit CSS Variables

When you create a variable, it is available for you to use inside the selector in which you create it. It also is available in any of that selector's descendants. This happens because CSS variables are inherited, just like ordinary properties.

To make use of inheritance, CSS variables are often defined in the `:root` element.

`:root` is a pseudo-class selector that matches the root element of the document, usually the html element. By creating your variables in `:root`, they will be available globally and can be accessed from any other selector in the style sheet.

#### Change a variable for a specific area

When you create your variables in `:root` they will set the value of that variable for the whole page.

You can then over-write these variables by setting them again within a specific element.

#### Penguin example

```html
<style>
  :root {
    --penguin-skin: gray;
    --penguin-belly: pink;
    --penguin-beak: orange;
  }

  body {
    background: var(--penguin-belly, #c6faf1);
  }

  .penguin {
    --penguin-belly: white;
    position: relative;
    margin: auto;
    display: block;
    margin-top: 5%;
    width: 300px;
    height: 300px;
  }

  .right-cheek {
    top: 15%;
    left: 35%;
    background: var(--penguin-belly, pink);
    width: 60%;
    height: 70%;
    border-radius: 70% 70% 60% 60%;
  }

  .left-cheek {
    top: 15%;
    left: 5%;
    background: var(--penguin-belly, pink);
    width: 60%;
    height: 70%;
    border-radius: 70% 70% 60% 60%;
  }

  .belly {
    top: 60%;
    left: 2.5%;
    background: var(--penguin-belly, pink);
    width: 95%;
    height: 100%;
    border-radius: 120% 120% 100% 100%;
  }

  .penguin-top {
    top: 10%;
    left: 25%;
    background: var(--penguin-skin, gray);
    width: 50%;
    height: 45%;
    border-radius: 70% 70% 60% 60%;
  }

  .penguin-bottom {
    top: 40%;
    left: 23.5%;
    background: var(--penguin-skin, gray);
    width: 53%;
    height: 45%;
    border-radius: 70% 70% 100% 100%;
  }

  .right-hand {
    top: 0%;
    left: -5%;
    background: var(--penguin-skin, gray);
    width: 30%;
    height: 60%;
    border-radius: 30% 30% 120% 30%;
    transform: rotate(45deg);
    z-index: -1;
  }

  .left-hand {
    top: 0%;
    left: 75%;
    background: var(--penguin-skin, gray);
    width: 30%;
    height: 60%;
    border-radius: 30% 30% 30% 120%;
    transform: rotate(-45deg);
    z-index: -1;
  }

  .right-feet {
    top: 85%;
    left: 60%;
    background: var(--penguin-beak, orange);
    width: 15%;
    height: 30%;
    border-radius: 50% 50% 50% 50%;
    transform: rotate(-80deg);
    z-index: -2222;
  }

  .left-feet {
    top: 85%;
    left: 25%;
    background: var(--penguin-beak, orange);
    width: 15%;
    height: 30%;
    border-radius: 50% 50% 50% 50%;
    transform: rotate(80deg);
    z-index: -2222;
  }

  .right-eye {
    top: 45%;
    left: 60%;
    background: black;
    width: 15%;
    height: 17%;
    border-radius: 50%;
  }

  .left-eye {
    top: 45%;
    left: 25%;
    background: black;
    width: 15%;
    height: 17%;
    border-radius: 50%;
  }

  .sparkle {
    top: 25%;
    left: 15%;
    background: white;
    width: 35%;
    height: 35%;
    border-radius: 50%;
  }

  .blush-right {
    top: 65%;
    left: 15%;
    background: pink;
    width: 15%;
    height: 10%;
    border-radius: 50%;
  }

  .blush-left {
    top: 65%;
    left: 70%;
    background: pink;
    width: 15%;
    height: 10%;
    border-radius: 50%;
  }

  .beak-top {
    top: 60%;
    left: 40%;
    background: var(--penguin-beak, orange);
    width: 20%;
    height: 10%;
    border-radius: 50%;
  }

  .beak-bottom {
    top: 65%;
    left: 42%;
    background: var(--penguin-beak, orange);
    width: 16%;
    height: 10%;
    border-radius: 50%;
  }

  .penguin * {
    position: absolute;
  }
</style>
<div class="penguin">
  <div class="penguin-bottom">
    <div class="right-hand"></div>
    <div class="left-hand"></div>
    <div class="right-feet"></div>
    <div class="left-feet"></div>
  </div>
  <div class="penguin-top">
    <div class="right-cheek"></div>
    <div class="left-cheek"></div>
    <div class="belly"></div>
    <div class="right-eye">
      <div class="sparkle"></div>
    </div>
    <div class="left-eye">
      <div class="sparkle"></div>
    </div>
    <div class="blush-right"></div>
    <div class="blush-left"></div>
    <div class="beak-top"></div>
    <div class="beak-bottom"></div>
  </div>
</div>
```

#### Use a media query to change a variable

CSS Variables can simplify the way you use media queries. For instance, when your screen is smaller or larger than your media query break point, you can change the value of a variable, and it will apply its style wherever it is used.

##### Another penguin example

```html
<style>
  :root {
    --penguin-size: 300px;
    --penguin-skin: gray;
    --penguin-belly: white;
    --penguin-beak: orange;
  }

  @media (max-width: 350px) {
    :root {
      --penguin-size: 200px;
      --penguin-skin: black;
    }
  }

  .penguin {
    position: relative;
    margin: auto;
    display: block;
    margin-top: 5%;
    width: var(--penguin-size, 300px);
    height: var(--penguin-size, 300px);
  }

  .right-cheek {
    top: 15%;
    left: 35%;
    background: var(--penguin-belly, white);
    width: 60%;
    height: 70%;
    border-radius: 70% 70% 60% 60%;
  }

  .left-cheek {
    top: 15%;
    left: 5%;
    background: var(--penguin-belly, white);
    width: 60%;
    height: 70%;
    border-radius: 70% 70% 60% 60%;
  }

  .belly {
    top: 60%;
    left: 2.5%;
    background: var(--penguin-belly, white);
    width: 95%;
    height: 100%;
    border-radius: 120% 120% 100% 100%;
  }

  .penguin-top {
    top: 10%;
    left: 25%;
    background: var(--penguin-skin, gray);
    width: 50%;
    height: 45%;
    border-radius: 70% 70% 60% 60%;
  }

  .penguin-bottom {
    top: 40%;
    left: 23.5%;
    background: var(--penguin-skin, gray);
    width: 53%;
    height: 45%;
    border-radius: 70% 70% 100% 100%;
  }

  .right-hand {
    top: 5%;
    left: 25%;
    background: var(--penguin-skin, black);
    width: 30%;
    height: 60%;
    border-radius: 30% 30% 120% 30%;
    transform: rotate(130deg);
    z-index: -1;
    animation-duration: 3s;
    animation-name: wave;
    animation-iteration-count: infinite;
    transform-origin:0% 0%;
    animation-timing-function: linear;
  }

  @keyframes wave {
      10% {
        transform: rotate(110deg);
      }
      20% {
        transform: rotate(130deg);
      }
      30% {
        transform: rotate(110deg);
      }
      40% {
        transform: rotate(130deg);
      }
    }

  .left-hand {
    top: 0%;
    left: 75%;
    background: var(--penguin-skin, gray);
    width: 30%;
    height: 60%;
    border-radius: 30% 30% 30% 120%;
    transform: rotate(-45deg);
    z-index: -1;
  }

  .right-feet {
    top: 85%;
    left: 60%;
    background: var(--penguin-beak, orange);
    width: 15%;
    height: 30%;
    border-radius: 50% 50% 50% 50%;
    transform: rotate(-80deg);
    z-index: -2222;
  }

  .left-feet {
    top: 85%;
    left: 25%;
    background: var(--penguin-beak, orange);
    width: 15%;
    height: 30%;
    border-radius: 50% 50% 50% 50%;
    transform: rotate(80deg);
    z-index: -2222;
  }

  .right-eye {
    top: 45%;
    left: 60%;
    background: black;
    width: 15%;
    height: 17%;
    border-radius: 50%;
  }

  .left-eye {
    top: 45%;
    left: 25%;
    background: black;
    width: 15%;
    height: 17%;
    border-radius: 50%;
  }

  .sparkle {
    top: 25%;
    left:-23%;
    background: white;
    width: 150%;
    height: 100%;
    border-radius: 50%;
  }

  .blush-right {
    top: 65%;
    left: 15%;
    background: pink;
    width: 15%;
    height: 10%;
    border-radius: 50%;
  }

  .blush-left {
    top: 65%;
    left: 70%;
    background: pink;
    width: 15%;
    height: 10%;
    border-radius: 50%;
  }

  .beak-top {
    top: 60%;
    left: 40%;
    background: var(--penguin-beak, orange);
    width: 20%;
    height: 10%;
    border-radius: 50%;
  }

  .beak-bottom {
    top: 65%;
    left: 42%;
    background: var(--penguin-beak, orange);
    width: 16%;
    height: 10%;
    border-radius: 50%;
  }

  body {
    background:#c6faf1;
  }

  .penguin * {
    position: absolute;
  }
</style>
<div class="penguin">
  <div class="penguin-bottom">
    <div class="right-hand"></div>
    <div class="left-hand"></div>
    <div class="right-feet"></div>
    <div class="left-feet"></div>
  </div>
  <div class="penguin-top">
    <div class="right-cheek"></div>
    <div class="left-cheek"></div>
    <div class="belly"></div>
    <div class="right-eye">
      <div class="sparkle"></div>
    </div>
    <div class="left-eye">
      <div class="sparkle"></div>
    </div>
    <div class="blush-right"></div>
    <div class="blush-left"></div>
    <div class="beak-top"></div>
    <div class="beak-bottom"></div>
  </div>
</div>
```
