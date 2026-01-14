**Scroll down for English!**

# ✒️ AtCoder ABC用テンプレート

### ✨ このテンプレートの良いところ

1. 提出とテストケースをひとつのテンプレで実現

   - そのままコピペしてAtCoderのサーバーに提出できる（ジャッジしてもらえる)

   - 手元のパソコンでテストケースを試せる

     ```bash
     node abc.js input.txt
     ```

2. 高速な処理かつシンプルな書き方で標準入力を取得することができる

### 📝 テンプレート紹介

```javascript
// ABC(問題の番号など。ex. ABC440-C)
// (出題のURLを貼っています。不要であれば削除してもOKです)
// 2026-01-14 (スニペットを呼び出したときの日付が自動で入ります)


"use strict";
const fs = require('fs');

function Main(input) {
    const args = input.trim().split(/\s+/);
    let cur = 0;
    const next = () => args[cur++];
    const nextNum = () => Number(next());
    const nextBigInt = () => BigInt(next());
    // const lines = input.replace(/\r/g, '').trim().split(/\n/);

    // 👇️ ロジックを記述 👇️



    // 👆️ ロジックを記述 👆️
}

Main(require("fs").readFileSync(process.platform === 'linux' ? "/dev/stdin" : process.argv[2], "utf8"));
```

上から順番に簡単に説明をしていきます(上記をコピペしてもOKですが、記事の下の方にVS Codeのスニペット登録用のjsonも用意しています。是非使ってみてください)。

まず、冒頭のこの部分。

```javascript
"use strict";
const fs = require('fs');
```

"use strict";は、予期せぬエラーや曖昧な記述を未然に防ぐためのJSの厳格モードで実行する宣言。

const fs = require('fs');は、ファイルシステムを扱い、外部のデータを読み込むためのモジュールを用意します。

次に、メインの処理を行う関数。

```javascript
function Main(input) {
    const args = input.trim().split(/\s+/);
```

ここでは、受け取った入力データ input を使いやすくしています。

trim() で前後の余計な空白を取り除き、split(/\s+/) でスペースや改行ごとに区切って、単語ごとのリスト args にしています。

ここからがこのテンプレートの便利ポイントその2です。

```javascript
let cur = 0;
    const next = () => args[cur++];
    const nextNum = () => Number(next());
    const nextBigInt = () => BigInt(next());
    // const lines = input.replace(/\r/g, '').trim().split(/\n/);
```

let cur = 0; は、今リストの何番目を見ているかという目印。

next() という関数は、呼ばれるたびに現在の cur の場所にある言葉を取り出して、目印をひとつ次に進めます。

nextNum() は取り出したものを数字に変換、nextBigInt() は巨大な整数として扱えるように変換。

これにより、next（またはnextNum、nextBigInt）を呼び出して「次の数字をちょうだい」というだけで、順番通りに次のデータを受け取れるようになります。小人さんが隣にいて、必要なときに声をかければ次の入力値が受け取れるイメージです。

#### 💡 args (next系) と lines の使い分け

このテンプレートには、コメントアウトされた `const lines = ...` という行があります。この2つの使い分けの目安は以下の通りです。

1. **`args` と `next()` 系を使う場合（推奨・基本）**

   - **向いている問題**: 数列、グラフの辺情報、クエリ処理など、ほとんどの問題。
   - **理由**: 改行やスペースの区切りを気にせず、前から順番に値を取り出せるため、実装が楽でバグりづらいです。

2. **`lines` を使う場合**

   - **向いている問題**: グリッド問題（迷路）、アスキーアートなど、文字列を「行ごと」に扱いたい問題。

   - **例**: 以下のような入力のとき。

     ```
     S...
     .#..
     ..#G
     ```

   - **理由**: `next()` 系だと1文字ずつ分解して再構築するのが手間ですが、`lines` なら `lines[y][x]` のように座標で直感的にアクセスできます。グリッド問題が出たら、`args` の行をコメントアウトして、`lines` のコメントアウトを外して使いましょう。

#### ⚠️ BigInt（巨大な整数）を使うときの注意点

`nextBigInt()` は大きな整数を扱うときに使いますが、普通の数字（Number型）と混ぜて使ってしまうとエラーになります。

* **混ぜるな危険（TypeError）**

    ```javascript
    const big = nextBigInt(); // 例えば 1000000000000000000n という値
    
    // ❌ 普通の数字と計算しようとするとエラーになります
    // console.log(big + 1); 
    
    // ⭕️ 相手にも 'n' をつけて BigInt にしてあげる必要があります
    console.log(big + 1n);
    ```

