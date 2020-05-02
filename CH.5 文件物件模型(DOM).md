---
tags: javascript, DOM , 前端
---
`參考書籍:javascript & jQuery 網站互動設計程式進化之道`
`#190623`

# Java Script自學筆記 - CH.5 文件物件模型(DOM)

## DOM (document object model)
* DOM 不屬於HTML也不屬於JavaScript，他是額外定義的一個規則
    * 建立HTML頁面模型 `p.184`
    * 存取和變更HTML頁面內容

## DOM樹 是網頁的模型
* 當瀏覽器載入網頁內容後，他會建立該頁的文件模型，此模型就稱為DOM tree；DOM tree會儲存在瀏覽器的記憶體中，並包含四種主要的類型的節點。
    1. 文件節點
    2. 標籤元件節點
    3. 屬性節點
    4. 文字節點

* 每一個節點都是一個物件，具備方法和特性。程式碼會存取和變更這個DOM樹(而非HTML檔案的原始檔)

## 活用DOM樹
### Sept1. 定位你所要尋找的標籤元件節點(存取元件)
* 選取一個標籤元件節點
    * [`getElementByID()`](#getElementById)
    * [`querySelector()`](#querySelector)
        * `querySelector('li.hot')`
        * 利用CSS選擇器，回傳遞**一個**符合條件的元件
    * 也可以藉由在巡訪DOM樹時，自節點間移動到指定的節點位置後，再進行選取
* 選取多個標籤元件節點
    * [`getElementsByClassName()` ](#getElementsByClassName)
    * [`getElementsByTagName()`](#getElementsByTagName)
    * [`querySelectorAll()`](#querySelectorAll)
        * 利用CSS選擇器選取**所有符合條件**的元件
        * 回傳一個節點"串列"
* 節點間的巡訪
    * 可以從一個標籤元件節點移動到與他相關的元件節點
    * [`parentNode`](#parentNode)
        * 選取目前元件節點的**父節點**(只會回傳一個)
    * [`previousSibling/nextSibling`](#previousSibling)
        * 選取前一個/後一個**兄弟節點**
    * [`firstChild/lastChild`](#firstChild)
        * 選取目前元件節點的第一個/最後一個**子節點**


### Sept2. 使用他的文字內容、子標籤元件和屬性(操作元件)

* 存取/變更文字節點
    * `<li class="attribute">text</li>`
    * 任何元件中的文字都是儲存在文字節點中
    * 1. 選取`<li>`元件節點
      2. 使用 firstChild
      3. 使用只有文字節點才具備的特性(`nodeValue`)已取得元件裡的文字內容
      * nodeValue
          * `document.getElementByClass('attribute').firstChild.nodeValue;`
          * 可允許存取或更新一個文字節點的內容

* 操作HTML文件內容
    * 允許存取此子元件、文字內容
        * `innerHTML`
    * [只存取文字內容 ](#2.操作容納節點)
        * `textContent`
    * [建立、加入、移除節點至DOM樹中 ](#利用DOM控制處理加入元件)
        *    `createElement()`.`createTextNode()`.`appendChild()`/`removeChild()`
* [存取或變更屬性值](#屬性節點)
    * [`className/id`](#建立屬性與變更屬性質)
        * 允許取得或更變class/id的屬性值
    * [`hasAttribute()`](#檢查屬性與擷取屬性值)
        * 可檢查**一個**屬性是否存在
    * [`getAttribute()`](#檢查屬性與擷取屬性值)
        * 取得屬性值
    * [`setAttribute()`](#建立屬性與變更屬性質)
        * 更變或設定屬性值
    * [`removeAttribute()`](#利用DOM控制處理移除元件)
        * 移除一項屬性

E.g

#### getElementById
```javascript=
// 存取元件
var el = document.getElementById('one');
// 操作元件
el.className = 'cool';
```
#### getElementsByTagName
* 取到的值
0 `<li id="one" class="hot">`
1 `<li id="two" class="hot">`
2 `<li id="three" class="hot">`
3 `<li id="four">`
```javascript=
var elements = document.getElementsByTagName('li');
    if (elements.length > 0){
        var el = elements[0];
        el.className = 'cool';
    }
```

#### getElementsByClassName
```javascript=
var change = document.getElementsByClassName('hot');

if (change.length > 2){
    var el = change[2]; 
    //var el = change.item(2);
    el.className = 'cool';
}

```
#### querySelectorAll (一起變色)
```javascript=
var hotItems = document.querySelectorAll('li.hot');
//三個class="hot"的都會變色

////var hotItems = document.querySelectorAll('li');
   //所有li具有id屬性的都會被回傳，不論id的屬性值為何
if (hotItems.length > 0){
  for( var i = 0; i < hotItems.length; i++) {
      hotItems[i].className='cool';
  }
}
```
#### querySelector 
* querySelector & querySelectorAll
```javascript=
// querySelector 僅回傳第一個符合條件的元件
var el = document.querySelector('li.hot');
el.className = 'cool';
//這時 第一個清單已經變成cool <li id="one" class="cool">

// querySelectorAll 回傳所有符合條件的節點串列
// 第二個符合條件的元件(第三個清單選項)被選取、更變
// 所以querySelectorAll只取到第二.三個清單
var els = document.querySelectorAll('li.hot');
els[1].className = 'cool';

//result. 第1.3項被更改
```
### 對整個節點串列執行相同操作 (巡訪節點串列)
#### querySelectorAll 
* 一起變色
* 一旦節點串列被建立後，for迴圈就可以用以巡訪串列中的每個元件
* 節點串列的lenght特性可顯示傳列中的節點項目數量。節點數量也就會是迴圈所需執行的次數
```javascript=
var hotItems = document.querySelectorAll('li.hot'); //將節點串列儲存於陣列中

//var hotItems = document.querySelectorAll('li');
   //所有li具有id屬性的都會被回傳，不論id的屬性值為何
if (hotItems.length > 0){
  for( var i = 0; i < hotItems.length; i++) { //巡訪
      hotItems[i].className='cool'; //更變class屬性
  }
}

//Result. 三個class="hot"的都會變色
```
### DOM巡訪
e.g.
```htmlmixed
<ul>
  <li></li>
  <li></li>
  <li></li>
  <li></li>
</ul>
```
#### parentNode
* 選取目前元件節點的**父節點**(只會回傳一個)
    * e.g. 從`<li>`開始尋找，則他的父節點為`<ul>`
    
#### previousSibling/nextSibling
* 選取前一個/後一個**兄弟節點**
    * e.g. 從第一個`<li>`開始找，後一個兄弟節點就是第二個`<li>`
 ```javascript=
// Select the starting point and find its siblings.
var startItem = document.getElementById('two');
var prevItem = startItem.previousSibling;
var nextItem = startItem.nextSibling;

// Change the values of the siblings' class attributes.
prevItem.className = 'complete';
nextItem.className = 'cool';
```
#### firstChild
* 選取目前元件節點的第一個/最後一個**子節點**
    * e.g. 從`<ul>`開始找第一個的話就是第一個`<li>`最後一個的話就是第四個`<li>`
```javascript=
// Select the starting point and find its children.
var startItem = document.getElementsByTagName('ul')[0];
var firstItem = startItem.firstChild;
var lastItem = startItem.lastChild;

// Change the values of the children's class attributes.
firstItem.className = 'complete';
lastItem.className = 'cool';

```




### 空白節點
* DOM巡訪有時候會遇到困難，因為部分瀏覽器在遇到兩標千元件之間的空白元件時，會自動加入一個文字節點
* previousSibling/nextSibling/firstChild/lastChild 這些特性會導致回傳不同的元件
* 要避免這個問題就要避免使用這些特性，取而代之的就是jQuery

### 選取/變更原件節點

#### 1.移動至文字節點
* 適用於元件只包含文字
* 使用`nodevalue`
```htmlmixed
<ul>
    <li id="one" class="hot"><em>fresh</em> figs</li>
</ul>
```
```javascript
var displaytext= document.getElementById('one').firstChild.nextSibling.nodeValue;
console.log(displaytext);
```

### 存取.變更文字節點
```javascript=
var itemTwo = document.getElementById('two');  // Get second list item

var elText  = itemTwo.firstChild.nodeValue;    // Get its text content

elText = elText.replace('pine nuts', 'kale');  // Change pine nuts to kale

itemTwo.firstChild.nodeValue = elText;         // Update the list item
```

#### 2.操作容納節點
* 適用於同時包含文字節點和子節點
* 使用`textcontent`.`innertext`
* `textcontent`允許將容納元件裡(和他的子節點)的文字擷取或更新
* `innertext` 盡量少用

```htmlmixed
<li id="one" class="hot"><em>fresh</em> figs</li>
```
* `textcontent`會蒐集到文字fresh.figs
* `innerHTML` 只會顯示figs，因為CSS規則會隱藏`<em>`
```javascript=
var firstItem = document.getElementById('one');
var showTextContent = firstItem.textContent;

var msg = '<p>顯示textContent:' + showTextContent + '</p>';
var el = document.getElementById('scriptResults');
el.innerHTML = msg; //innerHTML不要用

firstItem.textContent = 'sourdough bread';       // Update the first list item
```

### 利用DOM控制處理加入元件


* Step1: 建立元件 `createElement()`
* Step2: 提供內容 `createTextNode()`
    * 會建立一個新的文字節點，將這個節點儲存在一個變數中
    * 如果想要一個空的元件加入至DOM樹可省略其步驟
* Step3: 加入至DOM `appendChild()`
    * 允許指定將元件加入至任一樹節點下，以作為他的子節點

```javascript=
// Create a new element and store it in a variable.
var newEl = document.createElement('li');

// Create a text node and store it in a variable.
var newText = document.createTextNode('quinoa');

// Attach the new text node to the new element.
newEl.appendChild(newText);

// Find the position where the new element should be added.
var position = document.getElementsByTagName('ul')[0];

// Insert the new element into its position.
position.appendChild(newEl);
```
---
[問題](https://stackoverflow.com/questions/27079598/error-failed-to-execute-appendchild-on-node-parameter-1-is-not-of-type-no)
Your function is returning a string rather than the `div` node. `appendChild` can only append a node.

For example, if you try to appendChild the string:
```javascript=
var z = '<p>test satu dua tiga</p>'; // is a string 
document.body.appendChild(z);
```

The above code will never work. What will work is:
```javascript=
var z = document.createElement('p'); // is a node
z.innerHTML = 'test satu dua tiga';
document.body.appendChild(z);
```
---



### 利用DOM控制處理移除元件
* Step1: 將要移除的元件節點儲存於變數中 
* Step2: 將該節點的**父節點**儲存於變數中
    * `<ul>`為該節點的父節點，即為**容納節點**
    * `<li>` 為欲移除的元件節點
    
* Step3: 加入至DOM `appendChild()`
    * 允許指定將元件加入至任一樹節點下，以作為他的子節點


```htmlmixed
 <ul>
     <li id="one" class="hot"><em>fresh</em> figs</li>
     <li id="two" class="hot">pine nuts</li>
     <li id="three" class="hot">honey</li>
      <li id="four">balsamic vinegar</li>
</ul>
```
```javascript=
// Store the element to be removed in a variable.
var removeEl = document.getElementsByTagName('li')[3];

// Find the element which contains the element to be removed.
var containerEl = document.getElementsByTagName('ul')[0];

// Remove the element.
containerEl.removeChild(removeEl);
```

### 屬性節點
* `hasAttribute()`
     * 可檢查**一個**屬性是否存在
* `getAttribute()`
     * 取得屬性值
* `setAttribute()`
    * 更變或設定屬性值
* `removeAttribute()`
    * 移除一項屬性
* `className/id`
  * 允許取得或更變class/id的屬性值
#### 檢查屬性與擷取屬性值
* `hasAttribute()`
     * 可檢查**一個**屬性是否存在
     * 可將屬性名稱作為引數
* `getAttribute()`
     * 取得屬性值

```javascript=
var firstItem = document.getElementById('one'); // Get first list item 
if (firstItem.hasAttribute('class')) {          // If it has class attribute
  var attr = firstItem.getAttribute('class');   // Get the attribute

  // Add the value of the attribute after the list
  var el = document.getElementById('scriptResults');
  el.innerHTML = '<p>The first item has a class name: ' + attr + '</p>';
}
```

#### 建立屬性與變更屬性質
* `className`
* `setAttribute()` (需有兩個參數)
* 只會覆蓋完本的class，不會增加新的class
* 要增加新的屬性要使用jQuery的`addClass()`
    * (參數名稱,欲指定的參數)
```javascript=
var firstItem = document.getElementById('one'); // Get the first item
firstItem.className = 'complete';               // Change its class attribute

var fourthItem = document.getElementsByTagName('li').item(3);// Get fourth item
fourthItem.setAttribute('class', 'cool');       // Add an attribute to it
```

#### 移除屬性
* `removeAttribute()`
```javascript=
var firstItem = document.getElementById('one'); // Get the first item
if (firstItem.hasAttribute('class')) {          // If it has a class attribute
  firstItem.removeAttribute('class');           // Remove its class attribute
}
```