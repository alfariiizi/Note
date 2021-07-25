# Javascript Object


## [Objects](https://javascript.info/object)

> 💡 Object ini sebenernya seperti Hash Map atau Unordered Map nya C++.

2 Cara untuk membuat object:
```js
// "object constructor" syntax
let user = new Object();

// "object literal" syntax
let user = {};
```

### Literal dan propertinya
```js
let user = {     // an object
  name: "John",  // by key "name" store value "John"
  age: 30        // by key "age" store value 30
};

console.log(user.name); // John

user.isAdmin = true;    // add isAdmin to the user

delete user.age;        // delete age from the user
```
> Karena `age` juga boleh dikasih `,`(comma) di belakangnya meski ia merupakan properti terakhir.
> Hal ini biasanya dilakukan agar mempermudah penulisan saat ingin menambah baris properti baru.

### Square Brackets
```js
let user = {
  name: "John",
  age: 30,
  "likes birds": true  // multiword property name must be quoted
};

let keyBirds = "likes birds";
let keyName = name;

console.log( user[name] );          // John
console.log( user["likes birds"] ); // true
console.log( user[keyBirds] );      // true
console.log( user[keyName] );       // John

console.log( user.name );           // John
// console.log( user.keyName );        // ERROR
// console.log( user.keyBirds );       // ERROR

```

#### Computed properties

```js
// ..+ Computed Properties +..
let fruit = prompt("Which fruit to buy?", "apple");
let bag = {
    [fruit]: 5, // the name of the property is taken from the variable fruit
};

// ..+ Computed Properties essentially the same as: +..
let fruit = prompt("Which fruit to buy?", "apple");
let bag = {};
bag[fruit] = 5;

// ..+ Contoh lebih complex nya +..
let fruit = 'apple';
let bag = {
  [fruit + 'Computers']: 5 // bag.appleComputers = 5
};

// ..+ Contoh complex diatas equivalent dengan: +..
let fruit = 'apple';
let bag = {};
bag[fruit + "Computers"] = 5;   // bag.appleComputers = 5
```

### Property value shorthand

```js
// ..+ 1 +..
function makeUser(name, age) {
  return {
    name: name,     // OK
    age: age,       // OK
  };
}

// ..+ 2 +..
function makeUser(name, age) {
  return {
    name, // same as name: name
    age,  // same as age: age
  };
}

// ..+ 3 +..
let user = {
  name,  // same as name: name
  age: 30,
};
```

### Property name limitation

```js
// these properties are all right
let obj = {
    for: 1,     // same as "for": 1
    let: 2,     // same as "let": 2
    return: 3,  // same as "return": 3
    0: "test"   // same as "0": "test"
};

console.log( obj.let );     // 3
console.log( obj[0] );      // test
```
> Meskipun bisa pakek nama-nama reserve (`for`, `let`, dsb.), JANGAN PAKEK NAMA `__proto__` karena dia itu punya behaviour yang rada aneh.

### Property existance test, `in` operator