* **出力時の注意**

    そのまま `console.log(big)` すると `12345n` のように末尾に `n` がついたまま出力されてしまい、不正解（WA）になることがあります。
    最後に `console.log(ans.toString())` のように文字列にしてから出力しましょう。

最後に、この行。実際にデータを読み込んで `Main` 関数に渡している部分です。

```javascript
Main(require("fs").readFileSync(process.platform === 'linux' ? "/dev/stdin" : process.argv[2], "utf8"));
```

`process.platform === 'linux'` で、今動かしている環境が本番のAtCoderのサーバー（Linux）なのか、手元のパソコンなのかを判断。AtCoderのサーバーなら `/dev/stdin` という標準入力から、手元ならファイルからデータを`node abc.js input.txt`のターミナルのコマンドで読み込みます(ファイル名は任意)。

### ⚔️ 実践例

1. [ABC438 **C - 1D puyopuyo**](https://atcoder.jp/contests/abc438/tasks/abc438_c)

   2025-12-27のABC438の出題です。

   1～N個の数字を取り込むパターンで、基本形といってもいい頻出のパターンです。

   入力条件

   ![ABC438-Cの入力条件](../images/basement/ABC438C_input_ja.png)入力例

   ```
10
   1 1 1 4 4 4 4 1 2 3
   ```
   
   取り込み例

   ```javascript
const N = nextNum();
   const A = [];
   for (let i = 0; i < N; i++) {
       A.push(nextNum());
   }
   ```
   
   **forループ**を使うのがポイントです。

   取り込み結果

   ```javascript
// console.log(N);
   10
   // console.log(A);
   [1, 1, 1, 4, 4, 4, 4, 1, 2, 3]
   ```
   
   この例だとまだちょっと良さが実感できないかもしれませんが、次の例は便利さがわかりやすいと思います。

2. [**ABC440 C - Striped Horse**](https://atcoder.jp/contests/abc440/tasks/abc440_c)

   こちら先日2026-01-10のABC440の出題です。

   T個のテストケースがあって、それぞれのテストケースで先程のB問題を取り込むパターンです。

   初めて見るひとはちょっと悩んでしまったりするのではないでしょうか？

   わたしも最初はぜんぜんわからず、取り込みだけで30分くらい考えたこともあります😢

   こういうテストケースをスムーズに取り込むことができれば、本来の解法のロジックに注力できます。

   入力条件

   ![ABC440-Cの入力条件](../images/basement/ABC440C_input_ja.png)入力例

   ```
   4
   8 2
   1 10 10 1 1 10 10 1
   8 3
   1 10 10 1 1 10 10 1
   8 4
   1 10 10 1 1 10 10 1
   4 100
   100000 100000 100000 100000
   ```
   
   取り込み例

   ```javascript
   const T = nextNum();
   const tests = [];
   for (let i = 0; i < T; i++) {
       const N = nextNum();
       const W = nextNum();
       const C = [];
       for (let j = 0; j < N; j++) {
           C.push(nextNum());
       }
       tests.push({ N, W, C });
   }
   ```
   
   Tこのテストケースに対してforループを回していきます。ループのなかでは定数部分を取り込んでいき、さらにその中で配列として取り込んでいき、このときは最後にオブジェクトとしてtestsの配列に格納していきました。

   取り込み結果(表示は整形しています)

   ```javascript
   // console.log(tests);
   [
     { N: 8, W: 2, C: [1, 10, 10, 1, 1, 10, 10, 1] },
     { N: 8, W: 3, C: [1, 10, 10, 1, 1, 10, 10, 1] },
     { N: 8, W: 4, C: [1, 10, 10, 1, 1, 10, 10, 1] },
     { N: 4, W: 100, C: [ 100000, 100000, 100000, 100000 ] }
   ]
   ```
   
   わりとシンプルな書き方で、テストケースがしっかり取り込めました。

### ✅️ VS Codeのスニペットとして登録(推奨)

### ⚙️ 手順

1. `ctrl + shift + P` でコマンドパレットを開く
2. `Snippets: Configure Snippets`を開く
3. `javascript.json`を開く
4. 以下のスニペットを適宜名前など変更して登録する

```json
{
	"AtCoder Node.js Template (Smart Input)": {
		"prefix": "AtCoderTemplate", //💡呼び出すときの名前です。好きな名前に変更しましょう
		"body": [
			"// ABC$1",
			"// ",
			"// $CURRENT_YEAR-$CURRENT_MONTH-$CURRENT_DATE",
			"",
			"\"use strict\";",
			"const fs = require('fs');",
			"",
			"function Main(input) {",
			"    const args = input.trim().split(/\\s+/);",
			"    let cur = 0;",
			"    const next = () => args[cur++];",
			"    const nextNum = () => Number(next());",
			"    const nextBigInt = () => BigInt(next());",
			"    // const lines = input.replace(/\\r/g, '').trim().split(/\\n/);",
			"",
			"    // 👇️ ロジックを記述 👇️",
			"",
			"    $0",
			"",
			"    // 👆️ ロジックを記述 👆️",
			"}",
			"",
			"Main(require(\"fs\").readFileSync(process.platform === 'linux' ? \"/dev/stdin\" : process.argv[2], \"utf8\"));"
		],
		"description": "競技プログラミング用 Node.js 標準入力テンプレート（自動切替・高速化対応）"
	}
}
```



---



# ✒️ AtCoder Node.js Template (English Version)

### ✨ Key Features of This Template

1. **Single Template for Submission and Testing**

   - **Submit directly**: Copy and paste the code to the AtCoder server for judging.

   - **Test locally**: Run test cases on your local machine.

     ```bash
     node abc.js input.txt
     ```

2. **Fast and Simple Input Processing**: Efficiently handles standard input with concise code.

### 📝 Template Introduction

```javascript
// ABC(Problem Number, e.g., ABC440-C)
// (Link to the problem. Delete if not needed)
// 2026-01-14 (Date automatically inserted via snippet)


"use strict";
const fs = require('fs');

function Main(input) {
    const args = input.trim().split(/\s+/);
    let cur = 0;
    const next = () => args[cur++];
    const nextNum = () => Number(next());
    const nextBigInt = () => BigInt(next());
    // const lines = input.replace(/\r/g, '').trim().split(/\n/);

    // 👇️ Write your logic here 👇️



    // 👆️ Write your logic here 👆️
}

Main(require("fs").readFileSync(process.platform === 'linux' ? "/dev/stdin" : process.argv[2], "utf8"));
```

Here is a brief explanation of the code from top to bottom.

First, the beginning:

```javascript
"use strict";
const fs = require('fs');
```

`"use strict";` enables strict mode in JavaScript to prevent unexpected errors and ambiguous code. `const fs = require('fs');` imports the file system module to handle external data input.

Next, the main processing function:

```javascript
function Main(input) {
    const args = input.trim().split(/\s+/);
```

Here, we prepare the received `input` data for easy usage. `trim()` removes extra whitespace from both ends, and `split(/\s+/)` splits the input by spaces or newlines into a list of words called `args`.

Now for the second convenience point of this template:

```javascript
let cur = 0;
    const next = () => args[cur++];
    const nextNum = () => Number(next());
    const nextBigInt = () => BigInt(next());
    // const lines = input.replace(/\r/g, '').trim().split(/\n/);
```

`let cur = 0;` keeps track of which item in the list we are currently looking at. The `next()` function retrieves the word at the current `cur` position and advances the marker by one each time it is called. `nextNum()` converts the retrieved item to a number, and `nextBigInt()` converts it to a BigInt for handling large integers. This allows you to retrieve data sequentially just by calling `next` (or `nextNum`, `nextBigInt`), like saying "Give me the next number." It's like having a little helper next to you who hands you the next input value whenever you ask.

#### 💡 Difference between `args` (next functions) and `lines`

This template includes a commented-out line `const lines = ...`. Here is how to decide which to use:

1. **Using `args` and `next()` functions (Recommended/Basic)**

   - **Suitable for**: Sequences, graph edge information, query processing, and most other problems.
   - **Reason**: You can retrieve values in order without worrying about line breaks or spaces, making implementation easier and less error-prone.

2. **Using `lines`**

   - **Suitable for**: Grid problems (mazes), ASCII art, or problems where you want to handle strings "line by line".

   - **Example Input**:

     ```
     S...
     .#..
     ..#G
     ```

   - **Reason**: Using `next()` functions would split the input character by character, requiring reconstruction. With `lines`, you can intuitively access data using coordinates like `lines[y][x]`. If you encounter a grid problem, comment out the `args` line and uncomment the `lines` line.

#### ⚠️ Caution when using BigInt (Large Integers)

`nextBigInt()` is used for handling large integers exceeding $2^{53}-1$ (approx. $9 \times 10^{15}$), but it does not mix well with standard numbers (Number type). Mixing them will cause an error.

- **Do Not Mix (TypeError)**

  ```javascript
  const big = nextBigInt(); // e.g., value is 1000000000000000000n
  
  // ❌ Error when trying to calculate with a standard number
  // console.log(big + 1); 
  
  // ⭕️ You must add 'n' to the other number to make it a BigInt
  console.log(big + 1n);
  ```

- **Output Caution**

  If you `console.log(big)` directly, it may output with an `n` at the end (e.g., `12345n`), resulting in a Wrong Answer (WA). Always convert it to a string using `console.log(ans.toString())` before outputting.

Finally, this line reads the actual data and passes it to the `Main` function:

```javascript
Main(require("fs").readFileSync(process.platform === 'linux' ? "/dev/stdin" : process.argv[2], "utf8"));
```

`process.platform === 'linux'` checks if the code is running on the actual AtCoder server (Linux) or your local machine. If it's the AtCoder server, it reads from standard input (`/dev/stdin`). If it's local, it reads data from a file using the terminal command `node abc.js input.txt` (the filename can be anything).

### ⚔️ Practical Examples

1. [ABC438 **C - 1D puyopuyo**](https://atcoder.jp/contests/abc438/tasks/abc438_c)

   Problem from ABC438 on 2025-12-27. A pattern where you import 1 to N numbers. This is a very common, basic pattern.

   **Input**

   ![ABC438-Cの入力条件](../images/basement/ABC438C_input_en.png)

   **Input Example**

   ```
   10
   1 1 1 4 4 4 4 1 2 3
   ```

   **Import Example**

   ```javascript
   const N = nextNum();
   const A = [];
   for (let i = 0; i < N; i++) {
       A.push(nextNum());
   }
   ```

   Using a **for loop** is the key.

   **Import Result**

   ```javascript
   // console.log(N);
   10
   // console.log(A);
   [1, 1, 1, 4, 4, 4, 4, 1, 2, 3]
   ```

2. [**ABC440 C - Striped Horse**](https://atcoder.jp/contests/abc440/tasks/abc440_c)

   Problem from ABC440 on 2026-01-10. There are T test cases, and for each test case, you import a pattern similar to the previous problem.

   **Input**

   ![ABC440-Cの入力条件](../images/basement/ABC440C_input_en.png)
   
   *Input Example**
   
   ```
   4
   8 2
   1 10 10 1 1 10 10 1
   8 3
   1 10 10 1 1 10 10 1
   8 4
   1 10 10 1 1 10 10 1
   4 100
   100000 100000 100000 100000
   ```
   
   **Import Example**
   
   ```javascript
   const T = nextNum();
   const tests = [];
   for (let i = 0; i < T; i++) {
       const N = nextNum();
       const W = nextNum();
       const C = [];
       for (let j = 0; j < N; j++) {
           C.push(nextNum());
       }
       tests.push({ N, W, C });
   }
   ```

   We loop through T test cases. Inside the loop, we import the constant parts, then import arrays within that, and finally store everything as an object in the `tests` array.
   
   **Import Result** (Formatted for display)
   
   ```javascript
   // console.log(tests);
   [
     { N: 8, W: 2, C: [1, 10, 10, 1, 1, 10, 10, 1] },
     { N: 8, W: 3, C: [1, 10, 10, 1, 1, 10, 10, 1] },
     { N: 8, W: 4, C: [1, 10, 10, 1, 1, 10, 10, 1] },
     { N: 4, W: 100, C: [ 100000, 100000, 100000, 100000 ] }
   ]
   ```
   
   The test cases were imported correctly with relatively simple code.

### ✅️ Register as VS Code Snippet (Recommended)

### ⚙️ Steps

1. Open Command Palette with `ctrl + shift + P`
2. Open `Snippets: Configure Snippets`
3. Open `javascript.json`
4. Register the following snippet, changing the name as desired.

```json
{
	"AtCoder Node.js Template (Smart Input)": {
		"prefix": "AtCoderTemplate", //💡 Name used to call the snippet. Change to whatever you like.
		"body": [
			"// ABC$1",
			"// ",
			"// $CURRENT_YEAR-$CURRENT_MONTH-$CURRENT_DATE",
			"",
			"\"use strict\";",
			"const fs = require('fs');",
			"",
			"function Main(input) {",
			"    const args = input.trim().split(/\\s+/);",
			"    let cur = 0;",
			"    const next = () => args[cur++];",
			"    const nextNum = () => Number(next());",
			"    const nextBigInt = () => BigInt(next());",
			"    // const lines = input.replace(/\\r/g, '').trim().split(/\\n/);",
			"",
			"    // 👇️ Write your logic here 👇️",
			"",
			"    $0",
			"",
			"    // 👆️ Write your logic here 👆️",
			"}",
			"",
			"Main(require(\"fs\").readFileSync(process.platform === 'linux' ? \"/dev/stdin\" : process.argv[2], \"utf8\"));"
		],
		"description": "Node.js Standard Input Template for Competitive Programming (Auto-switch/High-speed)"
	}
}
```