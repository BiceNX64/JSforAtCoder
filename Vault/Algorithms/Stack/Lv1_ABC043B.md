この記事では、[ABC043 **B - バイナリハックイージー** ](https://atcoder.jp/contests/abc043/tasks/abc043_b)を解説していきます。 
この問題は、スタックを使った典型的な問題で、スタックがなにか理解しやすいと思います。

## 📚 スタックとは、

一言で言えば**「最後に入れたものが、最初に出てくる」というルールを持ったデータの保管場所のこと**です。
 専門用語では LIFO（Last-In, First-Out）と呼びます。 たとえば、何枚も重なったお皿を想像してみてください。

下の方にあるお皿を取り出そうと思ったら、まず上にあるものを全部どかさなければなりませんよね。 
つまり、**常に「一番新しいデータ」にしか触れられないという制約がある構造**なんです。

スタックでは、`push()`と`pop()`という2つの配列のメソッドが良く使われます。 先程のお皿の例でいえば、

- **push**: 新しいお皿を一番上に積み上げること(配列のいちばん最後に新しい要素を付け加えること)
- **pop** : 一番上にあるお皿を一枚手に取ること(配列のいちばん最後の要素を削除すること)

となります。この2つを使って、スタックの配列のいちばん最後の要素を管理していくことがポイントになります。

まずは、今回の問題をみていきましょう。

## ❓ 考え方

(※ 問題の詳細は、以下のリンクからご確認ください。 [ABC043 B - バイナリハックイージー 問題文](https://atcoder.jp/contests/abc043/tasks/abc043_b))

ルールはシンプルで、3つのキーを与えられた順番に押したあとにどのような文字列になっているか？です。 そして、3つのキーの挙動は以下のとおり。

```
0 キー: 文字列の右端に文字 0 が挿入される。
1 キー: 文字列の右端に文字 1 が挿入される。
バックスペースキー: 文字列が空なら、何も起こらない。そうでなければ、文字列の右端の 1 文字が削除される。
```

これをスタックの考え方を適用すると、

- 0 キー or 1 キー　→ `push()`
- バックスペースキー(文字列が空ではない場合)　→ `pop()`

となることがわかります。

### 💻 コード例と解説

```javascript
"use strict";
const fs = require('fs');

function Main(input) {
    const args = input.trim().split(/\s+/);
    let cur = 0;
    const next = () => args[cur++];
    const nextNum = () => Number(next());
    const nextBigInt = () => BigInt(next());
    // const lines = input.replace(/\r/g, '').trim().split(/\n/);

    // 👇️ Logic here 👇️

    const s = next().split("");
    const stack = [];
    const len = s.length;
    for (let i = 0; i < len; i++) {
        const x = s[i];
        if (x === "0" || x === "1") {
            stack.push(x);
        } else if (x === 'B' && stack.length > 0) {
            stack.pop();
        }
    }
    const ans = stack.join("");
    console.log(ans);

    // 👆️ Logic here 👆️
}

Main(require("fs").readFileSync(process.platform === 'linux' ? "/dev/stdin" : process.argv[2], "utf8"));
```

テンプレートの細かい解説は、[JS_for_ABC-template_v1.0.md](../../../Basement/JS_for_ABC-template_v1.0.md)を参照して、👇️ Logic here 👇️のあいだの部分に注目してください。

まず、取り込んだ入力を整形し、必要な変数を用意します(`stack`と`len`)

```javascript
    const s = next().split("");
    const stack = [];
    const len = s.length;
```

それからスタックの処理を書いていきます。

```javascript
    for (let i = 0; i < len; i++) {
        const x = s[i];
        if (x === "0" || x === "1") {
            stack.push(x);
        } else if (x === 'B' && stack.length > 0) {
            stack.pop();
        }
    }
```

まずfor文で与えられた文字列の長さでループして、与えられた文字列について1文字ずつスタックの処理で分岐させていきます。 
数値の `0` と文字列の `"0"` は厳密には違うものとして扱われるため、 `===` で比較するときは特に注意が必要です

文字が"0"または"1"の場合は、配列`stack`に`push`して右端に"0"または"1"を加えていきます。
文字が"B"(バックスペース)かつ`stack`が空ではない(`stack.length>0`)場合、`stack.pop()`で右端の文字を削除します。 
文字が"B"(バックスペース)であっても`stack`が空の場合はなにも処理をしません。

この処理のループが終わったときに配列`stack`が必要な文字を持っているので最後につなげて出力します。

```javascript
    const ans = stack.join("");
    console.log(ans);
```

## ✒️ まとめ

- スタック＝「最後に入れたものが、最初に出てくる」というルールを持ったデータの構造のこと。
- イメージは積み重ねられたお皿。いちばん上のお皿しか操作できない。
- `push()`と`pop()`という2つの配列のメソッドを使うことで管理していく。



---



In this article, we will explain [ABC043 **B - Unhappy Hacking (ABC Edit)**](https://atcoder.jp/contests/abc043/tasks/abc043_b). This problem is a classic example of using a "stack," making it easy to understand how this data structure works.

## 📚 What is a Stack?

Simply put, a stack is a **data storage location that follows the rule: "the last thing put in is the first thing that comes out."** In technical terms, this is called LIFO (Last-In, First-Out). For example, imagine a stack of plates.

If you want to take out a plate from the bottom, you must first remove all the plates on top of it. In other words, it is a **structure with a constraint where you can only access the most recent data.**

Two array methods, `push()` and `pop()`, are frequently used with stacks. Using the plate analogy:

- **push**: Adding a new plate to the top (adding a new element to the end of an array).
- **pop**: Taking the top plate off (removing the last element from an array).

The key is to manage the very last element of the array using these two methods.

Let's look at the problem.

## ❓ How to solve
(Please refer to the problem statement at the following link: [ABC043 B - Unhappy Hacking (ABC Edit)](https://atcoder.jp/contests/abc043/tasks/abc043_b))

The rules are simple: what string do you end up with after pressing three keys in the given order? The behaviors of the three keys are as follows:

```
0 Key: Appends the character '0' to the right end of the string.
1 Key: Appends the character '1' to the right end of the string.
Backspace Key: If the string is empty, nothing happens. Otherwise, removes the rightmost character.
```

Applying the concept of a stack here:

- 0 Key or 1 Key → `push()`
- Backspace Key (if the string is not empty) → `pop()`

### 💻 Code Example and Explanation

```javascript
"use strict";
const fs = require('fs');

function Main(input) {
    const args = input.trim().split(/\s+/);
    let cur = 0;
    const next = () => args[cur++];

    // 👇️ Logic here 👇️

    const s = next().split("");
    const stack = [];
    const len = s.length;
    for (let i = 0; i < len; i++) {
        const x = s[i];
        if (x === "0" || x === "1") {
            stack.push(x);
        } else if (x === 'B' && stack.length > 0) {
            stack.pop();
        }
    }
    const ans = stack.join("");
    console.log(ans);

    // 👆️ Logic here 👆️
}

Main(require("fs").readFileSync(process.platform === 'linux' ? "/dev/stdin" : process.argv[2], "utf8"));
```

For a detailed explanation of the template, please refer to [JS_for_ABC-template_v1.0.md](../../../Basement/JS_for_ABC-template_v1.0.md) and focus on the section between `👇️ Logic here 👇️`.

First, we format the input and prepare the necessary variables (`stack` and `len`).

```javascript
    const s = next().split("");
    const stack = [];
    const len = s.length;
```

Then, we implement the stack logic.

```javascript
    for (let i = 0; i < len; i++) {
        const x = s[i];
        if (x === "0" || x === "1") {
            stack.push(x);
        } else if (x === 'B' && stack.length > 0) {
            stack.pop();
        }
    }
```

We use a `for` loop to iterate through the given string and branch the logic for each character. Note that the number `0` and the string `"0"` are treated differently in JavaScript, so be careful with strict equality `===`.

- If the character is "0" or "1", we `push` it into the `stack` array.
- If the character is "B" (Backspace) and the `stack` is not empty (`stack.length > 0`), we use `stack.pop()` to remove the rightmost character.
- If it is "B" but the `stack` is empty, no action is taken.

After the loop, the `stack` array contains the final string, so we join it and print the output.

```javascript
    const ans = stack.join("");
    console.log(ans);
```

## ✒️ Summary

- A stack is a data structure following the "Last-In, First-Out" rule.
- Think of it as a stack of plates; you can only interact with the top one.
- It is managed using the `push()` and `pop()` array methods.
