## FLUX (Fluent Regex) 0.4.0
*by* [Selvin Ortiz](http://twitter.com/selvinortiz)

### Description
Fluent Regular Expressions _for_ PHP

----

### @Changelog

----
#### 0.4.0
- Adds `Flux` to the `Sortiz\Tools` namespace
- Implements `PSR-2` Compliance (Tabs over Spaces)
- Updates version number on `Flux` and this readme file
- Updates the class instantiation with fully qualified class name on `example.php`
- Adds references to other repos that have ported `flux`
- Addresses concerns outlined in [Issue #3](https://github.com/selvinortiz/flux/issues/3)

----
#### 0.3.0
- Improves documentation with `phone/date` examples
- Adds the `letters()` method
- Renames the `numbers()` method to `digits()`
- Adds support for quantifiers for `digits()`
- Adds `ignoreCase()` and promotes it above `inAnyCase()`
- Improves the documented API

_Thought hard about changing the name to `FluentX` any thoughts?_

----
#### 0.2.0
- Adds the `either( $option1, $option2 [, $option3 ...] )` method to handle OR cases
- Updates the *fluent* example in this readme file
- Adds the license

----
#### 0.1.0
Initial preview release

----

### @Todo
- Add source code comments
- Add support for quantifiers
- Add language methods for more advanced use cases
- Add support for array/array replacements
- Add reference to repos that have ported `Flux` (*)
- Add license notes (*)
- Add contributing notes
- Add credits

### @Examples

```php
/**
 * Build a URL pattern then test w/ match() and do a replace()
 */
$url	= 'http://www.selvinortiz.com';
$flux	= new Sortiz\Tools\Flux();
$flux
	->startOfLine()
	->find('http')
	->maybe('s')
	->then('://')
	->maybe('www.')
	->anythingBut('.')
	->either('.co', '.com')
	->ignoreCase()
	->endOfLine();

// URL Pattern /^(http)(s)?(\:\/\/)(www\.)?([^\.]*)(.co|.com)$/i
echo $flux->match( $url ); // true
echo $flux->replace( 'https://$5$6', $url ); // https://selvinortiz.com

/**
 * Build a US Date pattern then test w/ match() and do a replace()
 */
$date	= 'Monday, Jul 22, 2013';
$flux	= new Sortiz\Tools\Flux();
$flux
	->startOfLine()
	->word()
	->then(', ')
	->letters(3)
	->then(' ')
	->digits(1, 2)
	->then(', ')
	->digits(4)
	->endOfLine();

// Date Pattern /^(\w+)(, )([a-zA-Z]{3})( )(\d{1,2})(, )(\d{4})$/
echo $flux->match( $date ) ? 'matched' : 'unmatched'; // matched
echo $flux->replace( '$3/$5/$7', $date ); // Jul/22/2013

/**
 * Build a US Phone Number pattern then test w/ match() and do a replace()
 */
$phone	= '(612) 424-0013';
$flux	= new Sortiz\Tools\Flux();
$flux
	->startOfLine()
	->find('(')
	->digits(3)
	->then(')')
	->maybe(' ')
	->digits(3)
	->anyOf(' -')
	->digits(4)
	->endOfLine();

// Phone Pattern /^(\()(\d{3})(\))( )?(\d{3})([ \-])(\d{4})$/
echo $flux->match( $phone ) ? 'matched' : 'unmatched'; // matched
echo $flux->replace( '$2.$5.$7', $phone ); // 612.424.0013
```

### FLUX API
The **flux** API was designed to give you a _fluent chainable object_ to build patterns with.

#### `startOfLine()`
Adds a beginning of line `^` modifier

#### `endOfLine()`
Adds an end of line `$` modifier

#### `find( $val ) & then( $val )`
Allow you to augment the pattern with a required `segment` and it escapes regular expression characters

#### `maybe( $val )`
Allows you to augment the pattern with an optional `segment`

#### `any( $val ) & anyOf( $val )`
Allow you to create a set of characters to match

#### `anything()`
Adds a *wild card* `(.*)` `segment` to the pattern but it does not make `dotAll()` explicit

#### `anythingBut( $val )`
Will match anything but the characters in `$val` which is opposite of `any()` and `anyOf`

#### `word()`
Adds `(\w+)` to the pattern which will match a single word

#### `letters( $min=null, $max=null )`
Only matches characters in the alphabet and uses `$min` and `$max` to create a quantifier

#### `digits( $mix=null, $max=null )`
Only matches digits and uses `$min` and `$max` to create a quantifier like `word()`

#### `range( $from, $to [, $from, $to ...])`
Allows you to create a `range` character class like `a-z0-9` by calling `range('a', 'z', 0, 9)`

#### `orTry()`
This is experimental and I don't have the implementation I feel comfortable with... yet!

#### `ignoreCase() & inAnyCase()`
Adds the `i` modifier to the pattern which will allow you to match in a case insensitive manner

#### `dotAll()`
Adds the `s` modifier to the pattern which will allow you to match a `new line` when using `anything()`

#### `multiline()`
Adds the `m` modifier to the pattern which will allow you to search across multiple lines

#### `searchOneLine()`
Removes the modifier added by `multiline()` if it was previously called

#### `match( $subject )`
Simply takes your `$subject` in, compares it against the pattern, and returns whether a it matched or not

#### `replace( $replacement, $subject )`
You can replace matched `segments` by using the `$x` format where `x` is the `(int)` position of the matched `segment`

----

### Flux Elsewhere
There is interest in porting `Flux` to other languages/platforms like `NodeJS`, `Groovy` and `Java` they'll be listed here once available.

* [NodeJS](https://npmjs.org/package/node-flux) _by_ [James Brooks](http://james.brooks.so)

### Feedback
This is something that started as a weekend experiment but I would love to take it further so if you have any suggestions, please fire away!

_The best way to get in touch with me is via twitter [@selvinortiz](http://twitter.com/selvinortiz) we'll take if from there_ :)

### MIT License
*Flux* is released under the [MIT license](http://opensource.org/licenses/MIT) which pretty much means you can do with it as you please and I won't get mad because I'm that nice; )
