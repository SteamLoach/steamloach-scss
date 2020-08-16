# SteamLoach Scss

SteamLoach is a small flex-box library designed to help with scaffolding responsive layouts. It is primarily focused on removing the need to explicitly declare `@media` queries and uses 'scale' mixins to provide a structured way of defining properties across breakpoints.

## Contents

--------------------

* [Quick Start](#quick-start)
* [Variables](#variables)
* [Mixins](#mixins)

## <a id="quick-start"></a> Quick Start

1. `npm install steamloach-scss`
2. `@import 'steamloach-scss/steamloach.scss'`
3. `@include html-base()` mixin `:root` or `html`. This sets defaults for font sizing, font color, lineheight, and page background.

---------------------

## <a id="variables"></a> Variables

All variables are set with `$default!` values and can be overridden by importing a `config.scss` file or similar _before_ the main `steamloach.scss` file.

### Breakpoints & Grid Columns

SteamLoach uses 5 specified breakpoints and a default.

* `$default` - assumed when no other breakpoint applies
* `$lrg-mobile` - from `375px`
* `$phablet` - from `550px`
* `$tablet` - from `768px`
* `$laptop` - from `1024px`
* `$desktop` - from `1264px`

The `$grid-columns` variable is used to define the number of calculated 'width' columns. It has a default of `24`, with one 'column' being one 24th (4.17%) of the total available width.

### Spacing

```scss
//Sub
$space-1: 0.25rem; 
$space-2: 0.5rem; 
$space-3: 0.75rem; 
$space-4: 1rem; 

//Light
$space-5: 1.5rem; 
$space-6: 2rem; 
$space-7: 3rem; 
$space-8: 4rem; 

//Heavy
$space-9: 6rem; 
$space-10: 8rem; 
$space-11: 12rem; 
$space-12: 16rem; 

//Super
$space-13: 24rem; 
$space-14: 32rem; 
$space-15: 40rem; 
$space-16: 48rem; 
```

### Color

SteamLoach uses 7 `hsl` based color prefixes

* `$brand` - primary color
* `$accent` - secondary color
* `$tertiary` - optional third color


* `$success` - green by default 
* `$warning` - yellow by default
* `$danger` - red by default


* `$shade` - desaturated `$brand` by default 

A 7 shade palette is generated for each color, accessed by adding one of the following modifiers to the color prefix.

* `-darkest`
* `-darker`
* `-dark`
* `-base`
* `-light`
* `-lighter`
* `-lightest`

The `$shade` prefix also has `-black` and `-white` modifiers.

Example usage:
```scss
.card {
  color: $brand-base;
  border 1px solid $shade-dark;
}
```

Defaults for all 7 colors can be altered by setting the `$<prefix>-hue` and `$<prefix>-saturation` variables in your `config.scss` file. 

For example: 
```scss
$brand-hue: 199;
$brand-saturation: 77%;
``` 

### Theme

#### Page Background

The `$page-background` variable is used in the `html-base` mixin to set a background for the page. It defaults to `$shade-white` and can be overwritten in your `config.scss` file.

#### Shadows

SteamLoach includes two kinds of default shadow variable, `$shadow` and `$elevation`. `$shadow` variables are generally more direction-oriented, while `$elevation` is used to 'lift' elements in your design.

**`$Shadow`**
```scss
$under-shadow: 0px 6px 20px -6px; 
$centered-shadow: 0px 0px 4px 3px;
$up-shadow: 0px -4px 4px 0px; 
$down-shadow: 0px 4px 4px 0px; 
$inset-shadow: inset 0px 0px 2px; 
```

**`$Elevation`**
```scss
$elevation-lighter: 0px 1px 3px; 
$elevation-light: 0px 4px 6px; 
$elevation-medium: 0px 5px 15px; 
$elevation-heavy: 0px 10px 24px; 
$elevation-heavier: 0px 15px 35px; 
```

#### Borders

```scss
$border: 2px solid #000; 
$border-width: 2px; 
$border-radius: 10px; 
```

#### Transitions

```scss
$transition-function: ease-in-out; 
$transition-duration: 0.3s; 
```

### Typography

SteamLoach sets default font and color variables for `$text`, `$title`, and `$tertiary` prefixes. 

```scss
////Font Families
$text-font: 'Arial', sans-serif; 
$title-font: 'Verdana', sans-serif; 
$tertiary-font: 'Verdana', sans-serif; 
//Color
$text-color: $shade-darker; 
$offset-text-color: $shade-lightest; 
$title-color: $shade-black; 
$offset-title-color: $shade-lightest; 
$tertiary-color: $shade-black; 
$offset-tertiary-color: $shade-lightest; 
```

A type scale is also included:
```scss
//Sub
$text-smallest: 12px;
$text-smaller: 14px;
$text-small: 16px;

//Body
$text-root: 16px;
$text-body: 18px;
$text-large: 20px;
$text-larger: 24px;
$text-largest: 30px;

//Title
$title-smallest: 20px;
$title-smaller: 24px;
$title-small: 30px;
$title-medium: 36px;
$title-large: 48px;
$title-larger: 60px;
$title-largest: 72px;
```

Default line height is defined as follows:
```scss
$line-height-root: 1.5;
```

--------------

## <a id="mixins"></a> Mixins

### Breakpoint Modifiers

Many mixins support the `-from`, `-until`, and `-between` breakpoint modifiers. When using a breakpoint modifier, always pass the `$breakpoint` as the first argument (or the `$start` & `$end` breakpoint if using `-between`).

**For Example**
```scss
button {
  @include font-size-scale(
    $default: $text-smaller
    $on-tablet: $text-body
  ); 
}
```

### `-scale` Functions

Some mixins support the `-scale` modifier. When using the `-scale` modifer, relevant values can be set for any or all breakpoints. All breakpoint arguments except `$default` must be prefixed with `$on-`.

**For Example**
```scss
.card {
  //assuming $grid-columns: 24;
  @include column-scale(
    $default: 22,
    $on-phablet: 20,
    $on-tablet: 18,
    $on-laptop: 12,
  ); 
}
```

### Dimension Mixins

#### Column Width

'Columns' are calculated as a percentage of the total available width. The number of columns is controlled by the `$grid-columns` variable, which defaults to `24` and can be reset in your `config.scss` file.

##### `@include column($column)`

A simple mixin for setting percentage based width.

**Does not support breakpoint modifiers**

**Arguments**

| Name          | Required |  Accepts |
| ------------- |:-------------:| -----    |
| `$column`     | Required | any number between `0` and `$grid-columns` |

**Example**
```scss
.card {
  //assuming $grid-columns: 24;
  @include column(12) //Element will occupy 50% of the available space
}
```

##### `@include column-break($breakpoint, $column)`

Set percentage based width **above** the specified breakpoint.

**Does not support breakpoint modifiers**

**Arguments**

| Name          | Required |  Accepts |
| ------------- |:-------------:| -----    |
| `$breakpoint`     | Required | any valid breakpoint value |
| `$column`     | Required | any number between `0` and `$grid-columns` |

**Example**
```scss
.card {
  //assuming $grid-columns: 24;
  width: 100%
  @include column-break($laptop, 12) //Element will occupy 50% of the available space from the $laptop breakpoint onward
}
```

##### `@include column-scale([...$breakpoints: ...$columns])`

Set percentage based width based on breakpoint(s). All breakpoint arguments except `$default` must be prefixed with `$on-`.

**Does not support breakpoint modifiers**

**Arguments**

| Name          | Required |  Accepts |
| ------------- |:-------------:| -----    |
| `$default`     | Optional | any number between `0` and `$grid-columns` |
| `$on-lrg-mobile`| Optional | any number between `0` and `$grid-columns` |
| `$on-phablet`   | Optional | any number between `0` and `$grid-columns` |
| `$on-tablet`    | Optional | any number between `0` and `$grid-columns` |
| `$on-laptop`    | Optional | any number between `0` and `$grid-columns` |
| `$on-desktop`   | Optional | any number between `0` and `$grid-columns` |

**Example**
```scss
.card {
  //assuming $grid-columns: 24;
  @include column-scale(
    $default: 22,
    $on-phablet: 20,
    $on-tablet: 18,
    $on-laptop: 12,
  ); 
}
```

#### Custom Width

##### `@include custom-break($breakpoint, $width)`

Set any valid width **above** the specified breakpoint.

**Does not support breakpoint modifiers**

**Arguments**

| Name          | Required |  Accepts |
| ------------- |:-------------:| -----    |
| `$breakpoint`     | Required | any valid breakpoint value |
| `$width`     | Required | any valid width |

**Example**
```scss
.card {
  width: 450px;
  @include custom-break($tablet, 650px); 
}
```

##### `@include custom-scale([...$breakpoints: ...$widths])`

Set any valid width based on breakpoint(s). All breakpoint arguments except `$default` must be prefixed with `$on-`.

**Does not support breakpoint modifiers**

**Arguments**

| Name          | Required |  Accepts |
| ------------- |:-------------:| -----    |
| `$default`     | Optional | any valid width |
| `$on-lrg-mobile`| Optional | any valid width |
| `$on-phablet`   | Optional | any valid width |
| `$on-tablet`    | Optional | any valid width |
| `$on-laptop`    | Optional | any valid width |
| `$on-desktop`   | Optional | any valid width |

**Example**
```scss
.card {
  //assuming $grid-columns: 24;
  @include custom-scale(
    $default: 100%,
    $on-phablet: 90%,
    $on-tablet: 650px,
    $on-laptop: 800px,
  ); 
}
```

#### Height

##### `@include height-break($breakpoint, $height)`

Set any valid height **above** the specified breakpoint.

**Does not support breakpoint modifiers**

**Arguments**

| Name          | Required |  Accepts |
| ------------- |:-------------:| -----    |
| `$breakpoint`     | Required | any valid breakpoint value |
| `$height`     | Required | any valid height |

**Example**
```scss
.card {
  height: 90vh;
  @include custom-break($tablet, 75vh); 
}
```

##### `@include height-scale([...$breakpoints: ...$heights])`

Set any valid height based on breakpoint(s). All breakpoint arguments except `$default` must be prefixed with `$on-`.

**Does not support breakpoint modifiers**

**Arguments**

| Name          | Required |  Accepts |
| ------------- |:-------------:| -----    |
| `$default`     | Optional | any valid height |
| `$on-lrg-mobile`| Optional | any valid height |
| `$on-phablet`   | Optional | any valid height |
| `$on-tablet`    | Optional | any valid height |
| `$on-laptop`    | Optional | any valid height |
| `$on-desktop`   | Optional | any valid height |

**Example**
```scss
article {
  //assuming $grid-columns: 24;
  @include height-scale(
    $default: 100vh,
    $on-tablet: 80vh,
    $on-laptop: 650px,
  ); 
}
```

#### XY Sizing

##### `@include size($x, [...$])`

A sizing shorthand mixin. If only `$x` is specified, both width and height be given the same value. Optional `$min` and `$max` arguments can be used to enforce dimensions.

**Accepts breakpoint modifiers**

**Arguments**

| Name          | Required |  Accepts |
| ------------- |:-------------:| -----    |
| `$x`          | Required | any valid size |
| `$y`          | Optional | any valid size |
| `$min`          | Optional | `boolean` |
| `$max`          | Optional | `boolean`  |


**Example**
```scss
.logo-image {
  @include size(75px, $min: true); //element will be 75px by 75px, and will not shrink below that size
}
.svg-icon {
  @include size(12px, $min: true);
  @include size-between($lrg-mobile, $laptop, 16px);
  @include size-from($laptop, 18px);
}
```

### Flex Container Mixins

SteamLoach provides 3 flex-container mixins, all of which default to `flex-direction: row`. Flex-container mixins take shorthand arguments for content justification and alignment, with optional paramaters for item alignment, wrapping, and direction.

**All flex-container mixins support breakpoint modifiers**

##### `@include row($x, $y, [$...])`

Defaults to `100%` width and `relative` position. 

**Arguments**

| Name          | Required |  Accepts |
| ------------- |:-------------:| -----    |
| `$x`          | Required | one of: `center`, `between`, `around`, `start`, `end` |
| `$y`          | Required | one of: `center`, `baseline`, `stretch`, `start`, `end` |
| `$yc`         | Optional (defaults to `stretch`) | one of: `center`, `between`, `around`, `start`, `end`, `stretch` |
| `$no-wrap`    | Optional (defaults to `false`)| `boolean` |
| `$direction`  | Optional (defaults to `row`)| `row` or `column` |

**Example**
```scss
nav {
  @include row(between, center, $no-wrap: true);
}
.panel {
  @include row(center, center);
  @include row-from($tablet, start, center, $no-wrap: true);
}
```

##### `@include container($x, $y, [$...])`

Defaults to `relative` position. 

**Arguments**

| Name          | Required |  Accepts |
| ------------- |:-------------:| -----    |
| `$x`          | Required | one of: `center`, `between`, `around`, `start`, `end` |
| `$y`          | Required | one of: `center`, `baseline`, `stretch`, `start`, `end` |
| `$yc`         | Optional (defaults to `stretch`) | one of: `center`, `between`, `around`, `start`, `end`, `stretch` |
| `$no-wrap`    | Optional (defaults to `false`)| `boolean` |
| `$direction`  | Optional (defaults to `row`)| `row` or `column` |

**Example**
```scss
section {
  @include container(around, center, $direction: column)
}
.card {
  @incude container-between($lrg-mobile, $tablet, start, center)
}
```

##### `@include wrapper($x, $y, [$...])`

**Arguments**

| Name          | Required |  Accepts |
| ------------- |:-------------:| -----    |
| `$x`          | Required | one of: `center`, `between`, `around`, `start`, `end` |
| `$y`          | Required | one of: `center`, `baseline`, `stretch`, `start`, `end` |
| `$yc`         | Optional (defaults to `stretch`) | one of: `center`, `between`, `around`, `start`, `end`, `stretch` |
| `$no-wrap`    | Optional (defaults to `false`)| `boolean` |
| `$direction`  | Optional (defaults to `row`)| `row` or `column` |

**Example**
```scss
div {
  @include wrapper(start, center)
}
.widget {
  @include wrapper-until($laptop, center, start, $direction: column);
  @include wrapper(center, center);
}
```




































