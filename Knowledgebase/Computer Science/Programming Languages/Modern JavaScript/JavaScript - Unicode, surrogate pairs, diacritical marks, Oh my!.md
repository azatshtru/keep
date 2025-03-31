# Unicode representation in JavaScript
A string in JavaScript can hold any unicode character, unicode characters are represented as `\u{XXXXXX}` where `XXXXXX` can be replaced by any hexadecimal unicode value.
```javascript
alert( "\u{20331}" ); // 佫, a rare Chinese character (long Unicode)
alert( "\u{1F60D}" ); // 😍, a smiling face symbol (another long Unicode)
```

Additionally, if a unicode character only requires two hexadecimal digits, then it can be represented as `\xXX` where `XX` is replaced by the two-digit hexadecimal value. Also, if a unicode character only requires 4 hex characters, then `\xXXXX` can also be used to represent it.
# Surrogate Pairs
Two 4-digit hex values that mean nothing on their own in context of unicode, but can be used together are said to form a surrogate pair.

If a character has the code in the interval of `0xd800..0xdbff`, then it is the first part of the surrogate pair. The next character (second part) must have the code in interval `0xdc00..0xdfff`.

Surrogate pairs exist because there was a time when more-than-4-digit-unicodes didn't exist and people needed a way to represent more characters than were allowed by the 4-digit-unicode system. Hence, they used two 4-digit hexcodes to represent a single character.

Now that more-than-4-digit-unicode has been standardized, A surrogate pair can be replaced by a corresponding more-than-4-digit-unicode. 

For example, consider "mathematical script capital x", i.e. '𝒳', the unicode hexcode for this is `\x1d4b3`. However, this can also be represented by a surrogate pair `'\xd835\xdcb3'`.

`charCodeAt` can be used to get parts of a surrogate pair.
```javascript
alert( '𝒳'.charCodeAt(0).toString(16) ); // d835
alert( '𝒳'.charCodeAt(1).toString(16) ); // dcb3
```

`charPointAt` can be used to get the  surrogate pair.
```javascript
alert( '𝒳'.codePointAt(0).toString(16) ); // 1d4b3, reads both parts of the surrogate pair
```

as a side-effect of surrogate pairs, JavaScript returns two when the length property of a character with surrogate pair is accessed.
```javascript
alert( '𝒳'.length ); // 2, MATHEMATICAL SCRIPT CAPITAL X
alert( '😂'.length ); // 2, FACE WITH TEARS OF JOY
alert( '𩷶'.length ); // 2, a rare Chinese character
```

> [!CAUTION] splitting strings at an arbitrary point is dangerous
> Since the individual halves of a surrogate pair mean nothing on their own, if we slice a string arbitrarily, we can get one half of a surrogate pair without the other, resulting in invalid characters.
> ```javascript
> alert( 'hi 😂'.slice(0, 4) ); //  hi [?]
> ```
# Diacritical marks
Diacritical marks are used to represent the combination of a base character and accent or other symbols above and below the base character.
For instance, the letter `a` can be the base character for these characters: `àáâäãåā`. I'll refer to these marks above and below as "decorators" in the following text.

To represent these accented characters, unicode has one base character followed by a decorator.
For instance, if we have `S` followed by the special “dot above” character (code `\u0307`), it is shown as `Ṡ`.

To support arbitrary compositions, the Unicode standard allows us to use several Unicode characters: the base character followed by one or many decorators.
For instance, `S\u0307\u0323` is S followed by "dot above", then followed by "dot below", which gives `Ṩ`
## Unicode Normalization
Which decorator comes first, and which comes after?
`S\u0307\u0323` and `S\u0323\u0307` both represent `Ṩ`, but one has "dot above" before "dot below" and the other has the opposite. This can cause ambiguity.
For instance:
```javascript
let s1 = 'S\u0307\u0323'; // Ṩ, S + dot above + dot below
let s2 = 'S\u0323\u0307'; // Ṩ, S + dot below + dot above

alert( s1 == s2 ); // false though the characters look identical
```

To battle this ambiguity, there exists a normalization algorithm that brings each string to a simple normal form. The result of normalization algorithm is the most "direct" representation of the character in unicode. For example, `\u1e68` is the direct unicode representation of `Ṩ`. Hence, when `S\u0307\u0323` is normalized, it will give `\u1e68`.

In JavaScript, strings can be normalized using the `.normalized()` function.
```javascript
alert( "S\u0307\u0323".normalize() ) // \u1e68
alert( "S\u0307\u0323".normalize() == "S\u0323\u0307".normalize() ); // true
alert( "S\u0307\u0323".normalize().length ); // 1
```