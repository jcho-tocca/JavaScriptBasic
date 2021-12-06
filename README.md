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
## オブジェクトのクローン（マージ）
```js
let user = {
  name: "John",
  age: 30
};

let clone = Object.assign({}, user);
```
## ネストされたクローン
JavaScript ライブラリlodash にある_.cloneDeep(obj) を利用
https://lodash.com/docs#cloneDeep
# メソッド中の “this”
```js
let user = { name: "John" };
let admin = { name: "Admin" };

function sayHi() {
  alert( this.name );
}

// 2つのオブジェクトで同じ関数を使う
user.f = sayHi;
admin.f = sayHi;

// これらの呼び出しは異なる this を持ちます
// 関数の中の "this" は "ドット" の前のオブジェクトです
user.f(); // John  (this == user)
admin.f(); // Admin  (this == admin)

admin['f'](); // Admin (ドットでも角括弧でも問題なくメソッドにアクセスできます)
```
## オブジェクトなしでの呼び出し: this == undefined
```js
function sayHi() {
  alert(this);
}

sayHi(); // undefined
```
非 strict モード(誰かが use strict を忘れた場合)では、このようなケースでは this の値は グローバルオブジェクト (ブラウザでは window、後ほど学びます)になります。
## アロー関数は “this” を持ちません
アロー関数は特別で、それらは “自身の” this を持ちません。もしこのような関数で this を参照した場合、外部の “通常の” 関数から取得されます。

例えば、ここで arrow() は、外部の user.sayHi() メソッドから this を使います:
```js
let user = {
  firstName: "Ilya",
  sayHi() {
    let arrow = () => alert(this.firstName);
    arrow();
  }
};

user.sayHi(); // Ilya
```
# コンストラクタ 関数
似たようなオブジェクトを多数作成する必要がある場合
1. 名前は大文字で始めます。
2. "new" 演算子を使ってのみ実行されるべきです。
```js
function User(name) {
  // this = {};  (暗黙)

  // this へプロパティを追加
  this.name = name;
  this.isAdmin = false;

  // return this;  (暗黙)
}

let user1 = new User("Jack");
let user2 = new User("Ann");
let user3 = new User("Alice");
```
どのような関数（this を持たないアロー関数を除く）でもコンストラクタとして使用できます。
## コンストラクタの呼び出しモードの確認: new.target
```js
function User() {
  alert(new.target);
}

// new なし:
User(); // undefined

// new あり:
new User(); // function User { ... }

//-----------------------//

function User(name) {
  if (!new.target) { // new なしで実行した場合
    return new User(name); // ...new を追加します
  }

  this.name = name;
}

let john = User("John"); // new User へのリダイレクト
alert(john.name); // John
```
## コンストラクタの中のメソッド
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
# オプショナルチェイニング(Optional chaining) '?.'
```js
let userAdmin = {
  admin() {
    alert("I am admin");
  }
};

let userGuest = {};

userAdmin.admin?.(); // I am admin

userGuest.admin?.(); // なにもしない (このようなメソッドはありません)

//-----------------------//

let key = "firstName";

let user1 = {
  firstName: "John"
};

let user2 = null;

alert( user1?.[key] ); // John
alert( user2?.[key] ); // undefined

//-----------------------//

delete user?.name; // user が存在する場合、 user.name を削除します
```
# シンボル型
## 使用理由
```js
let user = { // 別のコードに属しているオブジェクト
  name: "John"
};

let id = Symbol("id");

user[id] = 1;

alert( user[id] ); // キーとして symbol を使ってデータにアクセスできます
```
user オブジェクトは別のコードに属しており、コードはそこでも動作するので、そこに単純に任意のフィールドを追加するべきではありません。それは安全ではありません。しかし、シンボルは誤ってアクセスすることはできず、サードパーティのコードは恐らく見ることすらできないため、恐らく問題になりません。
## symbol.description プロパティ
```js
let id = Symbol("id");
alert(id.description); // id
// symbol.description プロパティを利用して、説明のみを表示
```
## シンボルは for…in ではスキップされます。
```js
let id = Symbol("id");
let user = {
  name: "John",
  age: 30,
  [id]: 123
};

for (let key in user) alert(key); // name, age (no symbols)

// symbol による直アクセスは動作します
alert( "Direct: " + user[id] );
```
## グローバルシンボル
同じ名前のシンボルを同じエンティティにしたいとき使用
```js
// グローバルレジストリから読む
let id = Symbol.for("id"); // symbol が存在しない場合、作られます

// 再度読み込み
let idAgain = Symbol.for("id");

// 同じシンボル
alert( id === idAgain ); // true
```
## symbol から名前を取得
```js
let globalSymbol = Symbol.for("name");
let localSymbol = Symbol("name");

// symbol から名前を取得
alert( Symbol.keyFor(globalSymbol) ); // name, グローバルシンボル
alert( Symbol.keyFor(localSymbol) ); // undefined, グローバルではないので

alert( localSymbol.description ); // name
```
# オブジェクトからプリミティブへの変換
変換をするために、JavaScriptは3つのオブジェクトのメソッドを見つけ呼び出そうとします。
1. メソッドが存在する場合、obj[Symbol.toPrimitive]\(hint) を呼び出します
2. ない場合、hint が "string" であれば
  * obj.toString() と obj.valueOf() を試します。
