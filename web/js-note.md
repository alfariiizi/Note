# JavaScript Personal Note

Great Resource: 
- [The Modern JavaScript Tutorial](https://javascript.info/)


## Basic (Fundamental)

### [The Modern Mode, `"use strict"`](https://javascript.info/strict-mode)

`"use strict"` ini berguna untuk mengaktifkan modern mode pada javascript. Code tersebut seperti:
- `#version 330` pada GLSL.
- `cmake_minimum_required()` pada CMake.

Dikarenakan fungsi nya tersebut, `"use strict"` diletakkan di paling awal pada suatu source code.
Meskipun begitu, modern features dari javascript seperti classses dan modules biasanya sudah otomatis terdapat `"use strict"` didalamnya 👍🏼.


### [Variable](https://javascript.info/variables)

Ada 3 keyword jika ingin membuat suatu variable:
- `let`: mutable.
- `const`: immutable.
- `var`: the old-school way of `let`.

Overall, cara penamaan di javascript itu sama kayak yang ada di Linux C/C++.


### [Data Type](https://javascript.info/types)

Ada 8 basic data type di javascript:
- `number` for numbers of any kind: integer or floating-point, integers are limited by $±(2^{53}-1)$.
- `bigint` is for integer numbers of arbitrary length.
- `string` for strings. A string may have zero or more characters, there’s no separate single-character type.
- `boolean` for true/false.
- `null` for unknown values – a standalone type that has a single value null.
- `undefined` for unassigned values – a standalone type that has a single value undefined.
- `object` for more complex data structures.
- `symbol` for unique identifiers.

`typeof` operator, syntax (`someThing` disitu bisa berupa variable, function, object, class, dll):
- `typeof someThing`
- `typeof(someThing)`

Contoh:
```js
let name = "far";
let x = 10;

console.log( `the name is ${name}` );                                 // the name is far
console.log( 'the type of name: ' + typeof name );                    // the type of name: string
console.log( "the type of typeof(name): " + typeof( typeof name ) );  // the type of typeof(name): string

console.log( typeof x );                // number
console.log( typeof( typeof x ) );      // string
```

> 💡 Look at those: `` ` ``(backquote), `'`(single-quote), and `"`(double-quote).


### [`alert`, `prompt`, and `confirm`](https://javascript.info/alert-prompt-confirm)

> ⚠️ Fungsi - fungsi ini hanya dapat digunakan dalam suatu text HTML, tidak bisa digunakan pada console program seperti nodejs.

Fungsi - fungsi akan memunculkan suatu window pop-up yang meminta suatu input dari user.
- `alert` - memberitahu ke user, dimana user hanya dapat mengeklik "Oke" sebagai inputnya.
  - syntax:
    ```js
    // message: string => Teks pemberitahuan ke user
    alert(message);
    ```
  - contoh:
    ```js
    alert("Hello");
    ```
- `prompt` - meminta suatu input berupa `string`, jika user tidak mau mengisi dengan mengeklik "Cancel" atau menekan ESC key maka akan me-return `null`.
  - syntax:
    ```js
    // title: string => Sesuatu yang ditanyakan ke user
    // default: string => Nilai default nya
    result = prompt(title, [default]);
    ```
    > `[...]`(brackets) notation digunakan untuk memberitahu bahwa argument tersebut _opsional_ (boleh tidak diisi).
  - contoh:
    ```js
    let age = prompt('How old are you?', 100);
    alert(`You are ${age} years old!`);
    ```
- `confirm` - meminta suatu input berupa `boolean` berupa "OK" (`true`) atau "Cancel" (`false`).
  - syntax:
    ```js
    // question: string => Sesuatu yang diitanyakan ke user
    result = confirm(question);
    ```
  - contoh:
    ```js
    let isBoss = confirm("Are you the boss?");
    alert( isBoss ); // true if OK is pressed
    ```


### [Type Conversion](https://javascript.info/type-conversions)

- `string` conversion: `String(value)`
- `number` conversion: `Number(value)`
- `boolean` conversion: `Boolean(value)`

Hasil dari tipe data satu ke lainnya dapat dilihat di bagian [Summary](https://javascript.info/type-conversions#summary).


### [Basic Operators, Maths](https://javascript.info/operators)

Hampir semua (95%) operator sudah cukup mirip dengan operator yang ada di C++.


### [Comparisons](https://javascript.info/comparison)

Yaaa kebanyakan (90%) udah mirip C++. Yang membedakan adalah comparison yang ada javascript special keyword seperti `undefined` (beda karena di C++ ngga ada keyword tersebut 🤣)


### [Nullish coalescing operator `??`](https://javascript.info/nullish-coalescing-operator)

Membahas tentang operator `||`(OR) dan operator `??`.

- || returns the first truthy value.
- ?? returns the first defined value.

```js
let height = 0;

alert(height || 100); // 100
alert(height ?? 100); // 0
```

Operator `??` tersebut biasanya digunakan untuk mengatasi value yang `undefined` dan `null`.

Operator `||` biasanya juga digunakan untuk mengatasi value yang `undefined` dan `null`, serta juga `false` value (value nol)

> 🃏 Argumen aja punya _pengganti_, udah kayak pemain bola aja.


### [Label](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/label)

Label di JS ini cukup mirip dengan Label (label-`goto`) yang ada di C++.


### [Function Declaration](https://javascript.info/function-basics)

Contoh basic function:
```js
function showCount(count) {
  // if count is undefined or null, show "unknown"
  alert(count ?? "unknown");
}

showCount(0); // 0
showCount(null); // unknown
showCount(); // unknown
```

Mirip kayak python yaa kan, cuman kalo di python pakek keyword `def`, sedangkan di JS pakek keyword `function`.


### [Function Expression](https://javascript.info/function-expressions)

Contoh basic:
```js
// Function Expression
let sum = function(a, b) {
  return a + b;
};
```
Function Expression ini sangat berguna pada *conditional declaration*:
```js
let age = prompt("What is your age?", 18);

let welcome = (age < 18) ?
  function() { alert("Hello!"); } :
  function() { alert("Greetings!"); };

welcome(); // No Error
```
Kode diatas tidak bisa diimplementasikan menggunakan function declaration, sehingga solusi nya adalah dengan menggunakan function expression.

> ❓ _Kapan kita menggunakan [function declaration](#function-declaration)? Kapan kita menggunakan [function expression](#function-expression)_?
>
> Answer:
> 
> As a rule of thumb, when we need to declare a function, the first to consider is Function Declaration syntax. It gives more freedom in how to organize our code, because we can call such functions before they are declared.
> 
> That’s also better for readability, as it’s easier to look up `function f(…) {…}` in the code than `let f = function(…) {…};`. Function Declarations are more “_eye-catching_”.
> 
> …But if a Function Declaration does not suit us for some reason, or we need a *conditional declaration* (we’ve just seen an example), then Function Expression should be used.


### [Arrow functions, the basics](https://javascript.info/arrow-functions-basics)

Contoh **single line** arrow function:
```js
/* This arrow function is a shorter form of:

let sum = function(a, b) {
  return a + b;
};
*/
let sum = (a, b) => a + b;

// No argumen
let sayHi = () => alert("Hello!");

// Parameter `n` tidak butuh parentheses karena hanya 1 parameter
let double = n => n * 2;
```

Contoh **multiline** arrow function:
```js
let sum = (a, b) => {  // the curly brace opens a multiline function
    let result = a + b;
    return result; // if we use curly braces, then we need an explicit "return"
};
```
> Single-line arrow function ini langsung mereturn suatu _expression_.

Perbandingan langsung _arrow function_ dengan _function expression_:
```js
// arrow function (yang multiline)
let sum = (a, b) => { return a + b; };

// function declation
let sum = function(a, b) { return a + b; };
```
Sehingga, keyword `function(a, b)` diganti menjadi `(a, b) =>`.
> Multiline arrow function berubah dari _expression_ menjadi _body_. Maka perlu adanya `return` di kode diatas.


### [Javascript Fundamental Recap](https://javascript.info/javascript-specials)

Recap fundamental javascript. Ada beberapa section yang mungkin saya lewati dengan sengaja karena mungkin menurut saya syntax nya mirip C++. Tetapi, jika ternyata syntax nya ada yang cukup berbeda sekali dengan yang ada di C++ serta belum saya masukkan ke catatan diatas, maka kemungkinan besar saya akan catatan tersebut.