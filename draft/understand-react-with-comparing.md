---
title: Reactと厳密等価で、Reactの理解を深める
tags:
  - 'React'
  - '初心者'
private: false
updated_at: ''
id: null
organization_url_name: null
slide: false
---

- [押さえておきたいこと](#押さえておきたいこと)
  - [型 (Data types)](#型-data-types)
  - [プリミティブ型の厳密比較](#プリミティブ型の厳密比較)
  - [オブジェクトの厳密比較](#オブジェクトの厳密比較)
  - [オブジェクトの複製](#オブジェクトの複製)
- [これらの知識が必要な理由](#これらの知識が必要な理由)
  - [React.useState](#reactusestate)
  - [その他hook](#その他hook)
- [リンク](#リンク)

## 押さえておきたいこと

### [型](https://developer.mozilla.org/ja/docs/Web/JavaScript/Data_structures) (Data types)

```js

// プリミティブ型
const a = 1; // Number
const b = '1'; // String
const c = true; // Boolean
const d = null; // Null
const e = undefined; // Undefined
const f = Symbol(); // Symbol

// オブジェクト型
const g = {}; // Object
const h = []; // Array
const i = function() {}; // Function
```

### プリミティブ型の厳密比較

プリミティブ型の厳密比較は、ある比較対象が同じ型で、同じ値を示しているかどうかを判定します。

```js
const a = 1; // Number
const b = 1; // Number

console.log(a === b);
// => true
```

```js
const a = 1; // Number
const b = '1'; // String

console.log(a === b);
// => false
```

```js
const a = true; // Boolean

console.log(a === true);
// => true
```

```js
const a = null; // Null

console.log(a === null);
// => true
```

```js
const a = undefined; // Undefined

console.log(a === undefined);
// => true
```

```js
const a = Symbol(); // Symbol
const b = Symbol(); // Symbol

console.log(a === b);
// => false
```





### オブジェクトの厳密比較

オブジェクトの厳密比較は、プリミティブ型の厳密比較とは異なります。
**オブジェクトの参照**が等価であるかどうかを判定します。


```js
const a = {};
const b = {};

console.log(a === b);
// => false
```

```js
const a = [];
const b = [];

console.log(a === b);
// => false
```

```js
const a = function() {};
const b = function() {};

console.log(a === b);
// => false
```

以下の場合は、オブジェクトの参照が等価であるため、`true`となります。

```js
const a = {};
const b = a;

console.log(a === b);
// => true
```

```js
const a = [];
const b = a;

console.log(a === b);
// => true
```

```js
const a = function() {};
const b = a;

console.log(a === b);
// => true
```

オブジェクトの持つプロパティを変更したとき、オブジェクトの参照は変わりません。

```js
const a = { value: 1 };
const b = a;

b.value = 2;

console.log(a === b);
// => true

// a.valueとb.valueは同じオブジェクトを参照しているため、同じ値を示します。
console.log(a.value, b.value);
// => 2 2
```

### [オブジェクトの複製](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)

1. 異なるオブジェクトを参照するようにするには、新しいオブジェクトを作成する必要があります。

```js
const a = { value: 1 };

// スプレッド構文を使うと、新しいオブジェクトを作成できます。
const b = { ...a };

b.value = 2;

console.log(a === b);
// => false

// a.valueとb.valueは異なるオブジェクトを参照しているため、異なる値を示します。
console.log(a.value, b.value);
// => 1 2
```

配列、関数についても同様です。

```js
const a = [1, 2, 3];
const b = [...a];

b[0] = 4;

console.log(a === b);
// => false

// a[0]とb[0]は異なるオブジェクトを参照しているため、異なる値を示します。
console.log(a[0], b[0]);
// => 1 4
```

```js
const a = function() {};
const b = { ...a };

console.log(a === b);
// => false
```


オブジェクトのプロパティがオブジェクトの場合、シャローコピーでは、オブジェクトの参照がコピーされるため、同じオブジェクトを参照することになります。

```js
const a = { value: { value: 1 } };
const b = { ...a };

b.value.value = 2;

console.log(a === b);
// => false

// a.valueとb.valueは同じオブジェクトを参照しているため、同じ値を示します。
console.log(a.value === b.value);
// => true
console.log(a.value.value, b.value.value);
// => 2 2
```

この場合、オブジェクトのプロパティも新しいオブジェクトを作成する必要があります。

```js
const a = { value: { value: 1 } };

// スプレッド構文を使うと、新しいオブジェクトを作成できます。
const b = { ...a, value: { ...a.value } };

b.value.value = 2;

console.log(a === b);
// => false

// a.valueとb.valueは異なるオブジェクトを参照しているため、異なる値を示します。
console.log(a.value === b.value);
// => false
console.log(a.value.value, b.value.value);
// => 1 2
```

この例ではオブジェクトの要素において、オブジェクトのプロパティがオブジェクトの場合に手間がかかります。
この問題を解決するための工夫として、`JSON.stringify()`と`JSON.parse()`を使う方法があります。

```js

const a = { value: { value: 1 } };

// JSON.stringify()を使うと、オブジェクトをJson形式の文字列に変換できます。
// JSON.parse()を使うと、Json形式の文字列をオブジェクトに変換できます。

const b = JSON.parse(JSON.stringify(a));

b.value.value = 2;

console.log(a === b);
// => false

// a.valueとb.valueは異なるオブジェクトを参照しているため、異なる値を示します。
console.log(a.value === b.value);
// => false
console.log(a.value.value, b.value.value);
// => 1 2
```


## これらの知識が必要な理由

### React.useState

例えばReactには`useState`という関数があります。

```js
const [count, setCount] = useState(0);
```

この例の`setCount`は、`count`の値を更新するための関数です。
引数には、`count`に設定したい値を渡します。

```js
setCount(1);
```

引数に渡した値が関数だったとき、その関数の引数には、現在の値が渡されます。
関数の戻り値が、更新後の`count`の値となります。

```js
setCount((prevCount) => prevCount + 1);
```

正確には、**`setCount`に渡した値が現在の値と等価でないときに**、Reactは`count`の値を更新し、コンポーネントを再描画します。


大げさに書くと、`setCount`は以下のような処理をしています。



- `count`が`3`のときに、`3`に設定しようとするとき

  ```js
  console.log(count);
  // => 3

  // setCount(3); の誇張表現
  setCount((prevCount) => {

    console.log(prevCount);
    // => 3

    const nextCount = 3;
    console.log(nextCount === prevCount);
    // => true (prevCountとnextCountは等価のため、countの値は更新されない)

    return nextCount;
  });

  console.log(count);
  // => 3
  ```

- `count`が`3`のときに、`4`に設定しようとするとき

  ```js
  console.log(count);
  // => 3

  // setCount(4); の誇張表現
  setCount((prevCount) => {

    console.log(prevCount);
    // => 3

    const nextCount = 4;
    console.log(nextCount === prevCount);
    // => false (prevCountとnextCountは等価ではないため、countの値は更新される)

    return nextCount;
  });

  console.log(count);
  // => 4
  ```

`count`の値の変化を検知するために、**厳密等価 (===)** が使われています。



この考え方は`count`が`Object`だった場合にも同様です。
そのため、`setCount`の引数にコールバック関数を渡すときは注意が必要です。

:::note warn
  `prevCount`の値を更新した

  ```js
  setCount((prevCount) => {
    const nextCount = prevCount;
    nextCount.value = prevCount.value + 1;

    console.log(nextCount === prevCount);
    // => true (prevCountとnextCountは等価のため、countの値は更新されない)

    return nextCount;
  });
  ```

:::

:::note
  **`prevCount`のコピーを作成し**、その値を更新した

  ```diff
  setCount((prevCount) => {
-    const nextCount = prevCount;
+    const nextCount = { ...prevCount };
    nextCount.value = prevCount.value + 1;

    console.log(nextCount === prevCount);
    // => false (prevCountとnextCountは等価ではないため、countの値は更新される)

    return nextCount;
  });
  ```

:::


Reactでは`Array.prototype.map()`や`スプレッド構文`などがよく使われます。
その理由は、元のオブジェクトを変更せずに、**新しいオブジェクト**を作成できるためです。

```js
const [count, setCount] = useState([1, 2, 3, 4, 5]);
```

```js
setCount((prevCount) => {
  const nextCount = prevCount.map((item) => item * 2);

  console.log(nextCount === prevCount);
  // => false (prevCountとnextCountは等価ではないため、countの値は更新される)

  return nextCount;
});
```

```js
setCount((prevCount) => {
  const nextCount = [...prevCount, 6];

  console.log(nextCount === prevCount);
  // => false (prevCountとnextCountは等価ではないため、countの値は更新される)

  return nextCount;
});
```

### その他hook

Reactには、`useMemo`、`useCallback`、`useEffect`という関数があります。

これらに共通することは、第2引数に渡した値が変化したとき、つまり、**厳密等価 (===)** で`false`となったときに、第1引数の関数が実行されるということです。

これらの関数を適切に扱うためにも、**厳密等価 (===)** について理解しておく必要があります。

以下に示す例は、`count`の値が変化したときに、第1引数に渡した処理が実行されます。

```js
const [count, setCount] = useState(0);
setCount((prevCount) => prevCount + 1);
```

```js
useEffect(() => {
  console.log(`${count}に変化しました。`);

  return () => {
    console.log(`${count}から、別の値に変化しました。`);
  };

}, [count]);
```

```js
const result = useMemo(() => { return count * 2; }, [count]);
console.log(result);
// => countを2倍した値
```

```js
const result = useCallback(() => { return count * 2; }, [count]);
console.log(result());
// => countを2倍した値
```

## リンク

- [厳密等価演算子 - JavaScript | MDN](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Operators/Strict_equality)
- [useState - React](https://ja.react.dev/reference/react/useState)
