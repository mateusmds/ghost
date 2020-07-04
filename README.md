# ghost
A scripting esolang to summon spells and kill programs.<br/>
Inspired by the languages ZOMBIE and Pascal.

## Hello world in ghost

    main as entry
    spell main[_]:
    
    tell['Hello world!']
    end.
    
Current version: 1.0.2

## Introduction
Ghost is an interpreter forged to kill programs. To kill them, it must make the code fulfill its purpose so that it can die in peace.

## Design Principles

  * Ghost should be able to read and print<br/>
    content from user.
    
  * Ghost should be able to exhume scripts<br/>
    outside the code and use local `.lua` <br/>
    libraries.
    
  * Ghost should be able to explain why you<br/>
    are so dumb for not being able to kill<br/>
    a simple program.
    
## Usage
* Linux<br/>
  ``./ghost file.g`` to run directly the code.<br/>
  ``./ghost`` to show a hello entry.

## Code Syntax
Every script must start with a entry indication.

    <main-spell-name> as entry

### Comments
Anything after an `;` will disappear.

### Spells
Callable scopes in Ghost. The main spell will be called at the start of the program.

    spell <spell-name> [<args or _>]:
    
    ;your code
    end
    
The `end.` keyword will kill the program, so the main spell must use it as a termination. All other spells must end with `end`.

### Calling spells
  
  `#<spell-name>[<args or _>]` will jump to the spell scope and create locally `souls` if `args` is different of `_` (nothing).

### Spell return
Use the keyword `awake` to return something. If is an spell, the spell caller will be replaced by the returned value. If is the main spell, the `Dead message` will show the returned value.

    main as entry
    spell main[_]:
    
    tell[#hello['user']]
    end.
    
    spell hello[name]:
      
    awake 'Hello, ' pls ?name pls '!'
    end
    
    >>> Hello, user!

### I/O
Read and write values from or to user.

  `read[<name or _>]` returns the typed value by the user.<br/>
  `tell[<name or date>]` prints something to the user.

### Entities
Memory slots to read/write data. It can be `soul`s or `dead`s.

* ``soul <name> as <value>`` creates a global mutable entity.

* ``dead <name> as <value>`` creates a global immutable entity.

* ``graveyard <name> as (<values>,)`` creates a mutable array of values. (See Graveyards)

## Entity types
In a grave, there are only two types of information: names and dates.

``as 'name'`` : textual information, created with single quotes.<br/>
``as <date>`` : numeric information, which can be integer (`1`, `-20`) or floating point (`0.85`, `-2.5`).

## Read and write
To read an entity value, use the `?` operator. To write at the entity slot, use the `!` operator.

    dead bool as 1
    when ?bool : tell['true']
    else : tell['false']
    
    >>> true
    
    !bool as 0
    when ?bool : tell['true']
    else : tell['false']
    
    >>> false
    
## Operators

  * `mul` : same of `*`
  * `pow` : same of `^` or `**`
  
  * `div` : same of `/`
  * `mod` : same of `%`
  
  * `sum` : same of `+`
  * `min` : same of `-`
  
  * `eql` : same of `==`
  * `dif` : same of `~=` or `!=`
  
  * `grt` : same of `>`
  * `gre` : same of `>=`
  
  * `sml` : same of `<`
  * `sle` : same of `<=`
  
  * `and` : same of `&&`
  * `or`  : same of `||`
  
  * `not` : same of `!` or `not`
  
### Bolean Logic
Anything that is greater (or diferent) than 0 is true. Empty names (`''`) and `0` are false.

## IF-ELSE
Lines that are executed when something evaluates to true or false.

  * `when <val> : ;something` executed when `val`is true.
  * `else : ;something` executed when the logic line above is not executed.
  * `also : ;something` executet then the logic line is executed.
  
### Graveyards
A growable array of the same or different value types.

  ``@<graveyard-name>.<index>`` returns the value at the index.<br/>
  ``!<graveyard-name>.<index>`` set a new value at the index.<br/>
  ``@<graveyard-name>.len`` returns the length of the graveyard.

    graveyard fruits as ('apple', 'banana', 'pineapple')
    tell[@fruits.1] ;index starts at 1

    >>> apple

    !fruits.@fruits.len pls 1 as 'pineapple'
    tell[@fruits.@fruits.len]

    >>> pineapple

### Corpses
An corpse is an external `.g` file that is loaded inside the main file. (acts line `dofile` of Lua).
    
    ;corpuse.g
    necro as entry
    spell necro[_]:
    
    tell['hello, ' pls ?name pls '!']
    end.

    ;main.g
    main as entry
    spell main[_]:
    
    dead name as 'user'
    exhume corpuse ;or corpuse.g
    end.
    
    >>> hello, user!
    
### Libs
External `.lua` file that works as a internal Ghost lib. Use `invoke <module-name>` to load the file.

#### Mathlib
`invoke mathlib` Enables math functions and an random number generator.

  `min[<date>,]` returns the smallest gived `<date>`.<br/>
  `min[<date>,]` returns the greatest gived `<date>`.
  
  `sin[<date>]` returns sine of `<date>`.<br/>
  `cos[<date>]` returns cosine of `<date>`.<br/>
  `tan[<date>]` returns tangent of `<date>`.
  
  `asin[<date>]` returns arcsine of `<date>`.<br/>
  `acos[<date>]` returns arccosine of `<date>`.<br/>
  `atan[<date>]` returns arctangent of `<date>`.
  
  `sinh[<date>]` returns hyperbolic sine of `<date>`.<br/>
  `cosh[<date>]` returns hyperbolic cosine of `<date>`.<br/>
  `tanh[<date>]` returns hyperbolic tangent of `<date>`.
  
  `rand[<min>, <max>]` returns a random number between `<min>` and `<max>`.
