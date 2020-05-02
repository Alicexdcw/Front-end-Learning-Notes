---
tags: javascript, DOM , 前端
---
`參考書籍:javascript & jQuery 網站互動設計程式進化之道`
`#190623`

# Java Script自學筆記 - CH.6 事件
* 互動建立事件
* 事件觸發程式碼
* 程式碼回應使用者

### 術語
* 事件啟動(Fire)或喚起(Raised)
    * 當事件發生後，通常稱為事件被Fire or Raised 
        * e.g. 若使用者輕觸頁面連結，則瀏覽器的click事件會被Fire
* 事件觸發程式腳本
    * 事件可以觸發一個函式或程式腳本。
        * e.g. 元件的click事件啟動，他便會觸發一個程式腳本將所點選的元件放大

### 事件如何觸發js程式碼

1. 選取元件
2. 指定事件 a.k.a **繫結**一個事件至DOM節點
    * HTML事件處理器 (勿採用)
    * [傳統DOM事件處理器](#傳統的DOM事件處理器)
    * [DOM Level 2 事件監測器](#事件監聽器)
    
3. 呼叫程式碼

### 傳統的DOM事件處理器
* 一個事件處理器只能夠繫節**一個**函式
* 利用事件處理器將事件繫節至元件上，並且指定事件啟動時需要的函式
```javascript
//model
element.onevent = functionName;
//ex.
el.onblur = checkUsername;
```
* element 
    * 元件
    * **目標**的DOM元件節點
* onevent
    * 事件
    * 繫結到元件的事件名稱，並在名稱前加上字詞`on`
* functionName
    * 要執行的函式名稱(不需要加小括號)

```javascript=
function checkUsername(){
//檢查使用者名稱的長度
  var el = document.getElementById('username'); //DOM"元件"節點的參考值，一般會儲存於變數
  el.onblur = checkUsername; 
  //onblur "事件"名稱之前會加上字詞on以作為屬性名稱
  //checkUsername "函式"
}
```
* 使用DOM事件處理器
```javascript=
// Declare function 宣告函式
// Get feedback element 取得id屬性值為feedback的元件
// If username too short 
// Set msg
// Otherwise
// Clear message

// Get username input
// When it loses focus call checkuserName()

function checkUsername(){
  var elMsg = document.getElementById('feedback');
  if(this.value.length < 5){
    elMsg.textContent = 'usename必須大於5';
  }else{
    elMsg.textContent='';
  }
}

var elUsername = document.getElementById('username');
elUsername.onblur = checkUsername;
```


### 事件監聽器  
* (舊版瀏覽器不支援)
* 一次呼叫**多個**函式
* `addEventListener` 加入監聽器

```javascript
//model
element.addEventListener('event',functionName[,Boolean]);
//ex.
elUsername.addEventListener('blur',checkUsername, false);
```
```javascript
function checkUsername(){
    //檢查使用者名稱的長度
}
var el = document.getElementById('username');
elUsername.addEventListener('blur',checkUsername, false);
```

* element
    * 元件
    * **目標**的DOM元件節點
* 'event'
    * 要繫結至點的事件，要用引號`''`標示
    * 事件：`focus/focusin`　→ 元件取得焦點 
            `blur/focusout`　→ 元件失去焦點
* functionName
    * 不用括號
* [,Boolean]
    * 指定事件流程模式，一般設定為`false`



```javascript=
//建立一個監聽器，如果字串小於五的話，顯示文字
function checkUsername(){
  var elMsg = document.getElementById('feedback');
  if(this.value.length < 5){
    elMsg.textContent = '至少大於五';
  }else{
    elMsg.textContent = '';
  }
}
//將username綁上監聽器
var elUsername = document.getElementById('username');
elUsername.addEventListener('blur',checkUsername, false);
```

### DOM V.S. 監聽器
* DOM
```javascript
var elUsername = document.getElementById('username');
//element.onevent = functionName;
elUsername.onblur = checkUsername;
```
* 監聽器
```javascript
var elUsername = document.getElementById('username');
//element.addEventListener('event',functionName[,Boolean]);
elUsername.addEventListener('blur',checkUsername, false);
```

### 於事件處理器和監聽器中使用需傳入參數的函式
* 要傳入參數的話，必須將函式包裹在一個匿名函式中

```javascript
elUsername.addEventListener('blur', function() { 
  checkUsername(5); 
}, false);
```

```javascript=
// Username input
// Error msg element

// Declare function
// If username too short
// Otherwise
// Clear msg

// When it loses focus
// Pass argument here

  var elUsername = document.getElementById('username');
  var elMsg = document.getElementById('feedback');

  function checkUsername(minLength){
    if(elUsername.value.length < minLength){
      elMsg.textContent = '使用者名稱長度需小於' + minLength + '!!';
    }else{
      elMsg.textContent = '';
    }
  }

  elUsername.addEventListener('blur',function(){
    checkUsername(7);
  },false);
```