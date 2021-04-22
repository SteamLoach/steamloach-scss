# SteamLoach Scss

SteamLoach is a small flex-box library designed to help with building responsive layouts. It provides incremental variables (for breakpoints, spacing, typography, and colour), and concise mixins for a structured way of defining properties across breakpoints.

Steamloach aims to reduce the need to explicitly declare `@media` queries, while taking some of the guesswork out of spacing, sizing, and colorscheme; like so:

```scss

.card {

  //Create flex-container with justification and alignment
  @include wrapper(center, start);

  //Set column-based width across breakpoints
  @include column-scale(
    $default: 24,
    $on-lrg-mobile: 22,
    $on-tablet: 12,
    $on-laptop: 8,
  );

  padding: $space-4;

  //Set directional margin across breakpoints
  @include margin-scale(
    bottom,
    $default: $space-6,
    $on-tablet: $space-8,
  );

  //set font-size before specific breakpoint
  @include font-size-until($tablet, $text-small);
  font-size: $text-body;

  background: $shade-light;
}

```



<br />

---------------------

## Contents
* [Requirements](#requirements)
* [Quick Start](#quick-start)
* [Variables](#variables)
  * [Breakpoints](#breakpoints)
  * [Grid Columns](#grid-columns)
  * [Dimensions & Spacing](#dimensions-and-spacing)
  * [Typography](#typography)
  * [Color](#color)
  * [Shadow](#shadow)
  * [Borders](#borders)
  * [Transitions](#transitions)
* [Mixins](#mixins)
  * [Core Concepts](#concepts)
    * [From, Until, Between](#from-until-between)
    * [Scale](#scale-mixins)
  * [One-line Media Queries](#media-queries)
  * [Flex Elements](#flex-elements)
  * [Column-based Width](#column-based-width)
  * [Width & Height](#width-and-height)
  * [Max Width & Max Height](#max-width-and-max-height)
  * [XY Sizing](#xy-sizing)
  * [Padding & Margin](#padding-and-margin)
  * [Font Size](#font-size)
  * [HTML Base](#html-base)
  * [Hide Elements](#hide-elements)
  * [Borders](#border-mixin)
  * [Shadows](#shadow-mixin)
  * [Transitions](#transition-mixin)
* [Licence](#licence)


<br />

---------------------
## <a id="requirements"></a> Requirements

An installation of [Sass](https://sass-lang.com/install)

<br />

---------------------

## <a id="quick-start"></a> Quick Start

1. `npm install steamloach-scss`
2. `@import 'steamloach-scss/steamloach.scss'` into your main `scss` file.
3. (Optional) `@import` a config file *before* the main `steamloach.scss` file if you want to override any default variables.
3. (Optional) `@include html-base()` mixin under `:root` or `html`. This sets defaults for font sizing, font color, lineheight, and page background.

<br/>

---------------------

## <a id="variables"></a> Variables

All variables are set with `$default!` values and can be overridden by importing a config file _before_ the main `steamloach.scss` file.

<br />

### <a id="breakpoints"></a> Breakpoints

SteamLoach defines 5 breakpoints and a default.

* `$default` - assumed when no other breakpoint applies
* `$lrg-mobile` - from `375px`
* `$phablet` - from `550px`
* `$tablet` - from `768px`
* `$laptop` - from `1024px`
* `$desktop` - from `1264px`

```scss
.cta-button {
  @include x-pad-from($laptop, $space-4);
}
```

> These breakpoints are used under the hood by SteamLoach’s -scale mixins. You can override the default breakpoint values in your config file, but be aware that doing so will affect all `-scale` mixins.

<br/>

### <a id="grid-columns"></a> Grid Columns

The `$grid-columns` variable is used to define the number of calculated 'width' columns used in column-based width mixins. It has a default of `24`, with one 'column' being one 24th (4.17%) of width of the parent element.

```scss
.column {
  @include column-from($tablet, 12);
}

.card {
  @include column-scale(
    $default: 22,
    $on-tablet: 12,
    $on-desktop: 8,
  );
}
```

> `$grid-columns` can be overriden in your config file. Be aware that doing so will affecct all column-based mixins.

<br />

### <a id="dimensions-and-spacing"></a> Dimensions & Spacing



Steamloach uses a 16 point `rem` based spacing scale, accessed using the variables `$space-1` through `$space-16`.

```scss
.section-title {
  margin-bottom: $space-6;
}
.ui-button {
  padding: $space-2 $space-4;
}
```
> Spacing variables are `rem` based, so any change to your root `font-size` will affect all spacing variables.

<br />

Steamloach also provides incremental `px` based 'width' variables, intended for use when setting `max-width` on elements that require it.

```scss
$extra-narrow-width: 400px;
$narrow-width: 550px;
$medium-width: 768px;
$wide-width: 1024px;
$wider-width: 1248px;
$extra-wide-width: 1420px;
$super-wide-width: 1600px;
```

```scss
.content-panel {
  max-width: $extra-wide-width;
}
.body-copy {
  max-width: $narrow-width;
}
```

<br />

### <a id="typography"></a> Typography

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

An incremental `font-size` scale can be utilised by adding one of the following modifiers to the `$text` or `$title` prefix:
* `-smallest`
* `-smaller`
* `-small`
* `-medium`
* `-large`
* `-larger`
* `-largest`

By default there is some overlap between the largest `$text` and smallest `$title` sizes.

```scss
.card-title {
  font-size: $title-small;
  @include font-size-from($laptop, $title-medium);
}
.ui-button {
  font-size: $text-large;
}
```

Default line height is defined as follows:
```scss
$line-height-root: 1.5;
```
> You can override any of the default typography variables in your config file.

<br />

### <a id="color"></a> Color

SteamLoach uses 7 `hsl` based color prefixes

* `$brand` - primary color
* `$secondary` - secondary color
* `$accent` - accent color


* `$success` - green by default
* `$warning` - yellow by default
* `$danger` - red by default


* `$shade` - desaturated `$brand` by default

A tint palette is generated for each color, accessed by adding one of the following modifiers to the color prefix.

* `-black`
* `-darkest`
* `-darker`
* `-dark`
* `-base`
* `-light`
* `-lighter`
* `-lightest`
* `-white`

```scss
.card {
  color: $shade-lightest;
  background: $brand-base;
  border 1px solid $shade-dark;
}
.ui-button {
  background: $accent-light;
}
```

Base colors can be altered by defining `$<prefix>-hue` and/or `$<prefix>-saturation` variables in your `config.scss` file. For example:
```scss
$brand-hue: 199;
$brand-saturation: 77%;
```

Tint variables (`-dark`, `-darker`, etc) are generated from base colors and **should not** be overridden.

The `$page-background` variable is used in the `html-base` mixin to set a background for the page. It defaults to `$shade-white` and can be overwritten in your `config.scss` file.

<br />

### <a id="shadow"></a> Shadow

The `$shadow-shade` variable defines the default shadow color in Steamloach's `@include shadow()` mixin. It defaults to `$shade-dark` and can be overriden in your config file.

SteamLoach includes two kinds of predefined shadow variables, `-$shadow` and `$elevation-`. `-$shadow` variables are generally more direction-oriented, while `$elevation-` is used to 'lift' elements in your design.



**Shadow**
```scss
$under-shadow: 0px 6px 20px -6px;
$centered-shadow: 0px 0px 4px 3px;
$up-shadow: 0px -4px 4px 0px;
$down-shadow: 0px 4px 4px 0px;
$inset-shadow: inset 0px 0px 2px;
```

```scss
.card {
  @include shadow($under-shadow, $shade-darkest);
}
```

**Elevation**
```scss
$elevation-lightest: 0px 1px 3px;
$elevation-lighter: 0px 1px 3px;
$elevation-light: 0px 4px 6px;
$elevation-medium: 0px 5px 15px;
$elevation-heavy: 0px 10px 24px;
$elevation-heavier: 0px 15px 35px;
$elevation-heavist: 0px 20px 40px;
```
```scss
.card {
  @include shadow($elevation-light, $shade-darker);
}
```

<br />

### <a id="borders"></a> Borders

Border variables are used as defaults by Steamloach's `@include border()` mixins. They can be overriden in your config file as needed.

```scss
$border: 2px solid #000;
$border-width: 2px;
$border-radius: 10px;
```

<br />

### <a id="transitions"></a> Transitions

Transition variables are used as defaults by Steamloach's `@include transition()` shorthand mixin. They can be overriden in your config file as needed.

```scss
$transition-function: ease-in-out;
$transition-duration: 0.3s;
```
<br />

--------------

## <a id="mixins"></a> Mixins

Steamloach's mixins aim to provide a concise, semantic way to define values across breakpoints without having to explicitly declare `@media` queries.


### <a id="concepts"></a> Core Concepts

#### <a id="from-until-between"></a> From, Until, Between

**Most** mixins support the `-from`, `-until`, and `-between` modifiers, which allow you to specify a breakpoint (or breakpoints) _from_, _until_, or _between_ which the mixin will apply.

When using `-from` or `-until`, always pass a breakpoint (variable or otherwise) as the first argument.

```scss
.handheld-nav {
  @include hidden-from($laptop);
}
.nav-link {
  @include font-size-until($tablet, $text-small);
}
.card {
  @include x-pad-from(1000px, $space-4);
}
```

When using `-between`, pass the start and end breakpoint as the first and second argument respectively.

```scss
.card {
  @include y-margin-between($tablet, $laptop, $space-3);
}
```

#### <a id="scale-mixins"></a> Scale

**Some** mixins support the `-scale` modifier. `-scale` allows you to declare values across any or all [defined breakpoints](#breakpoints) in a single statement. When using `-scale` all breakpoint definitions except `$default` must be prefixed with `$on-`.

```scss
.ui-button {
  @include font-size-scale(
    $default: $text-small,
    $on-tablet: $text-medium,
    $on-desktop: $text-large,
  );
}
.card {
  @include margin-scale(
    bottom,
    $on-tablet: $space-4,
    $on-laptop: $space-8,
  );
}
```

<br />

### <a id="media-queries"></a> One-line Media Queries

`media` mixins provide a convenient way to declare breakpoint sensitive values for a single attribute.

**`@include media-<from/until/between>(...)`**

| Arguments          | Required |  Accepts |
| ------------- |-------------| -----    |
| `$breakpoint`     | required with `-from` and `-until` | any valid breakpoint |
| `$start` | required with `-between` | any valid breakpoint |
| `$end` | required with `-between` | any valid breakpoint |
| `$attribute` | required | any valid css attribute |
| `$val` | required | any valid value for the chosen attribute |

```scss
.handheld-nav {
  @include media-from($laptop, display, none);
}
.card {
  @include media-from($laptop, width, 100%)
  @include media-until($tablet, margin-bottom, $space-4);
}
.callout {
  @include media-between(600px, $tablet, padding, $space-2)
}
```

<br />


### <a id="flex-elements"></a> Flex Elements

Steamloach provides three flex-element mixins: `row`, `container`, and `wrapper`. These mixins take shorthand arguments for justification and item alignment, with optional paramaters for content aligmnent, wrapping, and flex-direction. Some things to note:
* All flex-containers default to `flex-direction: row;` and `flex-wrap: wrap;`
* Both `row` and `container` default to `position: relative;`
* `row` defaults to `width: 100%;`

**`@include row(...)`**
**`@include row-<from/until/between>(...)`**

**`@include container(...)`**
**`@include container-<from/until/between>(...)`**

**`@include wrapper(...)`**
**`@include wrapper-<from/until/between>(...)`**

| Arguments          | Required |  Accepts |
| ------------- |-------------| -----    |
| `$breakpoint`     | required with `-from` and `-until` | any valid breakpoint |
| `$start` | required with `-between` | any valid breakpoint |
| `$end` | required with `-between` | any valid breakpoint |
| `$x (justify-content)` | required | one of: `center`, `start`, `end`, `between`, `around` |
| `$y (align-items)` | required | one of `center`, `baseline`, `start`,`end`, `stretch` |
| `$yc(align-content)` | optional (defaults to `stretch`) | one of: `center`, `start`, `end`, `between`, `around`, `stretch` |
| `$no-wrap` | optional (defaults to `false`) | boolean |
| `$direction` | optional (defaults to `row`) | `row` or `column` |

```scss
.widget {
  @include wrapper(center, center);
}

.card-panel {
  @include container(around, center, $yc: between);
}

.media-card {
  @include wrapper-until(
    $laptop,
    between,
    center,
    $direction: column
  );
}

.top-nav {
  @include row(start, center, $no-wrap: true);
  @include row-from($laptop, between, center);
}
```

<br />

### <a id="column-based-width"></a> Column-based Width

`column` mixins provide a concise way of defining column-based width across breakpoints. 'Columns' are calculated as a percentage of the total available width of the parent element. The number of available columns is defined by the `$grid-columns` variable which defaults to `24` and can be overriden in your config file.

##### From, Until, Between

**`@include column(...)`**
**`@include column-<from/until/between>(...)`**

| Arguments          | Required |  Accepts |
| ------------- |-------------| -----    |
| `$breakpoint`     | required with `-from` and `-until` | any valid breakpoint |
| `$start` | required with `-between` | any valid breakpoint |
| `$end` | required with `-between` | any valid breakpoint |
| `$columns` | required | any integer between `1` and `$grid-columns` (`24` by default)  |

```scss
.card {
  @include column-from($laptop, 8);
}
.panel {
  @include column-until($tablet, 24);
  @include column(20);
}
```

##### Scale

Use `column-scale` to set column-based width across any or all [defined breakpoints](#breakpoints) in a single statement.

**`@include column-scale(...)`**

| Arguments          | Required |  Accepts |
| ------------- |-------------| -----    |
| `$default`     | optional | any integer between `1` and `$grid-columns` (`24` by default) |
| `$on-lrg-mobile`     | optional | any integer between `1` and `$grid-columns` (`24` by default) |
| `$on-phablet`     | optional | any integer between `1` and `$grid-columns` (`24` by default) |
| `$on-tablet`     | optional | any integer between `1` and `$grid-columns` (`24` by default) |
| `$on-laptop`     | optional | any integer between `1` and `$grid-columns` (`24` by default) |
| `$on-desktop`     | optional | any integer between `1` and `$grid-columns` (`24` by default) |

```scss
.content-block {
  @include column-scale(
    $default: 24,
    $on-phablet: 22,
    $on-tablet: 20,
    $on-laptop: 16,
  );
}
.panel {
  @include column-scale(
    $default: 22,
    $on-desktop: 18,
  );
}
```

<br />

### <a id="width-and-height"></a> Width & Height

`width` and `height` mixins can be used to set dimensions across breakpoints. The optional `$max` argument can be used to constrain dimensions as needed.

##### From, Until, Between

**`@include width-<from/until/between>(...)`**
**`@include height-<from/until/between>(...)`**

| Arguments          | Required |  Accepts |
| ------------- |-------------| -----    |
| `$breakpoint`     | required with `-from` and `-until` | any valid breakpoint |
| `$start` | required with `-between` | any valid breakpoint |
| `$end` | required with `-between` | any valid breakpoint |
| `$length` | required | any valid length |
| `$max` | optional (defaults to `false`) | boolean |

```scss
.modal-inner {
  width: 350px;
  @include width-from($tablet, 475px);
}
img {
  @include height-until($laptop, 325px, $max:true);
}
```

##### Scale

`width` and `height` scales can be used to set sizing across any or all breakpoints in a single statement.

**`@include width-scale(...)`**
**`@include height-scale(...)`**

| Arguments          | Required |  Accepts |
| ------------- |-------------| -----    |
| `$default`     | optional | any valid length |
| `$on-lrg-mobile`     | optional | any valid length |
| `$on-phablet`     | optional | any valid length |
| `$on-tablet`     | optional | any valid length |
| `$on-laptop`     | optional | any valid length |
| `$on-desktop`     | optional | any valid length |
| `$max` | optional (defaults to `false`) | boolean |

```scss
.hero {
  @include height-scale(
    $default: 90vh,
    $on-tablet: 75vh,
    $on-laptop: 650px,
  );
}
aside {
  @include width-scale(
    $on-tablet: 200px,
    $on-laptop: 350px,
  );
}
```

<br />

### <a id="max-width-and-max-height"></a>Max Width & Max Height

`max-width` and `max-height` mixins can be used to constrain dimensions across breakpoints.

##### From, Until, Between

**`@include max-width-<from/until/between>(...)`**
**`@include max-height-<from/until/between>(...)`**

| Arguments          | Required |  Accepts |
| ------------- |-------------| -----    |
| `$breakpoint`     | required with `-from` and `-until` | any valid breakpoint |
| `$start` | required with `-between` | any valid breakpoint |
| `$end` | required with `-between` | any valid breakpoint |
| `$length` | required | any valid length |

```scss
.card {
  @include max-width-until($tablet, 350px);
}
.hero-image {
  @include max-height-from($laptop, 650px);
}
```

##### Scale

`max-width` and `max-height` scales can be used to constrain sizing across any or all breakpoints in a single statement.

**`@include max-width-scale(...)`**
**`@include max-height-scale(...)`**

| Arguments          | Required |  Accepts |
| ------------- |-------------| -----    |
| `$default`     | optional | any valid length |
| `$on-lrg-mobile`     | optional | any valid length |
| `$on-phablet`     | optional | any valid length |
| `$on-tablet`     | optional | any valid length |
| `$on-laptop`     | optional | any valid length |
| `$on-desktop`     | optional | any valid length |

```scss
.hero {
  @include max-height-scale(
    $default: 90vh,
    $on-tablet: 75vh,
    $on-laptop: 650px,
  );
}
aside {
  @include max-width-scale(
    $on-tablet: 200px,
    $on-laptop: 350px,
  );
}
```
<br />

### <a id="xy-sizing"></a> XY Sizing

The `size` mixin provides a concise way to set the width and height of an element. If only `$x` (width) is specified, both width and height are given the same value. `$min` and `$max` can be used to constrain dimensions as needed.

**`@include size(...)`**
**`@include size-<from/until/between>(...)`**

| Arguments          | Required |  Accepts |
| ------------- |-------------| -----    |
| `$breakpoint`     | required with `-from` and `-until` | any valid breakpoint |
| `$start` | required with `-between` | any valid breakpoint |
| `$end` | required with `-between` | any valid breakpoint |
| `$x` (width) | required | any valid length |
| `$y` (height) | optional (defaults to `$x`) | any valid length |
| `$min` | optional (defaults to `false`) | boolean |
| `$max` | optional (defaults to `false`) | boolean |

```scss
.logo-image {
  @include size(75px, 100px, $min: true);
}
.svg-icon {
  @include size(12px, $min: true);
  @include size-between($lrg-mobile, $laptop, 16px);
  @include size-from($laptop, 20px);
}
```

<br />

### <a id="padding-and-margin"></a> Padding & Margin

`pad` and `margin` mixins provide concise shorthand for setting padding and margin across breakpoints.

##### From, Until, Between

**`@include pad-<from/until/between>(...)`**
**`@include margin-<from/until/between>(...)`**

| Arguments          | Required |  Accepts |
| ------------- |-------------| -----    |
| `$breakpoint`     | required with `-from` and `-until` | any valid breakpoint |
| `$start` | required with `-between` | any valid breakpoint |
| `$end` | required with `-between` | any valid breakpoint |
| `$xy` | optional | any valid length |
| `$x`| optional | any valid length |
| `$y`| optional | any valid length |
| `$top`| optional | any valid length |
| `$right`| optional | any valid length |
| `$bottom`| optional | any valid length |
| `$left`| optional | any valid length |

```scss
.ui-button {
  @include pad-until(
    $laptop,
    $x: $space-2,
    $y: $space-3,
  );
}
.card {
  @include margin-from($tablet, $bottom: $space-6);
}
```


##### Scale

`pad-scale` and `margin-scale` can be used to set padding and margin across any or all breakpoints.

| Arguments          | Required |  Accepts |
| ------------- |-------------| -----    |
| `$dir`     | required | one of: `x`, `y`, `xy` `top`, `right`, `bottom`, `left` |
| `$default`     | optional | any valid length |
| `$on-lrg-mobile`     | optional | any valid length |
| `$on-phablet`     | optional | any valid length |
| `$on-tablet`     | optional | any valid length |
| `$on-laptop`     | optional | any valid length |
| `$on-desktop`     | optional | any valid length |

```scss
.content-block {
  @include pad-scale(
    xy,
    $default: $space-4,
    $on-tablet: $space-6,
    $on-laptop: $space-10,
    $on-desktop $space-12,
  );
}
.hero {
  @include margin-scale(
    bottom,
    $default: $space-6,
    $on-laptop: $space-10,
  );
}
```

##### XY

Add an `x-` `y-` or `xy` prefix to set horizontal, vertical, or all values in a single statement.

**`@include x-pad(...)`**
**`@include x-pad-<from/until/between>(...)`**
**`@include y-pad(...)`**
**`@include y-pad-<from/until/between>(...)`**
**`@include xy-pad(...)`**
**`@include xy-pad-<from/until/between>(...)`**

**`@include x-margin(...)`**
**`@include x-margin-<from/until/between>(...)`**
**`@include y-margin(...)`**
**`@include y-margin-<from/until/between>(...)`**
**`@include xy-margin(...)`**
**`@include xy-margin-<from/until/between>(...)`**

| Arguments          | Required |  Accepts |
| ------------- |-------------| -----    |
| `$breakpoint`     | required with `-from` and `-until` | any valid breakpoint |
| `$start` | required with `-between` | any valid breakpoint |
| `$end` | required with `-between` | any valid breakpoint |
| `$val` | required | any valid length |

```scss
.card {
  @include x-pad($space-4);
  @include x-pad-from($laptop, $space-6);
  @include y-pad($space-4);
}
```


##### Single direction

Use the `single-` prefix to set a single value.

**`@include single-pad(...)`**
**`@include single-pad-<from/until/between>(...)`**

**`@include single-margin(...)`**
**`@include single-margin-<from/until/between>(...)`**

| Arguments          | Required |  Accepts |
| ------------- |-------------| -----    |
| `$breakpoint`     | required with `-from` and `-until` | any valid breakpoint |
| `$start` | required with `-between` | any valid breakpoint |
| `$end` | required with `-between` | any valid breakpoint |
| `$dir` | required | `top`, `right`, `bottom`, `left` |
| `$val` | required | any valid length |

```scss
.card {
  margin-bottom: $space-6;
  @include single-margin-from($laptop, bottom, $space-8);
}
```

<br />

### <a id="font-size"></a> Font Size

`font-size` mixins provide an easy way to set font size across breakpoints.

##### From, Until, Between

**`@include font-size-<from/until/between>(...)`**

| Arguments          | Required |  Accepts |
| ------------- |-------------| -----    |
| `$breakpoint`     | required with `-from` and `-until` | any valid breakpoint |
| `$start` | required with `-between` | any valid breakpoint |
| `$end` | required with `-between` | any valid breakpoint |
| `$size` | required | any vaild `font-size` |

```scss
.section-title {
  font-size: $title-medium;
  @include font-size-from($laptop, $title-large);
}
.ui-button {
  @include font-size-until($tablet, $text-small);
}
```

##### Scale

Use `font-size-scale` to set font size across any or all breakpoints in a single statement.

| Arguments          | Required |  Accepts |
| ------------- |-------------| -----    |
| `$default`     | optional | any valid `font-size` |
| `$on-lrg-mobile`     | optional | any valid `font-size` |
| `$on-phablet`     | optional | any valid `font-size` |
| `$on-tablet`     | optional | any valid `font-size` |
| `$on-laptop`     | optional | any valid `font-size` |
| `$on-desktop`     | optional | any valid `font-size` |

```scss
.nav-link {
  @include font-size-scale(
    $default: $text-smaller,
    $on-tablet: $text-medium,
    $on-laptop: $text-large,
  );
}

```

<br />

### <a id="html-base"></a> HTML Base

The `html-base` mixin can be used to set font family, size, lineheight, and color according to Steamloach's [typography variables](#typography). It should be used under `:root` or `html` to take effect across your application.

```scss

html {
  @include html-base();
}

```

Under the hood, the mixin sets the following values:
```scss

  font-family: $text-font;
  font-size: $text-root;
  line-height: $line-height-root;
  color: $text-color;

  //Page Background
  background-color: $page-background;

  //header sizing, color, lineheight
  h1, h2, h3,
  h4, h5, h6 {
    font-family: $title-font;
    color: $title-color;
  }

  h1, h2 {line-height: 1.0;}
  h3, h4 {line-height: 1.2;}
  h5, h6 {line-height: 1.3;}

```

<br />

### <a id="hide-elements"> </a> Hide Elements

The `hidden` mixin provides a consise way to hide elements across breakpoints.

**`hidden-<from/until/between>(...)`**

| Arguments          | Required |  Accepts |
| ------------- |-------------| -----    |
| `$breakpoint`     | required with `-from` and `-until` | any valid breakpoint |
| `$start` | required with `-between` | any valid breakpoint |
| `$end` | required with `-between` | any valid breakpoint |

```scss
.handheld-nav {
  @include hidden-from($laptop);
}
```
<br />

### <a id="border-mixin"></a> Borders

`border` mixins provide a concise way to set borders across breakpoints.

**`border(...)`**

| Arguments          | Required |  Accepts |
| ------------- |-------------| -----    |
| `$style` | optional (defaults to [`$border`](#borders) variable) | any valid border definition |
| `$radius` | optional (defaults to [`$border-radius`](#borders) variable) | any valid radius |

```scss
.card {
  @include border();
}
.ui-button {
  @include border(1px solid $accent-base, 12px);
}
```

**`border-<from/until/between>(...)`**

| Arguments          | Required |  Accepts |
| ------------- |-------------| -----    |
| `$breakpoint`     | required with `-from` and `-until` | any valid breakpoint |
| `$start` | required with `-between` | any valid breakpoint |
| `$end` | required with `-between` | any valid breakpoint |
| `$dir` | optional (border will be applied to all sides if not given) | any one of `x`, `y`, `top`, `right`, `bottom`, `left` |
| `$style` | optional (defaults to [`$border`](#borders) variable) | any valid border definition |
| `$radius` | optional (defaults to [`$border-radius`](#borders) variable) | any valid radius |

```scss
.column {
  @include border-from(
    $laptop,
    right,
    1px solid $shade-darker,
  );
}
```

<br />

### <a id="shadow-mixin"></a> Shadow

`shadow` mixins provide a concise way to set shadows across breakpoints.

**`shadow(...)`**

| Arguments          | Required |  Accepts |
| ------------- |-------------| -----    |
| `$style` | required | any valid shadow definition |
| `$shade` | optional (defaults to [`$shadow-shade`](#shadow) variable) | any valid color |

```scss
.card {
  @include shadow($elevation-medium);
}
.ui-button {
  @include shadow($elevation-lighter, $shade-base);
}
```

**`shadow-<from/until/between>(...)`**

| Arguments          | Required |  Accepts |
| ------------- |-------------| -----    |
| `$breakpoint`     | required with `-from` and `-until` | any valid breakpoint |
| `$start` | required with `-between` | any valid breakpoint |
| `$end` | required with `-between` | any valid breakpoint |
| `$style` |required | any valid shadow definition |
| `$shade` | optional (defaults to [`$shadow-shade`](#shadow) variable) | any valid color |

```scss
.card {
  @include shadow-from(
    $tablet,
    $elevation-light,
    $shade-base,
  );
}
```
<br />

### <a id="transition-mixin"></a> Transition

The `transition` mixin provide a concise way to apply transitions to an element.

**`transition(...)`**

| Arguments          | Required |  Accepts |
| ------------- |-------------| -----    |
| `$props` | optional (defaults to `all`) | any valid transition property |
| `$duration` | optional (defaults to [`$transition-duration`](#transitions) variable) | any valid duration |
| `$function` | optional (defaults to [`$transition-function`](#transitions) variable) | any valid transition function |

```scss
.ui-button {
  @include transition($duration: 0.6s);
}
```

<br />

--------------------
## <a id="licence"></a> Licence

Steamloach is copyright © 2021 Tom Armstrong. It is free software, and may be redistributed under the following terms.

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the “Software”), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED “AS IS”, WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.





























