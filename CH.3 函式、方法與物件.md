---
tags: javascript , 前端
---
`參考書籍:javascript & jQuery 網站互動設計程式進化之道`
`#190529`

# Java Script自學筆記 - CH.3 函式、方法與物件

## 什麼是函式(function)
* 經常被重複執行以完成相同的工作，就是適合
* 建立為可重複使用的函式
* 函式是一種方法，預先將某項工作所需要執行的步驟先儲存起來
* **參數**
    * 例如要算面積，要知道長、寬資訊，長、寬就是參數
* **回傳值**
    * 提供的答案
* 匿名函式
    * 無法被呼叫，當解譯器看到後會執行
    
* 函式被執行後才宣告
    * 可行的，因為解譯器事先讀取完整的程式腳本後，再依序執行每一行的敘述
    * 但一般還是先宣告再call 
### 基本函式

* 需要給function 一個名稱(或稱作 識別字(identifier))
```javascript=
var msg = 'Sign up to receive our newsletter for 10% off!';

// 宣告函式
function updateMessage() {
  var el = document.getElementById('message');
  el.textContent = msg;
}

// Call the function(呼叫函式)
updateMessage();
```
### 宣告一個需要資訊的函式
* 宣告函式時，需要再給參數，在這裡的參數功能就像變數一樣
* 每次呼叫函式計算面積所需的寬高尺寸可能都會不同
```javascript=
function getArea (width, height){
  return width * height;
}
```

### 呼叫一個需要資訊的函式
* **引數**
    * 呼叫時，需要在函式名稱後的括弧中傳入需使用的值
    * 以數值 or 變數 方式傳入
```javascript=
getArea(3,5)

wallWidth = 3;
wallHeight = 5;
getArea(wallWidth,wallHeight)
```

### 從函式取得一個回傳值
* **retrun** 
    * 回傳一個值給最初呼叫此函式的程式碼
    * retrun被執行時，會回到最初的呼叫此函式的敘述句位置，以下程式均不執行
```javascript=
function calculateArea(width,height){
  var area = width * height;
  return area;
}

var wallOne = calculateArea (3,5);
var wallTwo = calculateArea (8,5);
```

### 從函式取得多個回傳值
* 使用Array回傳多個值
```javascript=
function getSize (width,height,depth){
  var area = width * height ;
  var volume = width * height * depth;
  var sizes = [area , volume];
  return sizes;
}

var areaOne = getSize(3,2,2)[0]; //6 
var areaTwo = getSize(4,5,6)[1]; //120
console.log(areaOne,areaTwo);
```
## 只被呼叫一次，適合使用的函式
* **匿名函式、IIFE**使用時機
    * 計算函式所需要的值
    * 指定給物件的特性值
    * 當事件發生時，在事件處理器和偵測器中執行指定工作(CH6)
    * 避免兩個程式腳本中有相同名稱的變數發生命名衝突


### 1. 匿名函式

* 宣告變數後，內容應該是運算式，但是寫函式，所以這個函式又稱為"**函式運算式**"
```javascript=
var area = function(width,height){
  return width * height;
}
var sizes = area(2,4);
console.log(sizes); //8
```

### 2. 立即執行函式運算式(IIFE)
* 無法再被呼叫
```javascript=
var area = (function(){
  var width = 3;
  var height = 7;
  return width * height ;
}())
console.log(area); //21
```

* var area = (function(){
  var width = 3;
  var height = 7;
  return width * height ;
  }<font color = red>**()**</font>)
    * 此處的紅色括號為**最終結束括號**
        * 告知解譯器須立即執行此函式
    
* var area = <font color = green>**(** </font>function(){
  var width = 3;
  var height = 7;
  return width * height ;
}()<font color = green>**)** </font>

    * 此處的綠色括號為**群組運算子**
        *  確保解譯器將此函式視為一個運算式
        
## 變數的有效範圍(scope)

### 區域變數
* 在某個funciton內使用var宣告
* 當函式開始執行時，解譯器就會建立一個區域變數；一但函式執行完畢，就會把這些區域變數移除

### 全域變數
* 在function外宣告
* 儲存在電腦的記憶體中(比區域變數更占記憶體空間)
* 如果在宣告時未使用"var"進行宣告的話，解譯器會直接認為是全域變數(Negative case)

```javascript=
function getArea(width,height){
  var area = width* height;
  return area;
}

var wallSize = getArea(2,3);
document.write(wallSize);
```
* area 為區域變數
* wallSize為全域變數

## 物件
* 把一群變數和函式組合在一起
    * 在物件的變數稱為**特性**
    * 在物件的函式稱為**方法**
### 建立物件：實字標示法

