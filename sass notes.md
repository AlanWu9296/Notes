# Sass Convert

1. sass <source_file.format>:<target_file.format>
2. --watch (will automatically update the target file when the source file saved, cir-C to cancel , also can watch whole directory)
3. --style
   - compact 
   - compressed
   - expanded

# Variables

1. $<name> : <value>;
2. `#{$<name>}` call back the element name of scss into css as a string

# Inheritance

1. Inheritated Selector
   - `&` to select the mother element
2. Inheritated Properties
   - `%<name>{}` to create a place holder of the properties
   - `@extend %<name>` to inheritate the values in the place holder

```scss
.panel{
  background-color: red;
  height: 70px;
  border: 2px solid green;
}

.big-panel{
  @extend .panel;
  width: 150px;
  font-size: 2em;
}
```



# Partial & Import

`Partials` in Sass are separate files  that hold segments of CSS code. These are imported and used in other  Sass files. This is a great way to group similar code into a module to  keep it organized.

Names for `partials` start with the underscore (`_`) character, which tells Sass it is a small segment of CSS and not to convert it into a CSS file. Also, Sass files end with the `.scss` file extension. To bring the code in the `partial` into another Sass file, use the `@import` directive.

```scss
//if all your mixins are saved in a partial named "_mixins.scss", and they are needed in the "main.scss" file
// In the main.scss file
// Note that the underscore is not needed in the import statement - Sass understands it is a partial
@import 'mixins'
```



# Mixins

1. `@mixin <name>($<arg>){@content}`
2. `@include <name>(){}`

```scss
@mixin box-shadow($x, $y, $blur, $c){
  -webkit-box-shadow: $x, $y, $blur, $c;
  -moz-box-shadow: $x, $y, $blur, $c;
  -ms-box-shadow: $x, $y, $blur, $c;
  box-shadow: $x, $y, $blur, $c;
}

div {
  @include box-shadow(0px, 0px, 4px, #fff);
}
```

# List Function 

1. `length(<list>)`
2. `nth(<list>,<position>)`(starting from 1)
3. `set-nth(<list>,<postition><value>)`
4. `list-seperate()`
5. `join(<list1>,<list2>,<seperator>)`
6. `append(<list>,<value>,<seperator>)`
7. `index(<list>,<value>)`
8. `zip(<list...>)` 

# Maps

1. `$<name> : (<'name'>:<value>,)`
2. `inspect()` returns the result into a string so that it won't conflict with the sass syntax
3. `map-keys(<map>)`
4. `map-values(<map>)`
5. `map-has-key(<map>,<keyname>)`
6. `map-get(<map>,<keyname>)`
7. `map-merge(<map1>,<map2>)`
8. `map-remove(<map>,<key...>)`



# Math Operations & Functions

1. `round()`
2. `floor()`
3. `ceil()`
4. `percentage()`
5. `abs()`
6. `min()`
7. `max()`
8. `random(<optional int>)`



# If Directives

1. `@if <variable> <condition>{}`
2. `@else if<condition>{}`
3. `@else{}`

```scss
@mixin text-effect($val) {
  @if $val == danger {
    color: red;
  }
  @else if $val == alert {
    color: yellow;
  }
  @else if $val == success {
    color: green;
  }
  @else {
    color: black;
  }
}
```



# Loops

1. `@for $i from <start int> through <end int>{h#{$i}}`
2. `@each<variable> in <list>{}` or  `@each <variable1>, <variable2> in <map>{}`
3. `@while <condition>{}`

```scss
@for $i from 1 through 12 {
  .col-#{$i} { width: 100%/12 * $i; }
}
```

```scss
@each $color in blue, red, green {
  .#{$color}-text {color: $color;}
}


$colors: (color1: blue, color2: red, color3: green);
@each $key, $color in $colors {
  .#{$color}-text {color: $color;}
}
```

```scss
$x: 1;
@while $x < 13 {
  .col-#{$x} { width: 100%/12 * $x;}
  $x: $x + 1;
}
```



# Function Directives

1. `@function <func name>(<arg...>){@return <result>}`



# Source Map

mapping the .scss to the .css and tell the browser where this css property comes from

