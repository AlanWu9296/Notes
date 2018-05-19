# Sass Convert

1. sass <source_file.format>:<target_file.format>
2. --watch (will automatically update the target file when the source file saved, cir-C to cancel , also can watch whole directory)
3. --style
   - compact 
   - compressed
   - expanded

# Variables

1. $<name> : <value>;
2. `#{$<element_name>}` call back the element name of scss into css



# Inheritance

1. Inheritated Selector
   - `&` to select the mother element
2. Inheritated Properties
   - `%<name>{}` to create a place holder of the properties
   - `@extend %<name>` to inheritate the values in the place holder



# Mixins

1. `@mixin <name>($<arg>){@content}`
2. `@include <name>(){}`



# Import

1. `@import '<name.format>'`
2. naming the file name starting with `_` means the file is *partial* thus will not be converted



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



# Loops

1. `@for $i from <start int> through <end int>{h#{$i}}`
2. `@each<variable> in <list>{}` or  `@each <variable1>, <variable2> in <map>{}`
3. `@while <condition>{}`



# Function Directives

1. `@function <func name>(<arg...>){@return <result>}`



# Source Map

mapping the .scss to the .css and tell the browser where this css property comes from

