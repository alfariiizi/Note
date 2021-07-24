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


