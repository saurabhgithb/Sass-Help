# SASS HELP

## Installation

1. **Add Extension:** [Live Sass Compiler](https://marketplace.visualstudio.com/items?itemName=glenn2223.live-sass) by Glen Marks
2. **Change Settings:** In settings.json add:

```js
"liveSassCompile.settings.formats": [
        {
            "format": "compressed",
            "extensionName": ".css",
            "savePath": "/dist",
            "savePathReplacementPairs": null
        }
    ],
```

## Folder Structure

- The **app/scss/style.scss** file serves as the entry point to our Sass code. However we can name it anything but the common convention is to name it main.scss.

- The **dist/style.css** is the compiled css file that we've linked in our **index.html**.

- Inside **app/scss**, we have subfolders for `globals`, `layout` and `util`. Each of these folder contain _sass partials_ that are related to specific aspect of project's design.

```csharp
├── app
│   ├── scss
│   │   ├── globals
│   │   │   ├── _boilerplate.scss
│   │   │   ├── _colors.scss
│   │   │   └── _index.scss
│   │   ├── layout
│   │   │   ├── _grid.scss
│   │   │   └── _index.scss
│   │   ├── util
│   │   │   ├── _font.scss
│   │   │   └── _index.scss
│   └── style.scss
├── dist
│   ├── style.css
│   └── style.css.map
└── index.html

```

## Sass Partials

- **Create**: Prefix the filename with an underscore to create sass partials like `_filename.scss`.
- **Load**: Use `@forward` to import sass modules.

_app/scss/globals/\_index.scss_

```css
@forward "boilerplate";
@forward "typography";
@forward "colors";
```

> **Tip**💡: Keep one index file in each sass subfolder to keep your codebase organised.

## Sass Variables

- **Create**: Use `$` symbol followed by name of variable like `$font`.

_app/scss/util/\_fonts.scss_

```css
$font: "Poppins", sans-serif;
```

_app/scss/util/\_index.scss_

```css
@forward "fonts.scss";
```

- **Load**:
  1. Import using `@use`.
  2. Sass variables, however, must be referenced through a **namespace**, which by default is `filename`.
  3. for using different namespace, specify it using `as` keyword.

_app/scss/globals/\_boilerplate.scss_

```css
@use "../util";
/* @use '../util' as u; */

body {
  font-family: util.$font;
  /* font-family: u.$font; */
}
```

## Sass Mixins

like function return a value they return a set of CSS declarations.

- **Create**:

_app/scss/util/\_breakpoints.scss_

```css
@mixin breakpoint($size) {
  @media (min-width: map-get($breakpoint-up, $size)) {
    @content;
  }
}
```

- **Load and Use**:

_app/scss/layout/\_grid.scss_

```css
@use "../util" as u;

@include u.breakpoint(large) {
  grid-template-columns: 2fr 1fr;
  grid-template-rows: auto;
}
```

## Sass Functions

Returns a value after doing operations.

_app/scss/util/\_function.scss_

```css
@use "sass:math";
/* sass:math <-- Built-in Module of sass */

@function rem($pixel) {
  @if math.is-unitless($pixel) {
    @return math.div($pixel, 16) + rem;
  } @else {
    @error 'Pass #{$pixel} in number only, without units.';
  }
  /* #{$pixel} <-- this is interpolation. */
}
```

## Sass Extend and Placeholders

Basically copy pasting from one place to another

- **Create placeholder**:

```css
%widget {
  padding: u.rem(16);
}
```

- **Extend styles**:

```css
&__widget {
  /* $widget: &; */
  padding: u.rem(16);
  &.magenta {
    @extend %widget;
    /* extending using placeholder */
    background-color: rgb(155, 47, 155);
  }

  &.green {
    /* @extend .grid__widget; <-- extending using class name*/
    /* @extend #{$widget}; <-- extending using interpolation*/
    @extend %widget;
    /* extending using placeholder */
    background-color: rgb(13, 128, 70);
  }
}
```