3. そうでなく、hint が "number" であれば
  * obj.valueOf() と obj.toString() を試します。

## Symbol.toPrimitive
```js
let user = {
  name: "John",
  money: 1000,

  [Symbol.toPrimitive](hint) {
    alert(`hint: ${hint}`);
    return hint == "string" ? `{name: "${this.name}"}` : this.money;
  }
};

// 変換動作の確認:
alert(user); // hint: string -> {name: "John"}
alert(+user); // hint: number -> 1000
alert(user + 500); // hint: default -> 1500
```
## toString/valueOf
```js
let user = {
  name: "John",
  money: 1000,

  // hint="string" の場合
  toString() {
    return `{name: "${this.name}"}`;
  },

  // hint="number" or "default" の場合
  valueOf() {
    return this.money;
  }

};

alert(user); // toString -> {name: "John"}
alert(+user); // valueOf -> 1000
alert(user + 500); // valueOf -> 1500
```
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
# 文字列
```js
String.length // 長さ

String.charAt(index) // 文字抽出

String.split( 区切り文字 ) // 配列へ変換

String.concat(String) // 結合

String.trim() // 左右空白除去

String.toUpperCase() // 大文字

String.toLowerCase() // 小文字

String.substr(start, 長さ) // 文字列抽出

String.substring(start, end) // 文字列抽出

String.slice(start, end) // 文字列抽出(マイナス数字使用可能)

String.replace(対象文字列, 新しい文字列) // 置換
```
# 数字
```js
Number.toFixed(小数点の後に現れる桁の数) // 固定小数点表記

Number.toPrecision(桁数) // 桁数だけの文字列に変換

isNaN(Number) // NaN判定

parseInt(Number, 進法) // 整数変換

parseFloat(Number) // 実数変換
```
# 配列
```js
Array.length // 長さ

Array.join(区切り文字) // 文字結合

Array.split(区切り文字) // 文字列を配列に変換

Array.concat(Array) // 配列結合

Array.reverse() // 要素反転

Array.unshift(要素) // 最初から要素追加

Array.push(要素) // 最後から要素追加

Array.shift() // 最初の要素を抽出

Array.pop() // 最後の要素を抽出

Array.map(function(値, キー, Array){条件}) // 新しい配列生成

Array.forEach(function(値, キー, Array){条件} ) // 各要素を操作

Array.reduce(function(前の値、次の値){条件}) // 左から右へ各要素に対して関数を実行して、単一の値を返す

  var array = [1, 2, 3, 4, 5];
  array.reduce(function(prev, cur) {
    return prev + cur;
  }); // 15

Array.reduceRight(function(後ろの値、次の値){条件}) // 右から左へ

Array.filter(function(要素, キー, Array){条件}) // 条件に合う要素だけの新しい配列を返す

Array.sort(function( 前の値、次の値){条件}) // 並び替え

Array.slice([start[, end]]) // 配列の一部の浅いコピーを新しい配列オブジェクトに作成

Array.indexOf(検索値, スタートキー) // 検索値と同じ値の要素のキーを返す

Array.lastIndexOf (検索値, スタートキー) // 右から左へ

Array.includes (検索値, スタートキー) // 検索値があるがどうか

Array.find(function(要素, index, Array) {条件}); // 検索要素を探して返す

Array.findIndex (function(要素, index, Array) {条件}); // 検索要素のインデックスを探して返す

Array.every(function(要素){条件}) // 配列のすべての要素が通るかどうかをテスト

Array.some(function(要素){条件}) // 配列の少なくとも 1 つの要素が、渡された関数によって実施されるテストに通るかどうかをテスト

Array.isArray(チェックしたい値) // 配列判定
```
## ほとんどのメソッドは “thisArg” をサポートします
```js
arr.find(func, thisArg);
arr.filter(func, thisArg);
arr.map(func, thisArg);
// ...
// thisArg はオプションの最後の引数です

let army = {
  minAge: 18,
  maxAge: 27,
  canJoin(user) {
    return user.age >= this.minAge && user.age < this.maxAge;
  }
};

let users = [
  {age: 16},
  {age: 20},
  {age: 23},
  {age: 30}
];

// army.canJoin が true となるユーザを見つけます
let soldiers = users.filter(army.canJoin, army);

alert(soldiers.length); // 2
alert(soldiers[0].age); // 20
alert(soldiers[1].age); // 23
```
## 配列のループは"for of"を使用すべき
```js
let fruits = ["Apple", "Orange", "Plum"];

// 配列要素の反復処理
for (let fruit of fruits) {
  alert( fruit );
}
```

