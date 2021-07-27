# Javascript Data Types


## [Method of Primitives](https://javascript.info/primitives-methods)

```js
let str = "Hello";
alert( str.toUpperCase() ); // HELLO
```

Ingat, _Primitive Type_ itu ada 7:
- `string`
- `number`
- `bigint`
- `boolean`
- `symbol`
- `null`
- `undefined`

### Is `Number`, `String`, and `Boolean` are Objects ?

Well yesss !, but ..., There are no but wkwk. Absolutely right!. Those type are Objects!.

Javascript bakal ngebuat temporary object dari tipe - tipe data tersebut jika primitive variablenya manggil object method.
Maka dari itu, kalo ngebikin variable itu jangan pakek `new` operator:
```js
let str = String("Hello");
let str2 = new String("Hello");

console.log( typeof str );  // string
console.log( typeof str2 ); // object
```

> javascript ngebuat agar primitive type `undefined` dan `null` tidak punya method apapun.


## [Numbers](https://javascript.info/number)

To write numbers with many zeroes:
- Append "`e`" with the zeroes count to the number. Like: `123e6` is the same as 123 with 6 zeroes 123000000.
- A negative number after "`e`" causes the number to be divided by 1 with given zeroes. E.g. `123e-6` means 0.000123 (123 millionths).

For different numeral systems:
- Can write numbers directly in hex (`0x`), octal (`0o`) and binary (`0b`) systems.
- `parseInt(str, base)` parses the string str into an integer in numeral system with given base, $2 ≤ base ≤ 36$.
- `num.toString(base)` converts a number to a string in the numeral system with the given base.

For converting values like `12pt` and `100px` to a number:
- Use `parseInt`/`parseFloat` for the “soft” conversion, which reads a number from a string and then returns the value they could read before the error.
    ```js
    alert( parseInt('100px') );     // 100
    alert( parseFloat('12.5em') );  // 12.5
    alert( parseInt('12.3') );      // 12, only the integer part is returned
    alert( parseFloat('12.3.4') );  // 12.3, the second point stops the reading
    alert( parseInt('a123') );      // NaN, the first symbol stops the process

    // ..+ also using 2nd argumen as a BASE +..
    alert( parseInt('0xff', 16) );  // 255
    alert( parseInt('ff', 16) );    // 255, without 0x also works
    alert( parseInt('2n9c', 36) );  // 123456
    ```

For fractions:
- Round using Math.floor, Math.ceil, Math.trunc, Math.round or num.toFixed(precision).
- Make sure to remember there’s a loss of precision when working with fractions.

More mathematical functions:
- See the Math object when you need them. The library is very small, but can cover basic needs.


## [Strings](https://javascript.info/string)

```js
// ..+ Basic Declaration +..
let single = 'single-quoted';
let double = "double-quoted";
let backticks = `backticks`;
// math
alert(`1 + 2 = ${1 + 2}.`); // 1 + 2 = 3.   (with backticks)

// ..+ Multiline String +..
// with backticks
let guestList = `Guests:
 * John
 * Pete
 * Mary
`;
// same as above
let guestList = "Guests:\n * John\n * Pete\n * Mary";

// ..+ unicode and others special character like \n, \b, \a, etc. +..
alert( "\u{20331}" ); // 佫, a rare Chinese hieroglyph (long Unicode)
alert( "\u{1F60D}" ); // 😍, a smiling face symbol (another long Unicode)

// ..+ length +..
alert( `My\n`.length ); // 3

// ..+ Accessing character +..
let str = `Hello`;
// the first character
alert( str[0] ); // H
alert( str.charAt(0) ); // H
// the last character
alert( str[str.length - 1] ); // o
```

### Summary
> Sangat banyak sekali isi dari materi _String_ ini, bisa dilihat saja langsung di web nya.

- There are 3 types of quotes. Backticks allow a string to span multiple lines and embed expressions `${…}`.
- Strings in JavaScript are encoded using UTF-16.
- We can use special characters like `\n` and insert letters by their Unicode using `\u...`.
- To get a character, use: `[]`.
- To get a substring, use: `slice` or `substring`.
- To lowercase/uppercase a string, use: `toLowerCase`/`toUpperCase`.
- To look for a substring, use: `indexOf`, or `includes`/`startsWith`/`endsWith` for simple checks.
- To compare strings according to the language, use: `localeCompare`, otherwise they are compared by character codes.

There are several other helpful methods in strings:

- `str.trim()` – removes (“trims”) spaces from the beginning and end of the string.
- `str.repeat(n)` – repeats the string `n` times.
- …and more to be found in the [manual](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String).

Strings also have methods for doing search/replace with regular expressions. But that’s big topic, so it’s explained in a separate tutorial section [Regular expressions](https://javascript.info/regular-expressions).


## [Array](https://javascript.info/array)

```js
// ..+ How to declare +..
let arr = new Array();
// this code below is same as above
let arr = [];

// ..+ Accessing array element +..
let fruits = ["Apple", "Orange", "Plum"];
alert( fruits[0] );     // Apple
alert( fruits[1] );     // Orange
alert( fruits[2] );     // Plum
fruits[2] = 'Pear';     // now ["Apple", "Orange", "Pear"]
fruits[3] = 'Lemon';    // now ["Apple", "Orange", "Pear", "Lemon"]
alert( fruits.length ); // 4
alert( fruits );        // Apple,Orange,Pear,Lemon

// ..+ Mix element type +..
let arr = [ 'Apple', { name: 'John' }, true, function() { alert('hello'); } ];
// get the object at index 1 and then show its name
alert( arr[1].name ); // John
// get the function at index 3 and run it
arr[3](); // hello

// ..+ Trailing Comma +..
let fruits = [
  "Apple",
  "Orange",
  "Plum",   // <-- The trailing comma (like trailing comma in Object type)
];
```

### Summary

> Karena topik _array_ ini cukup banyak sekali yang dibahas di website nya. Agar tetap concise catatan ini, saya tulis summary nya saja

Array is a special kind of object, suited to storing and managing ordered data items.

- The declaration:
- The call to `new Array(number)` creates an array with the given length, but without elements.

- The length property is the array length or, to be precise, its last numeric index plus one. It is auto-adjusted by array methods.

- If we shorten length manually, the array is truncated.

We can use an array as a deque with the following operations:

- `push(...items)` adds items to the end.
- `pop()` removes the element from the end and returns it.
- `shift()` removes the element from the beginning and returns it.
- `unshift(...items)` adds items to the beginning.

To loop over the elements of the array:

- `for (let i=0; i<arr.length; i++)` – works fastest, old-browser-compatible.
- `for (let item of arr)` – the modern syntax for items only,
- `for (let i in arr)` – never use.

To compare arrays, don’t use the `==` operator (as well as `>`, `<` and others), as they have no special treatment for arrays. They handle them as any objects, and it’s not what we usually want.

Instead you can use _for..of_ loop to compare arrays item-by-item.

We will continue with arrays and study more methods to add, remove, extract elements and sort arrays in the next chapter [Array methods](https://javascript.info/array-methods).