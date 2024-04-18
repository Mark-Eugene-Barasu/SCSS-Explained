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

   SCSS                                     
*circle {                                  
    $size:100px;                               
    width: $size;                              
    height: $size;                             
    border-radius: $size/2;                    
}                         

    SASS
*circle 
    $size:100px
    width: $size
    height: $size
    border-radius: radius/2

### Nesting
Def: Many CSS properties start with a name prefix that acts as a kind of namespace. For example: font-family, font-size, font-weight all start with prefix font-
SCSS makes it easier and less redundant by allowing properties declaration to be nested.
The outer property name are added to the inner, separated by a hyphen. 

.enlarge {                                  
    font-size: 14px;                            
    transition: {                           
        property: font-size;
        duration: 4s;
        delay: 2s;
    }
    &:hover {
        font-size: 36px;
    }
}

.gray {
    @include prefix(filter, greyscale(50%), moz webkit);
}
    
### Interpolation 
Def: A property's name can include interpolation, which makes it possible to dynamically generate properties as needed. You can even interpolate the entire property name.

@mixin prefix($property, $value, $prefixes){
    @each $prefix in $prefixes {
        -#{$prefix}-#{$prefixes}: $value;
    }
    #{$property}: $value;
}

.gray {
    @include prefix(filter, grayscale(50), moz webkit);
}

### Hidden Declarations
Def: Sometime you only want a property declaration to show up some of the time. If a declaration's value is null or an empty unquoted string,
SCSS won't compile that declaration to CSS at all

$round-corners: false;
.button {
    border: 1px solid black;
    border-radius: if($rounded-corners, 5px, null);
}

### Default Values
Def: Normally when you assign a value to a variable, if that variable already has a value, its old value is overwritten. To make this possible, SCSS provides the !default flag. This assigns a value to a variable only if that variable is not defined or its value is null. Otherwise, the existing value will be used.

using the !default.
$white: #fff !default
$black: #000 !default

Variables
$border-radius: 0.2rem !default;
$border-shadow: 0 0.5rem 1rem rgba($black, 0.15)!default;

so, from here you can resign the same variable and the values you assign them will take presidency over the same variable name with the !default flag.

Call the Variables in SCSS
button {
    border-radius: $border-radius;
    box-shadow: $box-shadow;
}

button {
    border-radius: 0.1rem;
    box-shadow: 0 0.5rem 1rem rgba(34, 34, 34, 0.15);
}

### Scope
Def: Variables declared at the top level of a stylesheet are global. This means that they can be accessed anywhere in their module after they've been declared. But that's not true for all variables. Those declared in blocks.

SCSS
    $global-variable: global value;

    .header{
        $local-variable: local value;
        global: $global-variable;
        local; $local-variable;
    }
    .main {
        global: $global-variable:
    }

CSS
    .header {
        global: global value;
        local: local value;
    }

    .main {
        global: global value;
    }

### Shadowing 
Def: Local variables can even be declared with the same name as a global variable. If this happens, there are actually two different variables with the same name: one local and one global. This helps ensure that an author writing a local variable doesn't accidentally change the value of a global variable they aren't even aware of.

without !global yet
SCSS
    $variable: global value;

    .header{
        $variable: local value;
        value: $variable;
    }
    .main {
        value: $variable;
    }

CSS
    .header {
        value: local value;
    }
    .main {
        value: global value;
    }

Def: !global flag. A variable declaration flagged as !global will always assign to the global scope.

now with !global
SCSS
    $variable: first global value;

    .header {
        $variable: second global value !global;
        value: $variable;
    }

    .main {
        value: $variable;
    }

CSS
    .header {
        value: $second global value;
    }

    .main {
        value: $second global value;
    }

### Flow Control
Def: Variables declared in flow control rules have special scoping rules: they don't shadow variables at the same level as the flow control rule. Instead, they just assign to those variables. This makes it much easier to conditionally assign a value to a variable, or build up a value as part of a loop. 

SCSS
    $dark-theme: true !default;
    $primary-color: #f8bbd0 !default;
    $accent-color: #6a1b9a !default;

    @if $dark-theme {
        $primary-color: darken($primary-color, 60%);
        $accent-color: lighten($accent-color, 60%);
    }
    .button {
        background-color: $primary-color;
        border: 1px solid $accent-color;
        border-radius: 3px;
    }

CSS
    .button {
        background-color: #750c30;
        border: 1px solid #f5ebfc;
        border-radius: 3px;
    }
    .main {
        value: global value;
    }