# 反復可能な(iterables) オブジェクト
* for..of が始まると、そのメソッドを呼び出します(なければエラーになります)。
* メソッドは iterator (メソッド next をもつオブジェクト)を返さなければいけません。
* for..of が次の値を必要とするとき、そのオブジェクトの next() を呼びます。
* next() の結果は {done: Boolean, value: any} の形式でなければなりません。そして done=true は繰り返しが終わったことを示します。そうでない場合は、value は新しい値である必要があります。

## 反復オブジェクトにする方法１
```js
let range = {
  from: 1,
  to: 5
};

// 1. for..of の呼び出しは、最初にこれを呼び出します
range[Symbol.iterator] = function() {

  // ...これは iterator オブジェクトを返します:
  // 2. 以降、for..of はこのイテレータでのみ機能し、次の値を要求します
  return {
    current: this.from,
    last: this.to,

    // 3. for..of ループにより、各繰り返しで next() が呼ばれます
    next() {
      // 4. オブジェクト {done:.., value :...} を返す必要があります
      if (this.current <= this.last) {
        return { done: false, value: this.current++ };
      } else {
        return { done: true };
      }
    }
  };
};

// これで動作します!
for (let num of range) {
  alert(num); // 1, 2, 3, 4, 5
}
```
## 反復オブジェクトにする方法２
```js
let range = {
  from: 1,
  to: 5,

  [Symbol.iterator]() {
    this.current = this.from;
    return this;
  },

  next() {
    if (this.current <= this.to) {
      return { done: false, value: this.current++ };
    } else {
      return { done: true };
    }
  }
};

for (let num of range) {
  alert(num); // 1, そして 2, 3, 4, 5
}
```
## 反復可能(Iterables) と 配列ライク(array-like)
### 反復可能(Iterables)
* Symbol.iterator メソッドを実装したオブジェクト
### 配列ライク(array-like)
* インデックスと length を持ったオブジェクトです。なので、これらは配列のように見えます。
## 配列に変換 - Array.from
反復可能(Iterables) と 配列ライク(array-like)で配列メソッドを使うため、使用する
```js
let arrayLike = {
  0: "Hello",
  1: "World",
  length: 2
};

let arr = Array.from(arrayLike); // (*)
alert(arr.pop()); // World (メソッドが動作します)
```
# Map
Map は Object と同じように、キー付されたデータ項目の集まりです。主な違いは Map は任意の型のキーを許可することです。
```js
new Map() // 新しい map を作ります.
map.set(key, value) // キーで、値を格納します.
map.get(key) // 指定されたキーの値を返却します。map に存在しない key の場合には undefined になります.
map.has(key) // キーが存在する場合に true を返します。そうでなければ false になります.
map.delete(key) // キーで値を削除します.
map.clear() // map をクリアします.
map.size // 現在の要素の数です.
map.keys() // キーに対する iterable を返します。
map.values() // 値に対する iterable を返します。
map.entries() // エントリ [key, value]　の iterable を返します。これは for..of でデフォルトで使われます。
map.forEach((value, key, map) => {}); // ループ
Object.entries() // オブジェクトから Map を生成
Object.fromEntries(map.entries()); // Map から オブジェクト
Object.fromEntries(map); // Map から オブジェクト(entryies省略)
```
# Set
Set は、特別タイプのコレクションで、“値の集合” （キーなし）です。それぞれの値は一度しか現れません。
```js
new Set(iterable) // set を作ります。オプションで値の配列(任意の iterable が指定可能)からも可能です。
set.add(value) // 値を追加し、 set 自身を返します。
set.delete(value) // 値を削除し、value が呼び出し時に存在すれば true, そうでなければ false を返します。
set.has(value) // set の中に値が存在すれば true を返し、それ以外は false です。
set.clear() // set から全てを削除します。
set.size // set の要素数です。
set.forEach((value, valueAgain, set) => {}); // ループ
set.keys() // 値に対する iterable なオブジェクトを返します。
set.values() // set.keys と同じで、Map との互換性のためです。
set.entries() // [value, value] のエントリのための iterable なオブジェクトを返します。Map の互換性のために存在します。
```
Map と Set のイテレーションは常に挿入順で行われます。そのため、これらのコレクションが順序付けられていないとは言えませんが、要素を並べ替えたり、その番号で要素を直接取得することはできません。
# WeakMap と WeakSet
WeakMap のキーはプリミティブな値ではなくオブジェクトでなければならない。
* ユースケース: オブジェクトの追加のデータ格納したい時に使用
* ユースケース: キャッシュ
# Object.keys, values, entries
```js
Object.keys(obj) // キーの配列を返します。
Object.values(obj) // 値の配列を返します。
Object.entries(obj) // [key, value] ペアの配列を返します。
```
## オブジェクトの変換
```js
let prices = {
  banana: 1,
  orange: 2,
  meat: 4,
};

let doublePrices = Object.fromEntries(
  // 配列に変換して map を実行、その後 fromEntries でオブジェクトに戻します
  Object.entries(prices).map(([key, value]) => [key, value * 2])
);

alert(doublePrices.meat); // 8
```
# 分割代入
```js
let options = {
  title: "My menu",
  items: ["Item1", "Item2"]
};

function showMenu({
  title = "Untitled",
  width: w = 100,  // width は w に
  height: h = 200, // height は h に
  items: [item1, item2] // items の最初の要素は item1 へ、次は item2 へ
}) {
  alert( `${title} ${w} ${h}` ); // My Menu 100 200
  alert( item1 ); // Item1
  alert( item2 ); // Item2
}

showMenu(options);
```
非構造化対象全体のデフォルト値に {} を指定する
```js
function showMenu({ title = "Menu", width = 100, height = 200 } = {}) {
  alert( `${title} ${width} ${height}` );
}

showMenu(); // Menu 100 200
```
# Date 日付 と 時刻
https://ja.javascript.info/date
# JSON メソッド, toJSON
```js
JSON.stringify(obj) // オブジェクトをJSONに変換します。
JSON.parse(json) // JSONをオブジェクトに変換します。
```
# 残りのパラメータ
```js
function sumAll(...args) { // args は配列の名前です
  let sum = 0;

  for (let arg of args) sum += arg;

  return sum;
}

alert( sumAll(1) ); // 1
alert( sumAll(1, 2) ); // 3
alert( sumAll(1, 2, 3) ); // 6
```
```js
function showName(firstName, lastName, ...titles) {
  alert( firstName + ' ' + lastName ); // Julius Caesar

  // 残りは titles 配列に入ります
  // i.e. titles = ["Consul", "Imperator"]
  alert( titles[0] ); // Consul
  alert( titles[1] ); // Imperator
  alert( titles.length ); // 2
}

showName("Julius", "Caesar", "Consul", "Imperator");
```
# “arguments” 変数
```js
function showName() {
  alert( arguments.length );
  alert( arguments[0] );
  alert( arguments[1] );

  // 反復可能(iterable) です。
  // for(let arg of arguments) alert(arg);
}

// 表示: 2, Julius, Caesar
showName("Julius", "Caesar");

// 表示: 1, Ilya, undefined (2つ目の引数がないので)
showName("Ilya");
```
※アロー関数は "arguments" を持ちません
# スプレッド 演算子
```js
let arr = [3, 5, 1];
let arr2 = [8, 9, 15];

let merged = [0, ...arr, 2, ...arr2];

alert(merged); // 0,3,5,1,2,8,9,15 (0, then arr, then 2, then arr2)

// --------------------------

let str = "Hello";

alert( [...str] ); // H,e,l,l,o
```
## 配列/オブジェクトのコピー
### 配列のコピー
```js
let arr = [1, 2, 3];

let arrCopy = [...arr]; // 配列をパラメータのリストに展開します
                        // その後、結果を新しい配列に格納します

// 配列が同じコンテンツを持っているか？
alert(JSON.stringify(arr) === JSON.stringify(arrCopy)); // true

// 配列は等価か？
alert(arr === arrCopy); // false (同じ参照ではないため)

// 最初の配列を修正しても、コピーは修正されません:
arr.push(4);
alert(arr); // 1, 2, 3, 4
alert(arrCopy); // 1, 2, 3
```
### オブジェクトのコピー
```js
let obj = { a: 1, b: 2, c: 3 };

let objCopy = { ...obj }; // オブジェクトをパラメータのリストに展開します
                          // その後、結果を新しいオブジェクトに結果を返します

// オブジェクトは同じコンテンツを持っているか？
alert(JSON.stringify(obj) === JSON.stringify(objCopy)); // true

// オブジェクトは等価か？
alert(obj === objCopy); // false (同じ参照ではありません))

// 最初のオブジェクトを修正しても、コピーは修正されません:
obj.d = 4;
alert(JSON.stringify(obj)); // {"a":1,"b":2,"c":3,"d":4}
alert(JSON.stringify(objCopy)); // {"a":1,"b":2,"c":3}
```
※このオブジェクトコピーの方法は、let objCopy = Object.assign({}, obj) 、あるいは、配列の let arrCopy = Object.assign([], arr) よりもはるかに短いです。そのため、可能な場合はこちらの方が好まれます。

