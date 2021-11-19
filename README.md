# break/continue のためのラベル
```js
outer: for (let i = 0; i < 3; i++) {

  for (let j = 0; j < 3; j++) {

    let input = prompt(`Value at coords (${i},${j})`, '');

    // 文字から文字またはキャンセルされた場合、両方のループから抜ける
    if (!input) break outer; // (*)

    // 値に何かをする処理...
  }
}
alert('Done!');
```
# 数値変換 単項演算子 +
```js
// 数値の場合、何の影響もありません
let x = 1;
alert( +x ); // 1

let y = -2;
alert( +y ); // -2

// 非数値を数値に変換します
alert( +true ); // 1
alert( +"" );   // 0
```
# 8つのデータ型データ型
* number
* bigint
* string
* boolean
* null
* undefined
* symbol
* object
※object 以外はプリミティブ型(primitive type)
※typeof 演算子は値の型を返します。2つ例外があります:
```js
typeof null == "object" // 言語の間違い
typeof function(){} == "function" // 関数は特別に扱われます
```

# || && ??
## ||
```js
alert( 1 || 0 ); // 1 (1 は真)

alert( null || 1 ); // 1 (1 は最初の真値)
alert( null || 0 || 1 ); // 1 (最初の真値)

alert( undefined || null || 0 ); // 0 (すべて偽、なので最後の値が返却される)
```
## &&
```js
// 最初のオペランドが真の場合、
// AND は2つ目のオペランドを返す:
alert( 1 && 0 ); // 0
alert( 1 && 5 ); // 5

// 最初のオペランドが偽の場合、
// AND はそれを返します。2つ目のオペランドは無視されます。
alert( null && 5 ); // null
alert( 0 && "no matter what" ); // 0
```
## ??
```js
let firstName = null;
let lastName = null;
let nickName = "Supercoder";

// 最初の null/undefined でない値を表示します
alert(firstName ?? lastName ?? nickName ?? "Anonymous"); // Supercoder
```
# 関数式 vs 関数宣言
## 関数宣言
```js
function sum(a, b) {
  return a + b;
}
```
## 関数式
```js
let sum = function(a, b) {
  return a + b;
};
```

## 違い
### 呼び出されるタイミング
関数宣言はスクリプト/コードブロック全体で使用できます。
```js
sayHi("John"); // Hello, John

function sayHi(name) {
  alert( `Hello, ${name}` );
}
```
関数式は、実行がそれに到達した時に作られ、それ以降で利用可能になります。
```js
sayHi("John"); // エラー!

let sayHi = function(name) {  // (*) no magic any more
  alert( `Hello, ${name}` );
};
```
### スコープ
関数宣言がコードブロックの中で作られるとき、そのブロックの内側であればどこからでも見えます。しかし、その外側からは見えません。
```js
let age = prompt("What is your age?", 18);

// 条件付きで関数を宣言する
if (age < 18) {

  function welcome() {
    alert("Hello!");
  }

} else {

  function welcome() {
    alert("Greetings!");
  }

}

// ...後で使う
welcome(); // エラー: welcome は未定義です
```
if の外側で welcome を見えるようにするためにはどうしたらよいでしょうか？

正しいアプローチは、関数式を使い、welcome を if の外で宣言し、適切なスコープをもつ変数に代入することです。
```js
let age = 16; // 例として16

if (age < 18) {
  welcome();               // \   (実行)
                           //  |
  function welcome() {     //  |
    alert("Hello!");       //  |  関数宣言はそれが宣言されたブロックの中であれば
  }                        //  |  どこでも利用可能です
                           //  |
  welcome();               // /   (実行)

} else {

  function welcome() {     //  age = 16 の場合, この "welcome" は決して作られません
    alert("Greetings!");
  }
}

// ここは、波括弧の外です
// なのでその中で作られた関数宣言は見ることができません

welcome(); // エラー: welcome は定義されていません
```
## 関数宣言と関数式のどちらを選択するのか？
たいていのケースでは、関数の宣言が必要な場合、関数宣言が望ましいです。なぜなら、それ自身の宣言の前でも利用することができるからです。これにより、コード構成の柔軟性が増し、通常は読みやすくなります。

従って、関数宣言がそのタスクに適さない場合にのみ関数式を使うべきです。

# デバッガコマンド
次のように、debugger コマンドを使うことでもコードを停止することができます:
```js
function hello(name) {
  let phrase = `Hello, ${name}!`;

  debugger;  // <-- デバッガはここで止まります

  say(phrase);
}
```
# ESLint
https://eslint.org/docs/user-guide/getting-started
1. Node.js をインストールします。
2. npm install -g eslint コマンドで ESLint をインストールします(npm は Node.js パッケージインストーラです)
3. JavaScriptプロジェクト(すべてのファイルを含むフォルダ)のルートに .ellintrc という名前の設定ファイルを作ります
4. ESlint と統合するエディタのプラグインをインストール/有効化します。エディタの大多数はそれを持っています。
# コメント
関数の文書化のための特別な構文 JSDoc があります。: 使用方法、パラメータ、返却値
https://ja.wikipedia.org/wiki/JSDoc
# mocha による自動テスト
https://ja.javascript.info/testing-mocha
# Polyfill（ポリフィル）とトランスパイラ
1. トランスパイラ
最新のコードを解析し、古い構文を使って書き換える
2. Polyfill（ポリフィル）
不足している関数を定義する
# オブジェクト
## 生成
```js
let user = new Object(); // "オブジェクトコンストラクタ" 構文
let user = {};  // "オブジェクトリテラル" 構文
```
## 参照
```js
user.name;
user['name'];
```
## 追加
```js
user.isAdmin = true;
```
## 削除
```js
delete user.age;
```
## const オブジェクトは修正できます
```js
const user = {
  name: "John"
};

user.name = "Pete"; // (*)

alert(user.name); // Pete
```
## 算出プロパティ
オブジェクトリテラルでは、角括弧を使うことができます。それは 算出プロパティ と呼ばれます。
```js
let fruit = prompt("Which fruit to buy?", "apple");

let bag = {
  [fruit]: 5, // プロパティ名は変数 fruit から取られます
};

alert( bag.apple ); // 5 fruit="apple" の場合

let fruit = 'apple';
let bag = {
  [fruit + 'Computers']: 5 // bag.appleComputers = 5
};
```
## プロパティの短縮構文
```js
function makeUser(name, age) {
  return {
    name, // name: name と同じ
    age   // age: age と同じ
    // ...
  };
}

let user = makeUser("John", 30);
alert(user.name); // John
```
## プロパティ存在チェック, “in” 演算子
```js
// 1.
let user = {};

alert( user.noSuchProperty === undefined ); // true は "そのようなプロパティはありません" を意味する

// 2.
"key" in object // return boolean
```
## オブジェクトは“for…in” ループ
```js
let user = {
  name: "John",
  age: 30,
  isAdmin: true
};

for (let key in user) {
  // 키
  alert( key );  // name, age, isAdmin
  // 키에 해당하는 값
  alert( user[key] ); // John, 30, true
}
```
