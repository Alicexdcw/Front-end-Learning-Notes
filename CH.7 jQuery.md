---
tags: javascript, jquery , 前端
---
`參考書籍:javascript & jQuery 網站互動設計程式進化之道`
`#190716`

# Java Script自學筆記 - CH.7 jQuery

## What is jQuery?

Jquery is a JavaScript file, 可允許在頁面中引用

#### 1. 利用CSS樣式選擇器選取元件
* jQuery() 允許在頁面中找一個或多個的元件
* $() = jQuery()
* li.hot 選擇器
```javascript
$('li.hot')
```
#### 2. 利用JQUERY方法對元件進行操作
```javascript
$('li.hot').addClass('complete');
```
* addClass() -> 方法
* complere -> 參數

#### basic example
```javascript=
$(':header').addClass('headline'); //:header 所有<h1>~<h6>元件，在class屬性中加入headline
$('li:lt(3)').hide().fadeIn(500); //:lt(index) 所選取元件中，索引值小於index的元件  選取前三個項目，隱藏並淡出
//每一個li元件設定一個事件監聽器。當使用者點選時，便會觸發一個匿名函式將該元件自頁面中移除
$('li').on('click', function(){ 
  $(this).remove();
});
```
### 單一選取元件
```javascript
$('ul')
```
### 多選取元件
```javascript
$('li')
```
### 擷取資訊
```javascript
var content = $('li').html();
```
### 設定資訊
```javascript
$('li').html('Updated');
```

### 元件巡訪
```javascript
$('li em').addClass('seasonal');
$('li.hot').addClass('favorite');
```


### 鏈結語法
```javascript=
$('li[id!="one"]')
.hide()
.delay(500)
.fadeIn(1400); // semi-colon indicates end of chaining - can be writen on separate lines
```
# ??
```javascript=
$('li:first-child').addClass('next');
$('li.priority').addClass('highlight');
```

### 檢查頁面是否已準備完成`.ready()`
```javascript
$(document).ready(function(){
    //code
});
```
* $(document)會建立一個jQuery物件代表此頁面
* 當頁面已經預備好，在`.ready()`括號內的函式便會開始執行
#### document物件上的READY事件縮寫描述方式 
```javascript
$(function(){
    //code
});
```
### 比較
#### load事件
* 會啟動Load事件
* 已經被`.on()`取代
#### `.ready()`方法
* 檢查瀏覽器是否支援DOMContentLoaded事件，因為一但DOM載入完成，事件便會啟動(不會等所有要用的資源都載入才啟動)
#### 程式碼置於`</body>`之前
* 可以在程式碼執行前將HTML均載入至DOM中

* 還是會看到有些人用.ready()方法，因為這樣程式碼移至HTML頁面任何位置，軍仍可正確運作(This is a common situation,expectially codes is also offer to other)

