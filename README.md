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