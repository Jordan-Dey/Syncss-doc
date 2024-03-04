# Create

Syncss is designed to allow you to extend configurations and create new helper or component. When you extend your framwork try to folloew Syncss logical by grouping your customes values in configuration files. This approach will help you to build a consistent interface and reusable modules.

## Nomage methodology

// TODO JORDAN

## How made a configuration ?

With SASS you can create simple variables to centralyze your informations.

```scss
$myText: "a string value";
$myNumber: 5;
$myValue: 5px; // Work with 5em, 5rem, 5vw, 5%, ...
$myColor: pink; // And work for all propertie values.
$myList: 1, 2, 3, 4, 5; // Work with all types.
$myObject: ("name": "Jordan", "twitter": "@DeyJordan");
```

So for complexe configuration, it can be represented by a large object:

```scss
// this represents the base size of the heading allowed by the design system.
$headingBaseSize: 1.2rem;

// this represents how heading need to be construct in CSS.
$headingConfig: (
  (
    // level 1
    "line-height": 1,
    "font-size": $headingBaseSize * 3,
    "tablet": (
      "font-size": $headingBaseSize * 4,
    )
  ),
  (
    // level 2
    "line-height": 1,
    "font-size": $headingBaseSize * 2,
    "tablet": (
      "font-size": $headingBaseSize * 2.5,
    ),
    "desktop": (
      "font-size": $headingBaseSize * 3,
    )
  ),
  (
    // level 3
    "line-height": 1,
    "font-size": $headingBaseSize * 2,
    "tablet": (
      "font-size": $headingBaseSize * 2.5,
    )
  ),
  // ...
);
```

When you create a variable you can access it by differing way, like explained below.

### Loading the configuration.

If you whant read your config variables in another file your can use the SASS `@use` syntax.
By this way you can now access to variables contains in your file by typing `nameOfYourConfig.$yourVariableName`.

```scss
// load colors.scss file to access variables.
@use "../config/colors.scss";

.background-primary {
    // use the variable $primary.
    background-color: colors.$primary;
}
```

you can also create a alias for your file:

```scss
// load colors.scss file to access variables.
@use "../config/colors.scss";

.background-primary {
    // use the variable $primary.
    background-color: colors.$primary;
}
```

### Accessing complex object configuration.

Some time you need to access at a part of a complexes configuration, by exemple when you create a helper. In this case you can make a mixin to help you.

```scss
// Will get the configuration for a heading level
// From the collection variable.
// (See `$headingConfig` in "How made a configuration ?")
@function getHeadingConfig($level) {
  @return nth($headingConfig, $level);
}
```

Mixin can be access when you whant like variables.

```scss
// load text.scss file to access mixin.
@use "../config/text.scss";

// Get only the first level configuration
$headingFirstLevelConfig: text.getHeadingConfig(1);
// Get only the second level configuration
$headingSecondLevelConfig: text.getHeadingConfig(2);
```

Mixin can also be used to group informations, and keep flexibility to overry configuration in some case.

```scss
// config/borders.scss

// This values will help you to construct borders with "default" style.
// (This values can also be read in your project)
$defaultSize: 1.5px;
$defaultType: solid;
$defaultColor: colors.$grey;
$defaultRadius: 0;
// ...

// This mixin will create border with parameters or default values.
@mixin getStyles($size: null, $type: null, $color: null, $radius: null) {
  $computedSize: $size or $defaultSize;
  $computedType: $type or $defaultType;
  $computedColor: $color or $defaultColor;
  $computedRadius: $radius or $defaultRadius;

  border: $computedSize $computedType $computedColor;
  border-radius: $defaultRadius;
}
```

So you can use this mixin to create border quickly.

```scss
@use "../config/borders.scss";

.defaultBorder {
    @include borders.getStyles();
}

.redBorder {
    @include borders.getStyles($color: red);
}

.blueCircleBorder {
    @include borders.getStyles($color: blue, $radius: 50%);
}

.largeBorder {
    // You can also set properties readed in config.
    @include borders.getStyles($size: borders.$largeSize);
}
```

## How use a helper ?

A helper is a tool box to provide you simple action everywhere in your code.
He will produce classe name you can reuse in html or SCSS files.

```scss
// helper/text.scss

.text-primary {
  color: colors.$primary;
}
```

### In HTML

If you want to access this helper everywhere in your HTML code you need to add your helper to `_index.scss` file.

```scss
// styles/_index.scss

// Build helper and componants here.
// (order is important)
@use "./utilities/base.scss";

@use "./helper/accessibility.scss";
@use "./helper/position.scss";
@use "./helper/spaces.scss";
@use "./helper/text.scss"; // .text-primary is here !
@use "./helper/form.scss";
// you can add your helper here

@use "./componant/Btn.scss";
@use "./componant/Layout.scss";
```

You can now use your token in html classe.

```html
<span class="text-primary">Your text with primary color</span>
```

### In SCSS

Like for a configuration file, if you whant use a helper in SCSS you need to load it.
If they have a namespace conflict with another file you can set an alias.

```scss
@use "../config/text" as textConfig;
@use "../helper/text" as textHelper;

.Componant {
  // ...
}

.Componant--light {
    @extend .Componant;
    @extend .text-dark;
}

.Componant--dark {
  @extend .Componant;
  @extend .text-light;
  background-color: textConfig.$color-dark;
}
```