# 変数スコープ、クロージャ
JavaScript では、実行中のすべて関数、コードブロック {...} およびスクリプト全体には、レキシカル環境 と呼ばれる内部の（隠れた）関連オブジェクトがあります。

## Step 1. 変数
### レキシカル環境オブジェクトの構成
1. 環境レコード(Environment Record) 。
  プロパティとしてすべてのローカル変数をもつオブジェクトです(this の値など、他の情報もいくらか持っています)。
2. 外部のレキシカル環境 への参照。
  通常、直近の外部のレキシカルなコードに関連付けられています（波括弧の外側）。

* "変数" は単に、特別な内部オブジェクトである 環境レコード のプロパティです。“変数を取得または変更する” とは、“そのオブジェクトのプロパティを取得または変更する” ことを意味します。
* スクリプト全体に関連付けられた、いわゆるグローバルレキシカル環境です。
![image](https://user-images.githubusercontent.com/64624465/144795641-69b6cda4-6060-45f5-b784-32751bb9aaec.png)
## Step 2. 関数宣言
* 関数宣言は即座に完全に初期化されます。
レキシカル環境が作られたとき、関数宣言はすぐに使用できる関数（宣言まで使用できない let とは異なります）になります。
※関数式にはあてはまりません。
## Step 3. 内外のレキシカル環境












# クラス
* コンストラクタ関数：既存文法
* クラス：モーダンな文法（シンタックスシュガー）

## 関数クラスパターン（コンストラクタ関数）
```js
function User(name, birthday) {

  // User 内の他のメソッドからのみ見えます
  function calcAge() {
    return new Date().getFullYear() - birthday.getFullYear();
  }

  this.sayHi = function() {
    alert(name + ', age:' + calcAge());
  };
}

let user = new User("John", new Date(2000,0,1));
user.sayHi(); // John
```
## ファクトリークラスパターン（コンストラクタ関数）
new を全く使わずにクラスを生成することができます。
```js
function User(name, birthday) {
  // User 内の他のメソッドからのみ見えます
  function calcAge() {
    return new Date().getFullYear() - birthday.getFullYear();
  }

  return {
    sayHi() {
      alert(name + ', age:' + calcAge());
    }
  };
}

let user = User("John", new Date(2000,0,1));
user.sayHi(); // John
```
## プロトタイプベースのクラス
```js
function User(name, birthday) {
  this._name = name;
  this._birthday = birthday;
}

User.prototype._calcAge = function() {
  return new Date().getFullYear() - this._birthday.getFullYear();
};

User.prototype.sayHi = function() {
  alert(this._name + ', age:' + this._calcAge());
};

let user = new User("John", new Date(2000,0,1));
user.sayHi(); // John
```
## プロトタイプベースの継承
```js
// 以前と同じ Animal
function Animal(name) {
  this.name = name;
}

// すべての animals 食べることができますよね?
Animal.prototype.eat = function() {
  alert(this.name + ' eats.');
};

// 以前と同じ Rabbit
function Rabbit(name) {
  this.name = name;
}

Rabbit.prototype.jump = function() {
  alert(this.name + ' jumps!');
};

// 継承チェーンを設定します
Rabbit.prototype.__proto__ = Animal.prototype; // (*)

let rabbit = new Rabbit("White Rabbit");
rabbit.eat(); // rabbits も食べることができる
rabbit.jump();
```