Ada 2 cara untuk memastikan apakah ada property (let's say) `A` dalam object `Obj`:
- Menggunakan `undefined`:
    ```js
    let user = {};

    console.log( user.noSuchProperty === undefined ); // true means "no such property"
    ```
- Menggunakan `in` operator:
    ```js
    let user = { name: "John", age: 30 };

    let keyAge = "age"

    console.log( "age" in user );       // true, user.age exists
    console.log( "blabla" in user );    // false, user.blabla doesn't exist
    console.log( keyAge in user );      // true
    ```

> Saran saya, pakek saja `in` operator untuk mengecek daripada `undefined`, karena ada beberapa kasus yang hanya _works_ di `in` operator.

### The for...in loop

```js
let user = {
  name: "John",
  age: 30,
  isAdmin: true
};

for (let key in user) {
  // keys
  alert( key );  // name, age, isAdmin
  // values for the keys
  alert( user[key] ); // John, 30, true
}
```

#### Ordered like an object (SOOO..., THE HASH MAP KINDA MAKE THE HASH TABLE BECOME ORDERED MAP)
```js
let codes = {
  "49": "Germany",
  "41": "Switzerland",
  "44": "Great Britain",
  "1": "USA"
};

for (let code in codes) {
  alert(code); // 1, 41, 44, 49
}
```

Cheat, agar ordered map nya bisa tetep unordered map:
```js
let codes = {
  "+49": "Germany",
  "+41": "Switzerland",
  "+44": "Great Britain",
  "+1": "USA"
};

for (let code in codes) {
  alert( +code ); // 49, 41, 44, 1
}
```


## [Object references and copying](https://javascript.info/object-copy)

```js
// Primitive Data type - message dan phrase punya address memori yang berbeda
let message = "Hello!";
let phrase = message;

// Objects - user dan admin punya address memori yang sama
let user = { name: "John" };
let admin = user; // copy the reference
```

> Pada C++, _copy_, _reference_, dan _move_ itu biasanya saling berlawanan satu sama lain.
> 
> Pada Javascript ini, ada term _copy reference_ yang mana maksudnya adalah multiple object me-referencing satu memory address. Mirip kayak _reference_-ing kalo di C++.

### Copying value

#### Manual Copying

```js
let user = {
  name: "John",
  age: 30
};

let clone = {}; // the new empty object

// let's copy all user properties into it
for (let key in user) {
  clone[key] = user[key];
}

// now clone is a fully independent object with the same content
clone.name = "Pete"; // changed the data in it

alert( user.name ); // still John in the original object
```

#### Use `assign` method

##### Merging with `assign` method
```js
let user = { name: "John", canView: false };

let permissions1 = { canView: true };
let permissions2 = { canEdit: true };

// copies all properties from permissions1 and permissions2 into user
Object.assign(user, permissions1, permissions2);

// now user = { name: "John", canView: true, canEdit: true }
```
> `canView` ter-overwrite. `canEdit` tertambahkan ke property `user`.

##### Cloning with `assign` method
`assign` method ini menggunakan ***shallow copy***. Sehingga untuk object non-nested, method ini bisa digunakan. Untuk object yang nested, perlu digunakan `deep copy` yang mana dijelaskan pada [Nested Object](#nested-object).
```js
let user = {
  name: "John",
  age: 30
};

let clone = Object.assign({}, user);
```

#### Use spread syntax

Spread syntax ini mirip dengan Rest Paramter yang mana dibahas dibagian [Advanced Working with Function]()

### Nested Object

Nested object tidak bisa di-clone seperti pada bagian cloning [ini](#cloning-with-assign-method).
Misalnya:
```js
let user = {
  name: "John",
  sizes: {
    height: 182,
    width: 50
  }
};

let clone = Object.assign({}, user);

alert( user.sizes === clone.sizes ); // true, same object

// user and clone share sizes
user.sizes.width++;       // change a property from one place
alert(clone.sizes.width); // 51, see the result from the other one
```

Solusi nya adalah, dengan menggunakan nested looping clone, sehingga _deep clone_ (Kalo di C++ disebut ***deep copy***) bisa terjadi. Deep clone ini bisa ditulis manual di code nya, atau bisa juga dengan menggunakan [_.cloneDeep(obj)](https://lodash.com/docs#cloneDeep) dari librari [loadsh](https://lodash.com/).

### `const` object can be modified

```js
const user = {
  name: "John"
};

const server = {
    name: "google"
}

let adm = {
    root: true
};

user.name = "Pete";         // OK
// user = server;           // Error
// user = adm;              // Error
user.name = server.name;    // OK
user.name = adm.root;       // OK

console.log(user.name);         // true
console.log(typeof user.name);  // boolean
```
> Tipe `user.name` berubah dari `string` ke `bool`.


## [Garbage Collection](https://javascript.info/garbage-collection)

Garbage Collection ini intinya adalah ***Reachability Concept***. Jika suatu variable atau object tidak ada yang dapat me-*reach* nya, maka variable atau object tersebut akan di hapus.

> Anggap setiap key pada suatu object itu seperti suatu pointer. (Memang, key ini di hash tabel selalu merepresentasikan suatu pointer pada suatu memori dari hasil perhitungan hash map).

Bahasa C++ nya: Jika suatu chuck of memory tidak ada yang me-*point* nya (Tidak ada pointer program yang menunjuknya, dan tidak ada pointer system program (pada local variable kan meskipun ngga ada code pointer, tapi kan sistem melakukan pointer pada setiap local variable didalamnya kan yaaa)), maka terjadilah ***leak memory***.

Javascript mempunyai *Garbage Collection*, sehingga *leak memory* ini bisa teratasi.


## [Object method and `this` operator](https://javascript.info/object-methods)

### Method

```js
user = {
    name: "John",           // property (js)
    sayHi: function() {     // method (js)
        alert("Hello");
    },
    sayAmigos() {           // method (js) in shorthand
        alert("Amigos");
    }
};

user.sayWorld = function() {    // add sayWorld to the user object
  alert("World!");
};

function sayCool() {
  alert("Cool!");
};
user.sayCooloCoolo = sayCool;   // add sayCooloCoolo to the user object

user.sayHai();          // calling sayHai method
user.sayAmigos();       // calling sayAmigos method
user.sayWorld();        // calling sayWorld method
user.sayCooloCoolo();   // calling sayCooloCoolo method
```

### `this` operator

```js
let user = {
  name: "John",
  age: 30,

  sayHi() {
    alert(this.name); // "this" is the "current object"
  }

};

user.sayHi(); // John
```

#### `this` is not bound

I think it's a bad habit to use `this` in out of the bound (out of the method class itself).

```js
let user = { name: "John" };
let admin = { name: "Admin" };

function sayHi() {
  alert( this.name );
}

// use the same function in two objects
user.f = sayHi;
admin.f = sayHi;

// these calls have different this
// "this" inside the function is the object "before the dot"
user.f(); // John  (this == user)
admin.f(); // Admin  (this == admin)

admin['f'](); // Admin (dot or square brackets access the method – doesn't matter)
```

#### Arrow functions do not have `this` operator

```js
let user = {
    firstName: "Ilya",
    sayHi() {
        let arrow = () => alert(this.firstName);    // OK
        arrow();
    },
};

user.sayHi(); // Ilya
```
`this` pada arrow function diatas merupakan `this` dari `sayHi` method, sehingga dalam kasus diatas tersebut arrow function bisa menggunakan `this` operator.

### There are also really good exercise

Take a look at [here](https://javascript.info/object-methods#tasks).


## [Constructor Function and `new` operator](https://javascript.info/constructor-new#create-new-accumulator)

Ctor function sebenernya yaaa cuman function pada umumnya, tapi fungsi nya untuk men-construct suatu object.
Ini kayak `class` tapi lebih simple.

> Belum baca baca lebih tentang `class` di js itu seperti apa, tapi secara kasat mata, `class` di js itu rada mirip kayak `class` di C++.

```js
function User(name) {       // function ctor
  this.name = name;
  this.isAdmin = false;
}

let user = new User("Jack");

alert(user.name); // Jack
alert(user.isAdmin); // false
```

Ctor function yang langsung di-assign kan ke suatu object:
```js
// create a function and immediately call it with new
let user = new function() {
    this.name = "John";
    this.isAdmin = false;

    // ...other code for user creation
    // maybe complex logic and statements
    // local variables etc
};
```

### Ctor `new` keyword test

Intinya, ini mengetest kalo ada misalnya ada object yang di-assign ke function ctor tanpa `new` operator. Cukup menarik, cuman kayaknya daripada pakek ctor func, mungkin mendingan pakek `class` ... Yaa mungkin begitu. Baca - baca aja [disini](https://javascript.info/constructor-new#constructor-mode-test-new-target).

### `return` from constructor

Ini cukup aneh, constructor kok bisa me-return seseuatu ???. Kata si penulis juga `return` pada ctor ini cukup jarang sekali digunakan.

### Omitting parentheses

Omitting parentheses ini hanya bisa dilakukan jika ctor function tidak punya argumen.
```js
let user = new User; // <-- no parentheses
// same as
let user = new User();
```

### Method in Constructor Function

```js
function User(name) {
  this.name = name;

  this.sayHi = function() {
    alert( "My name is: " + this.name );
  };
}

let john = new User("John");

john.sayHi(); // My name is: John

/*
john = {
   name: "John",
   sayHi: function() { ... }
}
*/
```


## [Optional Chaining `.?`](https://javascript.info/optional-chaining)

The optional chaining ?. syntax has three forms:

- `obj?.prop` – returns `obj.prop` if `obj` exists, otherwise undefined.
    ```js
    let user = null;
    alert( user?.address ); // undefined
    alert( user?.address.street ); // undefined
    ```
- `obj?.[prop]` – returns `obj[prop]` if `obj` exists, otherwise undefined.
    ```js
    let key = "firstName";

    let user1 = {
    firstName: "John"
    };

    let user2 = null;

    alert( user1?.[key] ); // John
    alert( user2?.[key] ); // undefined
    ```
- `obj.method?.()` – calls `obj.method()` if `obj.method` exists, otherwise returns undefined.
    ```js
    let userAdmin = {
        admin() {
            alert("I am admin");
        }
    };

    let userGuest = {};

    userAdmin.admin?.(); // I am admin
    userGuest.admin?.(); // nothing (no such method)
    ```

The `delete` also work in this:
```js
delete user?.name; // delete user.name if user exists
```

Optional chaining `?.` can NOT be used as _lvalue_:
```js
let user = null;

user?.name = "John"; // Error, doesn't work
// because it evaluates to undefined = "John"
```

As we can see, all of them are straightforward and simple to use. The ?. checks the left part for null/undefined and allows the evaluation to proceed if it’s not so.

A chain of `?.` allows to safely access nested properties:
```js
let user = {}; // user has no address
alert( user?.address?.street ); // undefined (no error)
```

Still, we should apply `?.` carefully, only where it’s acceptable that the left part doesn’t exist. So that it won’t hide programming errors from us, if they occur.


## [Symbol type](https://javascript.info/symbol)

Object _key_ hanya boleh _symbol_ atau _string_.

```js
// ..+ id is a new symbol (Tanpa deskripsi) +..
let id = Symbol();

// ..+ id is a symbol (Dengan description "id") +..
let id = Symbol("id");

// ..+
let id = Symbol("id");

let user = {
  name: "John",
  [id]: 123,
};

// ..+ The are no way the object can have the same value + ..
let id1 = Symbol("id");
let id2 = Symbol("id");

alert(id1 == id2); // false

// ..+ 
let id = Symbol("id");
alert(id); // TypeError: Cannot convert a Symbol value to a string

// ..+
let id = Symbol("id");
alert(id.toString()); // Symbol(id), now it works

// ..+
let id = Symbol("id");
alert(id.description); // id
```

### Hidden Properties

Symbol memiliki sifat _hidden_ jika digunakan sebagai _properties_ pada suatu object.
Sifat hidden ini bertujuan agar, misalnya:

- Si A ingin menambahkan suatu properties pada suatu object javascript librari. Namun karena object librari tersebut cukup kompleks dan Si A tidak ingin berurusan dengan code internal dari object tersebut, maka Si A dapat menggunakan _symbol_ sebagai properties pada object librari tersebut. Hal ini bertujuan agar menghindari _overwrite_ nama properties pada object tersebut.
- Si B dan Si C, bekerjasama membuat suatu object. Keduanya mengetes dan menambahkan fitur - fitur pada object tersebut. Si B dan Si C menambahkan properties bernama `id`. Dalam branch masing - masing, keduanya mengetes dan menambahkan method lainnya yang berhubungan dengan `id` tersebut. Ketika mereka berdua merasa method tentang `id` sudah cukup, maka mereka melakukan _merging_ branch. Agar `id` disini tidak konflik, mereka menggunakan _symbol_. Setelah beberapa bulan ternyata fitur yang ada pada `id` nya Si C lebih berguna daripada `id` nya Si B. Si `B` pun menghapus `id` nya tersebut, kemudian Si C menambahkan nama `"id"` (bukan `id` symbol) sebagai properti resmi dari object yang dikembangkan oleh Si B dan Si C tersebut. Jika Si B dan Si C pada awalnya tidak menggunakan `id` sebagai symbol, maka yang ternjadi adalah:
  ```js
  let user = { name: "John" };

  // Si B script uses "id" property
  user.id = "Si B id value";

  // ...Another script also wants "id" for its purposes...

  user.id = "Si C id value"
  // Boom! overwritten by another script!
  ```

#### Symbol are skipped by for..in

```js
let id = Symbol("id");
let user = {
  name: "John",
  age: 30,
  [id]: 123
};

for (let key in user) alert(key); // name, age (NO SYMBOL)

// the direct access by the symbol works
alert( "Direct: " + user[id] );
```

#### `object.assign()`

```js
let id = Symbol("id");
let user = {
  [id]: 123
};

let clone = Object.assign({}, user);  // It's Also COPYING SYMBOL

alert( clone[id] ); // 123
```

### Global Symbol

Global symbol ini kayak symbol yang pakek hash map yang sama:
```js
// read from the global registry
let id = Symbol.for("id"); // if the symbol did not exist, it is created

// read it again (maybe from another part of the code)
let idAgain = Symbol.for("id");

// the same symbol
alert( id === idAgain ); // true
```

#### `Symbol.keyFor()`

```js
// get symbol by name
let sym = Symbol.for("name");   // global symbol
let sym2 = Symbol.for("id");    // global symbol
let sym3 = Symbol("address");   // local symbol

// get name by symbol
alert( Symbol.keyFor(sym) );    // name
alert( Symbol.keyFor(sym2) );   // id
alert( Symbol.keyFor(sym3) );   // undefined
```

### System Symbol

There are many system symbols used by JavaScript which are accessible as `Symbol.*`. We can use them to alter some built-in behaviors. For instance, later in the tutorial we’ll use `Symbol.iterator` for [iterables](https://javascript.info/iterable), `Symbol.toPrimitive` to setup [object-to-primitive conversion](https://javascript.info/object-toprimitive) and so on.

### Is Symbol really hidden ?

Technically, symbols are not 100% hidden. There is a built-in method `Object.getOwnPropertySymbols(obj)` that allows us to get all symbols. Also there is a method named `Reflect.ownKeys(obj)` that returns all keys of an object including symbolic ones. So they are not really hidden. _But most libraries, built-in functions and syntax constructs don’t use these methods_.


## [Object to Primitive conversion](https://javascript.info/object-toprimitive)

Kebanyakan fungsi output seperti `alert` dan operator math, concatenate, dll. hanya bekerja pada _primitive type_.
Sehingga, jika _object_ dimasukkan ke fungsi fungsi dan operator tersebut, maka diperlukan suatu fungsi khusus untuk memberitahu javascript bagaimana cara memperlakukan _object_ ini jika dimasukkan ke fungsi atau operator yang ada.

> Kalo di C++, ini kayak kita ngebuat _operator overloading_. Cuman operator overloading ini ditujukan untuk object type, bukan class.

There are 3 types (hints) of it:
- `"string"` (for alert and other operations that need a string)
- `"number"` (for maths)
- `"default"` (few operators)

The conversion algorithm is:

1. Call `obj[Symbol.toPrimitive](hint)` if the method exists.
2. Otherwise if hint is `"string"`
    - try `obj.toString()` and `obj.valueOf()`, whatever exists.
3. Otherwise if hint is `"number"` or `"default"`
    - try `obj.valueOf()` and `obj.toString()`, whatever exists.

`obj[Symbol.toPrimitive](hint)` demo:
```js
let user = {
    name: "John",
    money: 1000,

    [Symbol.toPrimitive](hint) {
    alert(`hint: ${hint}`);
    return hint == "string" ? `{name: "${this.name}"}` : this.money;
    }
};

// conversions demo:
alert(user); // hint: string -> {name: "John"}
alert(+user); // hint: number -> 1000
alert(user + 500); // hint: default -> 1500
```

`obj.toString() and `obj.valueOf()` demo:
```js
let user = {
  name: "John",
  money: 1000,

  // for hint="string"
  toString() {
    return `{name: "${this.name}"}`;
  },

  // for hint="number" or "default"
  valueOf() {
    return this.money;
  }

};

alert(user); // toString -> {name: "John"}
alert(+user); // valueOf -> 1000
alert(user + 500); // valueOf -> 1500
```

Pada kebanyakan kasus, sebenernya ngimplementasiin `obj.toString()` aja udah cukup:
```js
let user = {
  name: "John",

  toString() {
    return this.name;
  }
};

alert(user); // toString -> John
alert(user + 500); // toString -> John500
```

Further Conversion:
```js
let obj = {
  // toString handles all conversions in the absence of other methods
  toString() {
    return "2";
  }
};

alert(obj + 2); // 22 ("2" + 2), conversion to primitive returned a string => concatenation
alert(obj * 2); // 4, object converted to primitive "2", then multiplication made it a number
```

