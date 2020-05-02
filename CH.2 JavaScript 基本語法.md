---
tags: javascript , 前端
---
`參考書籍:javascript & jQuery 網站互動設計程式進化之道`
`#190201 #090528`

# Java Script自學筆記 - CH.2 JavaScript 基本語法

## 什麼是變數
每一次程式碼運行的時候，儲存在變數中的資料是可以更變的(或可變化的)

### 變數:如何宣告(declare)

```javascript
var quantity;
```
* 第一個字詞均以小寫表示

### 變數:如何指定值?
```javascript
quantity = 3;
```
* 指定一個值予變數(assign a value)
* 未設定變數之前 為undefined(未定義)

## 資料型別：數值、字串、布林值
JavaScript能夠辨別數值、字串、布林值

### 數值
    e.g. 0.75
* 利用變數儲存數值資料 

JS
```javascript=
// Create three variables to store the information needed.
var price;
var quantity;
var total;

// Assign values to the price and quantity variables.
price = 5;
quantity = 14;
// Calculate the total by multiplying the price by quantity.
total = price * quantity;

// Get the element with an id of cost.
var el = document.getElementById('cost');
el.textContent = '$' + total;
```
 HTML
```htmlmixed=
<h1>Elderflower</h1>
<div id="content">
    <h2>Custom Signage</h2>
    <div id="cost">$70</div>
    <img src="images/preview.jpg" alt="Sign">
</div>
```

### 字串
    e.g.'Hello World'
* 利用變數儲存字串資料
```javascript=
// Create variables to hold the name and note text.
var username;
var message;

// Assign values to these variables.
username = 'Molly';
message = 'See our upcoming range';

// Get the element with an id of name.
var elName = document.getElementById('name');
// Replace the content of this element.
elName.textContent = username;

// Get the element with an id of note.
var elNote = document.getElementById('note');
// Replace the content of this element.
elNote.textContent = message;
```
### 布林值
    e.g. ture/false
* 利用變數儲存字串資料
```javascript=
var inStock;
var shipping;
inStock = true;
shipping = false;

// Get the element that has an id of stock
var elStock = document.getElementById('stock');
// Set class name with value of inStock variable
elStock.className = inStock;

// Get the element that has an id of shipping
var elShip = document.getElementById('shipping');
// Set class name with value of shipping variable
elShip.className = shipping;
```

## 縮寫

```javascript=
var price = 5;
var quantity = 14;
var total = price * quantity;
// ------------- //

var price,quantity,total;
price = 5;
quantity = 14;
otal = price * quantity;
// ------------- //

var price = 5 , quantity = 14;
var total = price * quantity;

```
* 將總金額寫入至具有id屬性值為cost`<sapn id="cost"></span>`的元件中
```javascript=
var el = document.getElementById('cost');
el.textContent ='$' + tatal;
```

## 變數的命名規則

1. **不能以數字起始**，**不能**包含".""-"，以字母、"$"、"_"起始
2. 不能使用keywords或reserved words(保留字)
3. 駝峰式: e.g.`firstName`


## 陣列(Array)
* 儲存一系列的值，且值與值**具有關聯性**
* 並不需要明確指出他需要襖存的資料項目數量
    * e.g. 購物清單
* 一樣要賦予一個變數名稱
* 陣列值不需要都是相同資料型別，可同時儲存字串、數值、布林

1. array literal(陣列實字)(比較好的方式)
```javascript=
var colors; 
colors = ['white', 'black', 'custom'];
//var colors = ['white', 'black', 'custom'];

//colors[2]='beige'; Update the third item in the array

// Show the first item from the array.
var el = document.getElementById('colors'); //取得id=colors的元件
el.textContent = colors[0]; //顯示陣列中第1個項目
//0:white 1:black 2:custom
```
2. array constructor (陣列構造子)
```javascript=
var colors = new Array('white', 
                       'black',
                       'custom');

// Show the first item from the array.
var el = document.getElementById('colors');
el.textContent = colors[0];
```
* 存取陣列中的項目
```javascript=
var itemThree;
itemThree = colors[2];
```
* 陣列的項目數量 `length`
```javascript=
var numColors;
numColors = colors.length;
```
* 改變array中的item content
```javascript
colors[2]='beige'; //Update the third item in the array
```

## 運算式(expressions)
* 一個式子
```javascript
var color = 'beige';
var area = 3*2;
```
## 運算子(operators)
* 符號 `e.g. '' > + * &&`
```javascript
buy = 3 > 5;
buy = (5>3) && (2<4);
```
### 算術運算子(執行數學基本運算)
* `++` 將目前的值加1
* `--` 將目前的值減1
* `%` MOD 相除回傳餘數