### 取得元件內容 `.html()` `.text()`
* #### `.html()`
    * 當`.html()`自jQuery選取集合中**擷取資訊**時
    * 只會取得集合中**第一個元件**的HTML內容
    * 和他的子孫元件內容
    * EX1: 第一個元件的HTML內容 `ul`
    ```javascript
    $('ul').html();
    ```
    回傳結果
    ```html
    <li id="one" class="hot"><em>fresh</em> figs</li>
    <li id="two" class="hot">pine nuts</li>
    <li id="three" class="hot">honey</li>
    <li id="four">balsamic vinegar</li>
    ```
    * #### `ul` `.html()` `.append()`
    ```javascript
    $(function() {
      var $listHTML = $('ul').html();
      $('ul').append($listHTML);
    });
    ```
    * 選擇器會回傳`<ul>`元件
    * `.html()` 取得`ul`(包含4個`li`)
    * `.append()`
        * 將內容插入在選取元件之內，於元件的結束標籤`</li>`之前，在`<li>`元件之後

    ![](https://i.imgur.com/FVZtggo.png)

    
    * EX2:他的子孫元件內容 `li`
    ```javascript
    $('li').html();
    ```
    回傳結果
    ```html
    <em>fresh</em> figs
    ```
    * #### `li` `.html()` `.append()`
    ```javascript
    $(function() {
      var $listItemHTML = $('li').html();
      $('li').append('<i>' + $listItemHTML + '</i>');
    });
    ```
    ![](https://i.imgur.com/WmRlGmR.png)

    
* #### `.text()`
    * 擷取文字內容時，回傳集合裡**每個元件**中的文字內容
    * 和他的子孫元件內容
    * EX1: **每個元件**中的文字內容 `ul`
    ```javascript
    $('ul').text();
    ```
    回傳結果
    ```
    fresh figs 
    pine nuts 
    honey 
    balsamic vinegar
    ```
    * #### `ul` `.text()` `.append()`
    

    ```javascript
    $(function() {
      var $listText = $('ul').text();
      $('ul').append('<p>' + $listText  + '</p>');
    });
    ```
    ![](https://i.imgur.com/zZglLPq.png)
    * EX2:他的子孫元件內容 `li`
    ```javascript
    $('li').text();
    ```
    回傳結果:一個li內容空白會回傳，但是li和li之間的空白不會回傳
    ```html
    fresh figspine nutshoneybalsamic vinegar
    ```
    * #### `li` `.text()` `.append()`
    ```javascript
    $(function() {
      var $listItemText = $('li').text();
      $('li').append('<i>' + $listItemText + '</i>');
    });
    ```
    ![](https://i.imgur.com/hmpWKJh.png)

    * 要取得`<input>`.`<textarea>`元件內容，可用`.val()`
    

### 變更元件 `.html()` `.text()` `replaceWith()` `remove()` 
```javascript
$(function() {
  $('li:contains("pine")').text('almonds');
  $('li.hot').html(function() {
    return '<em>' + $(this).text() + '</em>';
  });
  $('li#one').remove();
});
```
* `$(this).text()`
    * this 會參考至目前的清單項目
    * $(this) 將該元件放置於一個新的jQuery物件，所以可以在該物件上操作jQuery方法

### 插入元件 `.before()` `.after()` `.prepend()` `.append()`

```
.before() <li> .prepend() item .append() </li> .after()
```
```javascript=
$(function() {
  $('ul').before('<p class="notice">Just updated</p>');
  $('li.hot').prepend('+ ');
  var $newListItem = $('<li><em>gluten-free</em> soy sauce</li>');
  $('li:last').after($newListItem);
});
```
![](https://i.imgur.com/984GGYM.png)

### 擷取和設定屬性值 `attr()` `removeAttr()` `addClass()` `removeClass()`

* #### `attr()`
    * **擷取或設定**一個指定的屬性和值
    * 要描述屬性名稱
    ```javascript
    $('li#one').attr('id');
    ```
    * **更變**一個屬性值
    * 除了描述屬性名稱外，還要指定一個新的值
    ```javascript
    $('li#one').attr('id','idName');
    //Example
    $('ul').attr('id', 'group');
    //result: <ul id="group"></ul>
    ```
* #### `removeAttr()`
    * 移除一個指定的屬性(和屬性值)
    ```javascript
    $('li#one').removeAttr('id');
    ```
* #### `addClass()`
    * 將一個新的屬性值加入現有的class，不會覆蓋現有的class
* #### `removeClass()`
    * 自class屬性中移除一個屬性值，保留其他屬性值
```javascript=
$(function() {
  $('li#three').removeClass('hot');
  $('li.hot').addClass('favorite');
  $('ul').attr('id', 'group');
});
```
![](https://i.imgur.com/LAjVaEI.png)

### 擷取和設定CSS特性 `.css()`
* `.css()`可**擷取**和**設定**CSS特性的值
* 擷取一個CSS特性
    * ```javascript
        var backgroundColor = $('li').css('backgroun-color');
      ```
* 設定一個CSS特性
    * ```javascript
        $('li').css('backgroun-color','#272727');
      ```
    * 需要以像素來描述尺寸大小時，可用`+=` `-=` 運算子表示值的增加和減少
        * ```javascript
            $('li').css('padding-left','+=20'); //左方填入空白間隔
          ```
          
* 設定多個特性
    * ```javascript
        $('li').css({
          'background-color':'#272727',
          'font-family':'Arial'
        });
      ```
EX: 
```javascript=
$(function() {

  // Get the background color of the first list item.
  var backgroundColor = $('li').css('background-color');

  // Write what the background color was after the list.
  $('ul').append('<p>Color was: ' + backgroundColor + '</p>');

  // Set several properties on all list items.
  $('li').css({
    'background-color': '#c5a996',
    'border': '1px solid #fff',
    'color': '#000',
    'text-shadow': 'none',
    'font-family': 'Georgia',
    'padding-left': '+=75'
  });
});
```
### 操作選取集合裡的每個單一元件`each()`
* #### `each()`
    * 對所選取的元件集合建立迴圈的功能
    * 參數是一個函式
    * 適用情況
        * 在選取集合中自每個元件擷取資料
        * 針對每個元件執行一系列的動作
* #### `this`
    * when you use `each()` 巡訪，可用`this` Key word 來存取目前元件
* #### `$(this)`
    * 用關鍵字`this`來建立一個新的jQuery選取集合，此集合會包含目前的元件
    * 後續便可以在目前的元件上使用jQuery方法(?)

```javascript=
$(function() {
  $('li').each(function() {
    var ids = this.id;
    $(this).append(' <span class="order">' + ids + '</span>');
  });
});
```
Explan:
* `$('li')`
    * 選取包含所有`<li>`的元件
* `.each()`
    * 對選取集合中的每個元件執行匿名函式 
* `function(){}` 
    * 此匿名函式對清單裡的每一個元件均會執行
*  `this`
    *  在迴圈中會參考至目前的元件節點
    *  用來擷取目前元件的id屬性，並儲存於變數ids中
* `$(this)`
    * 用來建立一個包含目前元件的jQuery object

* ##### `this` V.S `attr()`的擷取
    * use`ids = this.id;` is more better than `ids = $(this).attr('id')` beacuse 解譯器會需要先建立一個jQuery object ，接著才使用方法來存取屬性資訊


## 事件方法 `p.326`
### .on()方法可用來處理所有的事件
* 使用選擇器建立jQuery選取集合
* 使用.on()指定要回應的事件名稱。
```javascript
$('li').on('click',function(){
  $(this).addClass('complete');
});
```
* .on('欲回應事件',當事件發生時想要執行的程式碼)

### EVENT物件
* 每個事件處理函式都需要接收一個event事件
* 具有特性和方法可以告訴你所發生的事件其相關資訊

```javascript
$('li').on('click' function(e){
  eventType = e.type;
})
```
* `(e)` 
    * `e` 在小括號內，像參數一樣，此名稱在函式內就可以被使用，並參考至該event物件
    * 指定event物件一個參數名稱
* `e.type`的`e`
    * 在函式中使用該參數名稱以參考至event物件    
* `type`
    * 存取物件的特性和方法

* event物件特性
    * timeStampe
        * 紀錄事件發生的時間
        * 無法在Firefox使用
    * type
        * 紀錄被觸發事件的類型

EX:
```javascript=
$(function() {

  $('li').on('click', function(e) {
    $('li span').remove(); //在li元件內的<span>元件均先移除
    var date = new Date(); //建立一個DATE物件
    date.setTime(e.timeStamp);  //設定物件的時間為事件被觸發當下的時間
    var clicked = date.toDateString(); //將事件被觸發的時間轉換為具有可讀性的日期文字
    $(this).append('<span class="date">' + clicked + ' ' + e.type + '</span>');  //將典籍事件的時間寫入至清單項目的文字內容之後
  });

});
```

### 事件處理器的其他參數(委派事件)
### 特效
### 動畫效果呈現css特性
### 巡訪DOM
### 於選取元件集合中加入或篩選元件
### 依序尋找元件(運用索引值)

### 表單的運用操作
**step1. build jquery object:**
        newItemButton newItemForm textInput
**step2. 設定一開始進入的頁面:**
        按鈕顯示，表單隱藏
**step3. 當點擊"new item"按鈕時的畫面顯示:**
        按鈕隱藏，表單顯示
        
**step4. 當表單送出後的動作:**
        1.當表單送出時，呼叫一匿名函式並傳入event事件 
        2.停止表單送出的動作 
        3.擷取使用者輸入文字並儲存於變數中 
        4.將一個新的清單項目利用.after()方法，加入至清單的尾端 
        5.送出後的畫面顯示:表單隱藏，按鈕顯示，textinput(文字框)的顯示內容
        
* HTML
```htmlmixed=
    <div id="page">
      <h1 id="header">List</h1>
      <h2>Buy groceries</h2>
      <ul>
        <li id="one" class="hot"><em>fresh</em> figs</li>
        <li id="two" class="hot">pine nuts</li>
        <li id="three" class="hot">honey</li>
        <li id="four">balsamic vinegar</li>
      </ul>
      <div id="newItemButton"><button href="#" id="showForm">new item</button></div>
      <form id="newItemForm">
        <input type="text" id="itemDescription" placeholder="Add description..." />
        <input type="submit" id="addButton" value="add" />
      </form>
    </div>
```
* JS
```javascript=
$(function() {
//step1. build jquery object
  var $newItemButton = $('#newItemButton'); //建立jQuery物件
  var $newItemForm = $('#newItemForm');
  var $textInput = $('input:text'); //:text 所有文字元件
//step2. 設定一開始進入的頁面:按鈕顯示，表單隱藏
  $newItemButton.show();
  $newItemForm.hide();
//step3. 當點擊"new item"按鈕時的畫面顯示:按鈕隱藏，表單顯示
  $('#showForm').on('click', function(){  //.on('欲回應事件',當事件發生時，想要執行的程式碼) 使用者互動
    $newItemButton.hide();
    $newItemForm.show();
  });
//step4. 當表單送出後的動作
  $newItemForm.on('submit', function(e){   //submit 表單送出。當表單送出時，會呼叫一匿名函式並傳入event事件
    e.preventDefault();  //preventDefault() 停止表單送出的動作
    var newText = $textInput.val(); //var newText = $textInput.val(); //$textInput = $('input:text')   :text 挑出所屬性質為text的<input>元件  .val()擷取使用者所輸入的文字，儲存於變數newText中
    $('li:last').after('<li>' + newText + '</li>');  //將一個新的清單項目利用.after()方法,加入至清單的尾端
    $newItemForm.hide();
    $newItemButton.show();
    $textInput.val('');
  });

});
```

### 程式腳本置放的位置
* `</body>`之前
    * 程式碼載入並不阻礙其他頁面的載入
    * DOM在程式腳本執行前均已載入完成