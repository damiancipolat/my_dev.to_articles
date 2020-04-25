Maybe you have seen this as a meme or in some JS forum.

<img src="https://github.com/damiancipolat/js_vs_memes/blob/master/doc/js_coercion.png?raw=true" align="center" width="700px"/>

I'm sure you have thought, this is impossible to happen, in a code language can this return **"true"**. It's a joke? - Not this returns **"true"** and there are many reasons for this to happen.

### Introduction:
To explain this, is necessary to understand some concepts.

- [Implicit and explicit coercion](#implicit_explicit).
- [Objects type coercion](#object_coercion).
- [Functions toString and valueOf](#functions).
- [Explanation](#explanation).

<a name="implicit_explicit"></a>
### 1)  Implicit and explicit coercion.
There are two types of coercion **explicit** and **explicit**.

#### **Type coercion**:
Is the process of converting value from one type to another (such as string to number, object to boolean, and so on). Any type, be it primitive or an object, is a valid subject for type coercion. To recall, primitives are: number, string, boolean, null, undefined + Symbol (added in ES6).

#### **Explicit type coercion**:
When a developer expresses the intention to convert between types by writing the appropriate code, like Number(value).

#### **Implicit type coercion**:
Values can also be converted between different types automatically, and it is called implicit type coercion. It usually happens when you apply operators to values of different types, like 1 == null, 2/’5', null + new Date(), or it can be triggered by the surrounding context, like with if (value) {…}, where value is coerced to boolean.

There are only three types of conversion in JavaScript:

- **to string**
- **to boolean**
- **to number**

<a name="object_coercion"></a>
### 2) Objects, type coercion.
When a JS engine encounters expression like [1] + [4,5], first it needs to convert an object to a primitive value, which is then converted to the final type. And still there are only three types of conversion: **numeric, string and boolean**.

Numeric and string conversion make use of two methods of the input object: valueOf and toString . Both methods are declared on Object.prototype and thus available for any derived types, such as Date, Array, etc.

In general the algorithm is as follows:

- If input is already a primitive, do nothing and return it.
- Call input.toString(), if the result is primitive, return it.
- Call input.valueOf(), if the result is primitive, return it.
- If neither input.toString() nor input.valueOf() yields primitive, throw TypeError.

<a name="functions"></a>
### 3) Functions toString and valueOf
Take a look of two object prototype methods.

#### **object.toString()**
The method .toString() is used when an object is coerced into a string. This is helpful when you want to get a nice string representation of an object vs. the not-so-useful "[object Object]".

```javascript
const s = {
  name: 'Damian',
  surname: 'Cipolat',
  toString: function() {
    console.log('LOG', this.name,this.surname);
    return `Fullname: ${this.name},${this.surname}`;
  }
}
console.log(s);
// { x: 7, y: 3, toString: [Function: toString] }
console.log(s + '');
// 'Square: 7,3'
console.log(+s);
// NaN
```

#### **object.valueOf()**
The method .valueOf() is used when an object is coerced into a primitive value such as a number. If you’ve ever added a number to an object and ended up with a value of NaN, then this method is your friend.

```javascript
const box = {
  l: 7,
  w: 3,
  h: 4,
  valueOf: function() {
    return this.l * this.w * this.h
  }
}
console.log(box)
// { [Number: 84] l: 7, w: 3, h: 4, valueOf: [Function: valueOf] }
console.log(String(box))
// [object Object]
console.log(+box)
// 84
```
<a name="explanation"></a>
### Explanation:
Ok, it's time to discover the magic, why a==1&&a==2&&a==3 is **"true"**? To make this expression success, we can override the function **toString**, this function is used in the implicit type coercion of the operator **"=="**.

This is the full code:
```javascript
const a = {
  tmp: 1,
  toString: function () {
    console.log('TO STRING');
    return a.tmp++;
  }
}

if(a == 1 && a == 2 && a == 3) {
  console.log('JS Magic!');
}
```

So the reason here is the toString function is called 3 times and the **TO STRING** appears in the console output.

<img src="https://github.com/damiancipolat/js_vs_memes/blob/master/doc/console.png?raw=true" align="center" width="700px"/>

### Readings
Some interesting links to continue reading.
- https://dorey.github.io/JavaScript-Equality-Table/
- https://wtfjs.com/
- https://medium.com/intrinsic/javascript-object-type-coercion-b2ec176c02c4
- https://www.freecodecamp.org/news/js-type-coercion-explained-27ba3d9a2839
- https://developer.mozilla.org/es/docs/Web/JavaScript/Guide/Trabajando_con_objectos