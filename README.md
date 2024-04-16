# SCSS-Explained
This is a documentation of the different sub-topics about SCSS:

## Statement
Def: A SCSS or style sheet is made up of a series of statEment, which are evaluated in order to build the resulting CSS.

### Universal Statement
Def:  SCSS style sheet statement types:
* variable declaration, like $var: value;
* flow control "at rule" @if and @each
* The @error, @warn and @debug rules

### Top Level Statements
Def: These statements can only be used at the top level of a stylesheet, or nested in CSS statement at the top level:
* Module load, using @use
* Import using @import
* Mixin definitions using @mixin
* Function definition using @function

### Style Rules
Def: There are few ways of writing SCSS, using the SASS syntax or the SCSS Syntax

   SCSS                                         SASS
*circle {                                  *circle 
    $size:100px;                               $size:100px
    width: $size;                              width: $size
    height: $size;                             height: $size
    border-radius: $size/2;                    border-radius: radius/2
}                                          

### Nesting
Def: Many CSS properties start with a name prefix that acts as a kind of namespace. For example: font-family, font-size, font-weight all start with prefix font-
SCSS makes it easier and less redundant by allowing properties declaration to be nested.
The outer property name are added to the inner, separated by a hyphen. 

*enlarge {                                  *gray {
    font-size: 14px;                            @include prefix(filter, greyscale(50%), moz webkit);
    transition: {                            }
        property: font-size;
        duration: 4s;
        delay: 2s;
    }
    &:hover {
        font-size: 36px;
    }
}

### Interpolation 
Def: A property's name can include interpolation, which makes it possible to dynamically generate properties as needed. You can even interpolate the entire property name.

@mixin prefix($property, $value, $prefixes){
    @each $prefix in $prefixes {
        -#{$prefix}-#{$prefixes}: $value;
    }
    #{$property}: $value;
}

*gray {
    @include prefix(filter, grayscale(50), moz webkit);
}

### Hidden Declarations
Def: Sometime you only want a property declaration to show up some of the time. If a declaration's value is null or an empty unquoted string,
SCSS won't compile that declaration to CSS at all

$round-corners: false;
* button {
    border: 1px solid black;
    border-radius: if($rounded-corners, 5px, null);
}