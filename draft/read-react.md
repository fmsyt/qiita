---
title: 他の人の書いたReactのコードを読むために必要なJavaScriptの知識
tags:
  - 'React'
  - '初心者'
private: false
updated_at: ''
id: null
organization_url_name: null
slide: false
---

- [Objectの操作](#objectの操作)
  - [基本操作](#基本操作)
  - [オブジェクトのプロパティを追加](#オブジェクトのプロパティを追加)
  - [オブジェクトのプロパティを削除](#オブジェクトのプロパティを削除)
  - [オブジェクトのプロパティを更新](#オブジェクトのプロパティを更新)
  - [オブジェクトのプロパティを取得](#オブジェクトのプロパティを取得)
  - [オブジェクトの操作 (スプレッド構文)](#オブジェクトの操作-スプレッド構文)
  - [オブジェクトの複製](#オブジェクトの複製)
  - [オブジェクトのマージ](#オブジェクトのマージ)
- [配列の操作](#配列の操作)
  - [基本操作](#基本操作-1)
  - [配列の要素を追加](#配列の要素を追加)
  - [配列の要素を削除](#配列の要素を削除)
  - [配列の要素を更新](#配列の要素を更新)
  - [配列の要素を取得](#配列の要素を取得)
  - [配列の要素をマージ](#配列の要素をマージ)
  - [配列の複製](#配列の複製)
- [Reactでよく使われるJavaScriptの構文](#reactでよく使われるjavascriptの構文)
  - [アロー関数 (Arrow functions)](#アロー関数-arrow-functions)
  - [Array.prototype.map()](#arrayprototypemap)
  - [オプショナルチェーン (Optional Chaining)](#オプショナルチェーン-optional-chaining)
  - [三項演算子 (Conditional Operator)](#三項演算子-conditional-operator)
  - [Null合体演算子 (Nullish Coalescing Operator)](#null合体演算子-nullish-coalescing-operator)
  - [短絡評価 (AND) (Short-circuit evaluation)](#短絡評価-and-short-circuit-evaluation)
  - [短絡評価 (OR) (Short-circuit evaluation)](#短絡評価-or-short-circuit-evaluation)
  - [分割代入 (Destructuring assignment)](#分割代入-destructuring-assignment)
  - [テンプレートリテラル (Template literals)](#テンプレートリテラル-template-literals)

## Objectの操作

### 基本操作

```js
const obj = {
  a: 1,
  b: 2,
  c: 3,
};

console.log(obj.a);
// => 1
console.log(obj['a']);
// => 1

const key = 'a';
console.log(obj[key]);
// => 1
```



### オブジェクトのプロパティを追加

```js
obj.d = 4;
obj['e'] = 5;

console.log(obj);
// => { a: 1, b: 2, c: 3, d: 4, e: 5 }
```

### オブジェクトのプロパティを削除

```js
delete obj.a;
delete obj['b'];

console.log(obj);
// => { c: 3 }
```

### オブジェクトのプロパティを更新

```js
obj.c = 10;
obj['d'] = 20;

console.log(obj);
// => { c: 10, d: 20 }

```

### オブジェクトのプロパティを取得

```js
// キーを取得
const keys = Object.keys(obj);
console.log(keys);
// => ['c', 'd']

// 値を取得
const values = Object.values(obj);
console.log(values);
// => [10, 20]

// キーと値を取得
const entries = Object.entries(obj);
console.log(entries);

// => [
//   ['c', 10],
//   ['d', 20],
// ]
```

### オブジェクトの操作 (スプレッド構文)

```js
const obj1 = {
  a: 1,
  b: 2,
};

const obj2 = {
  c: 3,
  d: 4,
};

const mergedObj = {
  ...obj1,
  ...obj2,
};

console.log(mergedObj);
// => { a: 1, b: 2, c: 3, d: 4 }
```



### オブジェクトの複製

```js
const clonedObj = { ...obj };

console.log(clonedObj);
// => { c: 10, d: 20 }

// このとき、objとclonedObjは別のオブジェクトとなる
console.log(obj === clonedObj);
// => false
```

### オブジェクトのマージ

```js
// オブジェクトのプロパティをマージ (重複するプロパティは後のオブジェクトのプロパティで上書きされる)
const obj1 = {
  a: 1,
  b: 2,
};

const obj2 = {
  b: 3,
  c: 4,
};

const overwrittenObj = {
  ...obj1,
  ...obj2,
};

console.log(overwrittenObj);
// => { a: 1, b: 3, c: 4 }
```

## 配列の操作

### 基本操作

```js
const array = [1, 2, 3, 4, 5];

console.log(array[0]);
// => 1

console.log(array[1]);
// => 2

console.log(array.length);
// => 5
```

### 配列の要素を追加

```js
const array = [1, 2, 3, 4, 5];

// 配列の要素を追加
array.push(6);
array[6] = 7;

console.log(array);
// => [1, 2, 3, 4, 5, 6, 7]
```

### 配列の要素を削除

```js
const array = [1, 2, 3, 4, 5];

// 配列の最初の要素を削除 (array[0]から、1つの要素を削除)
array.splice(0, 1);

// 配列の末尾の要素を削除
array.pop();

console.log(array);
// => [2, 3, 4]
```

### 配列の要素を更新

```js
const array = [1, 2, 3, 4, 5];

array[0] = 10;
array[1] = 20;

console.log(array);
// => [10, 20, 3, 4, 5]
```

### 配列の要素を取得

```js
const array = [1, 2, 3, 4, 5];

const firstItem = array[0];
const secondItem = array[1];

console.log(firstItem);
// => 1
console.log(secondItem);
// => 2
```

### 配列の要素をマージ

```js
const array1 = [1, 2, 3];
const array2 = [4, 5, 6];

const mergedArray = [...array1, ...array2];

console.log(mergedArray);
// => [1, 2, 3, 4, 5, 6]
```

### 配列の複製

```js
const array = [1, 2, 3, 4, 5];

const clonedArray = [...array];

console.log(array);
// => [1, 2, 3, 4, 5]

console.log(clonedArray);
// => [1, 2, 3, 4, 5]

// このとき、arrayとclonedArrayは別の配列となる
console.log(array === clonedArray);
// => false
```


## Reactでよく使われるJavaScriptの構文

### [アロー関数](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Functions/Arrow_functions) (Arrow functions)

```js
const add = (arg1, arg2) => {
  return arg1 + arg2;
};

const result = add(1, 2);
console.log(result);
// => 3
```

処理によっては、以下のように`return`を省略することもできます。
これは、**アロー関数の処理が1行の場合に限り**可能です。

```js
const add = (arg1, arg2) => arg1 + arg2;

const result = add(1, 2);
console.log(result);
// => 3
```

`function`との違いは、以下のページを参照してください。

- [アロー関数 - JavaScript | MDN](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Functions/Arrow_functions)



### [Array.prototype.map()](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Array/map)

```js
const array = [1, 2, 3, 4, 5];
const newArray = array.map((item) => item * 2);

console.log(newArray);
// => [2, 4, 6, 8, 10]

// このとき、arrayとnewArrayは別の配列となる
console.log(array === newArray);
// => false
```


### [オプショナルチェーン](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Operators/Optional_chaining) (Optional Chaining)

`null`または`undefined`のから要素にアクセスしようとしたときのエラーを回避するために使います。

```js
const obj = {
  objectOrNull: null,
}

// console.log(obj.objectOrNull.value);
// => error

console.log(obj.objectOrNull?.value);
// => undefined
```

### [三項演算子](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Operators/Conditional_Operator) (Conditional Operator)

```js
const a = 1;
const b = 2;

const c = a > b ? 'a は b より大きい' : 'a は b 以下';

console.log(c);
// => a は b 以下
```

これは、if文を使うと以下のように書くことができます。

```js
const a = 1;
const b = 2;

let c;
if (a > b) {
  c = 'a は b より大きい';
} else {
  c = 'a は b 以下';
}

console.log(c);
// => a は b 以下
```


### [Null合体演算子](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Operators/Nullish_coalescing) (Nullish Coalescing Operator)

```js
const numberOrNull = null;

const number = numberOrNull ?? 0;

console.log(number);
```

### [短絡評価 (AND)](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Operators/Logical_AND) (Short-circuit evaluation)

```js
const a = 1;
const b = 2;

const c = a && b;

console.log(c);
// => 2
```

これは、if文を使うと以下のように書くことができます。

```js
const a = 1;
const b = 2;

let c;
if (a) {
  c = b;
} else {
  c = a;
}

console.log(c);
// => 2
```


### [短絡評価 (OR)](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Operators/Logical_OR) (Short-circuit evaluation)

```js
const a = 1;
const b = 2;

const c = a || b;

console.log(c);
// => 1
```

これは、if文を使うと以下のように書くことができます。

```js
const a = 1;
const b = 2;

let c;
if (a) {
  c = a;
} else {
  c = b;
}

console.log(c);
// => 1
```


### [分割代入](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment) (Destructuring assignment)

```js
const obj = {
  a: 1,
  b: 2,
  c: 3,
};

const { a, b, c } = obj;

console.log(a);
// => 1
console.log(b);
// => 2
console.log(c);
// => 3
```

オブジェクトを分割代入するとき、任意の変数名を指定することもできます。

```js
const obj = {
  a: 1,
  b: 2,
  c: 3,
};

const { a: x, b: y, c: z } = obj;

console.log(x);
// => 1
console.log(y);
// => 2
console.log(z);
// => 3
```


配列に対しても同様に分割代入ができます。

```js

const array = [1, 2, 3, 4, 5];

const [a, b, c, d, e] = array;

console.log(a);
// => 1
console.log(b);
// => 2
console.log(c);
// => 3
console.log(d);
// => 4
console.log(e);
// => 5
```

配列の要素を分割代入するとき、不要な要素は`_`で省略することができます。

```js

const array = [1, 2, 3, 4, 5];

const [a, b, , , e] = array;

console.log(a);
// => 1

console.log(b);
// => 2

//console.log(c);
// => error

console.log(e);
// => 5
```

### [テンプレートリテラル](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Template_literals) (Template literals)

```js
const name = 'John';
const age = 20;

const message = `My name is ${name}. I'm ${age} years old.`;

console.log(message);
// => My name is John. I'm 20 years old.
```

オブジェクトや関数なども、テンプレートリテラルの中で展開することができます。

```js
const obj = { a: 1 };

const func = () => {
  return 'Hello';
};

const message = `obj.a: ${obj.a}, func(): ${func()}`;

console.log(message);
// => obj.a: 1, func(): Hello
```