```javascript=
var hotel = {
  name: 'Quay', //特性，這些是變數
  rooms: 40, //特性，這些是變數
  booked: 25, //特性，這些是變數
  gym: true, //特性，這些是變數
  roomTypes: ['twin','double','suite'], //特性，這些是變數
  checkAvailability : function (){     //方法，這是函式
    return this.rooms - this.booked; 
  } //方法，這是函式
  
  
//存取物件裡的方法
  var roomsFree = hotel.checkAvailability();
  //存取物件裡的特性 有兩種
  //1
  var rooms = hotel.rooms;
  //2
  var booked = hotel ['booked']; //方法不能這樣存取

```
* `this` 是用來表示此物件(hotel)中的rooms和booked特性
* 物件:
    * hotel

| 特性： | 鍵 | 鍵值 |
| ------ | ----| ---- |
|    | name| string
|    | rooms| number
|     |booked| number
|     |gym| boolean
|     |roomTypes| array
| 方法： |checkAvailability | function


#### 存取物件
```javascript=
  //存取物件裡的方法
  var roomsFree = hotel.checkAvailability();
  //存取物件裡的特性 有兩種
  //1
  var rooms = hotel.rooms;
  //2
  var booked = hotel ['booked']; //方法不能這樣存取
```
#### 方括號語法存取物件特性的使用時機為
```javascript
var booked = hotel ['booked'];
```

* 特性的名稱為數值型態(技術可行，但盡量避免)
* 以變數取代方括號中的特性名稱 (CH12時討論)

#### 以實字標示法建立物件
```javascript=
var hotel = {
  name : 'Quay',
  rooms : 40,
  booked : 25,
  checkAvailability : function() {
    return this.rooms - this.booked; // Need "this" because inside function
  }
};

// Update the HTML
var elName = document.getElementById('hotelName'); // Get element
elName.textContent = hotel.name;                   // Update HTML with property of the object

var elRooms = document.getElementById('rooms');    // Get element
elRooms.textContent = hotel.checkAvailability();   // Update HTML with property of the object

```

### 建立物件：建構子標示法
* 利用關鍵字`new`和物件建構子`Object()`可以建立一個空物件
* 可後續再為物件加入特性和方法
```javascript=
var hotel = new Object();

hotel.name = 'Park';
hotel.rooms = 120;
hotel.booked = 77;

hotel.checkAvailability = function() {
  return this.rooms - this.booked;  
};

//變更物件
hotel.name = 'Alice'; 
hotel['name']= 'Alice'; //方法不能這樣
delete hotel.name; //刪除一個特性
hotel.name = '';//將特性清除重設 
//變更物件
var elName = document.getElementById('hotelName'); // Get element
elName.textContent = hotel.name;                   // Update HTML with property of the object

var elRooms = document.getElementById('rooms');    // Get element
elRooms.textContent = hotel.checkAvailability();
```

### 建立多個物件：建構子標示法
* 用多個物件來表示類似的事情
* 以**函式**的方法，作為建立物件的模板
    * 模板要包含物件所需的特性和方法

```javascript=
function Hotel(name,rooms,booked){
  this.name = name;
  this.rooms = rooms;
  this.booked = booked;
  this.checkAvailability = function(){
    return this.rooms - this.booked;
  }
}
```
* `this`可取代物件名稱的描述
* 建構子函式成稱經常以**大寫**字母開頭，為了提醒需要使用時要加上`new`
### 呼叫建構子函式
```javascript=

var quayHotel = new Hotel ('Quay',40,25);
var parkHotel = new Hotel ('park',120,77);
var aliceHotel = new Hotel ('alice',111,50);

// Update the HTML for the page
var details1 = quayHotel.name + ' rooms: ';
    details1 += quayHotel.checkAvailability();
var elHotel1 = document.getElementById('hotel1');
elHotel1.textContent = details1;

var details2 = parkHotel.name + ' rooms: ';
    details2 += parkHotel.checkAvailability();
var elHotel2 = document.getElementById('hotel2');
elHotel2.textContent = details2;

var details3 = aliceHotel.name + ' rooms: ' + aliceHotel.checkAvailability();
    // details3 += aliceHotel.checkAvailability();
var elHotel3 = document.getElementById('hotel3');
elHotel3.textContent = details3;
```

### 增加或移除特性
```javascript=
var hotel = {
  name : 'Park',
  rooms : 120,
  booked : 77
};

//增加特性
hotel.gym = true;
hotel.pool = false;
//移除特性
delete hotel.booked;

// Update the HTML
var elName = document.getElementById('hotelName'); // Get element
elName.textContent = hotel.name;                   // Update HTML with property of the object

var elPool = document.getElementById('pool');      // Get element
elPool.className = hotel.pool;                     // Update HTML with property of the object

var elGym = document.getElementById('gym');        // Get element
elGym.className = hotel.gym;   
```
## THIS關鍵字

