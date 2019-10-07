---
tags: javascript, AJAX, JSON, 前端
---
`參考書籍:javascript & jQuery 網站互動設計程式進化之道`
`#190822`

# Java Script自學筆記 - CH.8 AJAX and JSON

---
給自己的NOTE: 以後先看最後的範例，會比較有成功的畫面
---
## 本章內容功能:
1. [點擊一個nav的項目，網址列不變](#運用JQUERY載入HTML內容至頁面)
2. 點擊一個頁面內容(課程活動)，就會自動出現課程內容資訊(都儲存在一個html檔orXML or JSON中)
3. [點到哪個nav項目，哪個就換顏色](#點到哪個導覽列項目就換顏色)(不用再每個頁面都改nav的顏色啦啦啦!!以前都要一個頁面一個頁面改class，知道了這個之後就不用啦!!趕快學起來!!) 
* MyNote: 我自己目前覺得有點像Angular等等其他JS的Framwork一樣(可能就是這個功能 哈哈) 點擊一個nav的項目，網址列不變那種

## What is AJAX

* 可向伺服器要求資料，不須重新載入整個頁面
* 通常以JS物件標示法的格式(就是JSON-JavaScript Object Notation)進行傳遞
* 是 <font color ="blue"> **非同步處理的模型**</font>
    * 瀏覽器能夠從伺服器要求資料，一旦送出資料請求，使用者可以在瀏覽器等待資料載入時，仍然可以繼續進行其他的操作
    * 瀏覽器不用等對第三方回傳資料後再顯示
    * 當伺服器回傳資料時，便會啟動事件(就如頁面載入完成後便啟動`load`事件)
    * 同步處理:當瀏覽器處理`<script>`時，傳統上會先停止頁面其他內容的處理，直到程式碼都載入完畢
* 目前AJAX名詞已用於泛指在瀏覽器中可以提供非同步功能機制的一個系統技術   


### AJAX技術的範例
1. 線上搜尋功能(自動填寫功能)，在搜尋框中搜尋輸入文字，還沒打完之前就會有參考的字
![](https://i.imgur.com/Eeq1JUv.png)

2. 註冊的時候檢查帳號是否用過
3. 購物車資訊的更新

## 資料格式: JSON.HTML.XML
* 伺服器可回傳 [JSON](#JSON).[HTML](#HTML).[XML](#XML)
    * #### [JSON](#以AJAX載入JSON資料)
        * JS物件標示法，資料可以利用JSON格式進行編排
        * 並不是一個物件
        * 只是一個**單純的文字資料**
        * HTML其實也只是單純的文字資料，只是瀏覽器把他轉換為DOM物件
        * 格式
            ```json
            "location":"San Francisco, CA",
            "capacity":270,
            "booking":ture,
            ```
            * **鍵**，需至於雙引號中 `e.g. "location"`
            * **鍵值**，可以是任一資料型別 `e.g. array.object.null`
        * `JSON.parse()` 可將JSON資料轉換為JS物件
            * 讓瀏覽器可以操作運用
        * `JSON.stringiry()` 可將JS物件轉換為字串 
            * 可將JS的物件從瀏覽器傳到其他應用程式
        ```json=
        {
          "events": [
            {
              "location": "San Francisco, CA",
              "date": "May 1",
              "map": "img/map-ca.png"
            },
            {
              "location": "Austin, TX",
              "date": "May 15",
              "map": "img/map-tx.png"
            },
            {
              "location": "New York, NY",
              "date": "May 30",
              "map": "img/map-ny.png"
            }
          ]
        }
        ```
        * `"events":[]` 為一陣列
        * 中間為三個物件，儲存於名為events的陣列中
        * 物件也可以只用一行描述
            * `{"location": "New York, NY","date": "May 30","map": "img/map-ny.png"}`
        * 優點
            * 資料請其來源可以是任何其他網站 ([CORS跨來源資源共用](https://developer.mozilla.org/zh-TW/docs/Web/HTTP/CORS))
        * 缺點
            * 因為是JS的一種，所以具有包含惡意內容的風險


    * #### [HTML](#以AJAX載入HTML資料)
        * 缺點
            * 只能以瀏覽器呈現
    * #### [XML](#以AJAX載入XML資料)
        * 可延伸性標籤語言
            * 標籤可以自行定義 `e.g. <location></location>`
        * 非常具有彈性的資料格式

### AJAX如何運行
![](https://i.imgur.com/HKouWRP.png)
### 以AJAX載入HTML資料
**EX:**
```javascript=
var xhr = new XMLHttpRequest();                 // Create XMLHttpRequest object

xhr.onload = function() {                       // When response has loaded
  // The following conditional check will not work locally - only on a server
  if(xhr.status === 200) {                       // If server status was ok
    document.getElementById('content').innerHTML = xhr.responseText; // Update
  }
};

xhr.open('GET', 'data/data.html', true);        // Prepare the request
xhr.send(null);                                 // Send the request
```
#### 1. 資料請求
* 瀏覽器向伺服器發送資料要求，會實作一個XMLHttpRequest(請求)物件
```javascript
var xhr = new XMLHttpRequest(); //請求object

xhr.open('GET', 'data/test.json', 'ture');//開始準備請求的發送
xhr.send(null);//將準備好的資料請求發送出去
```
*  `new XMLHttpRequest()`
    *  建立一個XMLHttpRequest物件的實例(發出請求)
        * (利用建構子標示法)
        ```javascript
        var hotel = new Object();
        hotel.name='Quay';
        hotel.rooms=40;
        ```
*  `open()` 
      *  開始**準備**請求的發送
      *  `open('HTTP方法','需要處理資料請求頁面的URL','boolen是否以非同步方式進行')`
      *  HTTP方法有: `GET` 和 `POST`
*  `send()`
      *  將準備好的資料請求**發送**出去    
      *  可提供額外的資訊傳遞給伺服器端
    
#### 2. 資料回應
* 當伺服器回應資料後，瀏覽器便會啟動一個事件進行處理
```javascript
xhr.onload = function(){
  if(xhr.status === 200)
  document.getElementById('content').innerHTML = xhr.responseText;//處理伺服器回傳資料的程式碼
}
```
* `.onload`
    * 當瀏覽器從伺服器接受並載入資料後，`onload`事件便會啟動，觸發一個函式

* `status`
     * 當伺服器回應資料請求時，要回傳一個狀態碼訊息，以說明請求已經回復完成
         * 200  伺服器以回應且無誤 
         * 304  未修改
         * 404  找不到頁面
         * 500  伺服器內部錯誤
* 更新頁面 `document.getElementById('content').innerHTML = xhr.responseText;`
    * `document.getElementById('content')`
        * 選取id屬性為content的元件
    *  `.innerHTML`
        *  將伺服器端回應的新HTML內容取代元件到現有的內容
        *  使用這個必須確認伺服器不會回傳惡意的內容(XSS攻擊)
    * 以 `XMLHttpRequest`物件的`responseText`特性取得新的HTML內容



### 以AJAX載入JSON資料

* 當伺服器回應時，JSON格式的資料必須轉換為HTML

**EX:**
```javascript=
var xhr = new XMLHttpRequest();                 // Create XMLHttpRequest object

xhr.onload = function() {                       // When readystate changes
  // The following conditional check will not work locally - only on a server
  //if(xhr.status === 200) {                      // If server status was ok
    responseObject = JSON.parse(xhr.responseText);

    // BUILD UP STRING WITH NEW CONTENT (could also use DOM manipulation)
    var newContent = '';
    for (var i = 0; i < responseObject.events.length; i++) { // Loop through object
      newContent += '<div class="event">';
      newContent += '<img src="' + responseObject.events[i].map + '" '; //img要哪一張
      newContent += 'alt="' + responseObject.events[i].location + '" />'; //alt要哪一個
      newContent += '<p><b>' + responseObject.events[i].location + '</b><br>'; //p.b s內容要為何
      newContent += responseObject.events[i].date + '</p>'; //日期
      newContent += '</div>';
    }
    
    //////////////////上面迴圈跑完會變
    /*
    <div class="event">
      <img src="img/map-ca.png" alt="Map of California" />
      <p><b>San Francisco, CA</b><br>
      May 1</p>
    </div>
    <div class="event">
      <img src="img/map-tx.png" alt="Map of Texas" />
      <p><b>Austin, TX</b><br>
      May 15</p>
    </div>
    <div class="event">
      <img src="img/map-ny.png" alt="Map of New York" />
      <p><b>New York, NY</b><br>
      May 30</p>
    </div>
    */
    

    // Update the page with the new content
    document.getElementById('content').innerHTML = newContent;

  //}
};

xhr.open('GET', 'data/data.json', true);        // Prepare the request
xhr.send(null);                                 // Send the request

// When working locally in Firefox, you may see an error saying that the JSON is not well-formed.
// This is because Firefox is not reading the correct MIME type (and it can safely be ignored).

// If you get it on a server, you may need to se the MIME type for JSON on the server (application/JSON).
```
* **反序列化(deserializing)**
    * 當JSON資料從伺服器端傳到瀏覽器端的時候，是以字串的形式傳送
    * 所以到達瀏覽器端的時候，必須將字串轉換為JS物件
    * 使用內建JSON物件的`parse()`方法
        * 這是一個全域的物件，所以不需要先建立他的實例

* **資料回應**

    1. `responseObject = JSON.parse(xhr.responseText);`
        * 將字串轉換為JS物件
    2. `var newContent = '';`
         * 建立新的字串內容，也可用DOM處理
         * `newConent`變數是用以儲存新的HTML資料
             * 初始設定為空字串，當程式碼進入迴圈時，便會加入新的字串內容
    3. `for(){}`
        * 巡訪每個代表活動事件的物件
    4. `document.getElementById('content').innerHTML = newContent;`
        * 將新的HTML內容加入頁面
        * HTML:直接使用`responseText`更新 
            * `document.getElementById('content').innerHTML = xhr.responseText;`


### 以AJAX載入XML資料

**EX:**

```javascript=
var xhr = new XMLHttpRequest();        // Create XMLHttpRequest object

xhr.onload = function() {              // When response has loaded
 // The following conditional check will not work locally - only on a server
 // if (xhr.status === 200) {             // If server status was ok

  // THIS PART IS DIFFERENT BECAUSE IT IS PROCESSING XML NOT HTML
  var response = xhr.responseXML;                      // Get XML from the server
  var events = response.getElementsByTagName('event'); // Find <event> elements

  for (var i = 0; i < events.length; i++) {            // Loop through them
    var container, image, location, city, newline;      // Declare variables
    container = document.createElement('div');          // Create <div> container
    container.className = 'event';                      // Add class attribute

    image = document.createElement('img');              // Add map image
    image.setAttribute('src', getNodeValue(events[i], 'map'));
    image.setAttribute('alt', getNodeValue(events[i], 'location'));
    container.appendChild(image);

    location = document.createElement('p');             // Add location data
    city = document.createElement('b');
    newline = document.createElement('br');
    city.appendChild(document.createTextNode(getNodeValue(events[i], 'location')));
    location.appendChild(newline);
    location.insertBefore(city, newline);
    location.appendChild(document.createTextNode(getNodeValue(events[i], 'date')));
    container.appendChild(location);

    document.getElementById('content').appendChild(container);
  }
// }

  function getNodeValue(obj, tag) {                   // Gets content from XML
    return obj.getElementsByTagName(tag)[0].firstChild.nodeValue;
  }

 // THE FINAL PART IS THE SAME AS THE HTML EXAMPLE BUT IT REQUESTS AN XML FILE
};

xhr.open('GET', 'data/data.xml', true);             // Prepare the request
xhr.send(null);                                     // Send the request
```

### 運用來自其他伺服器的資料
### JSONP
### JQUERY 與 AJAX:資料請求
* `.load()`
    * 載入HTML片段內容到元件中
* [`$.get()`](#JQUERY和AJAX:資料請求)
    * 以HTTP GET 方法載入資料
    * 向伺服器**請求**資料
    * `$.get('votes.php', queryString, function(data) {
    $('#selector').html(data);
  }`
      * `queryString` 傳送伺服器端的資料(字串或JSON)
* [`$.post()`](#運用AJAX傳送表單)
    * 以HTTP POST 方法載入資料
    * **發送**資料，以更新伺服器的資料
* `$.getJSON()`
    * GET請求載入JSON資料
    * 取得JSON資料
* `$.getScript()`
    * GET請求載入和執行JS資料
    * 取得JS(ex:JSONP)資料
* `$.ajax()`
    * 可執行任何需求


### JQUERY 與 AJAX:資料回應
* 當使用`.load()`方法時，自伺服器端所回傳的HTML資料會直接置入jQuery選取集合
* 當資料是以jqXHR物件取得的，需要指定資料後續處理的方式

* JQXHR特性
    * `responseText`
    * `responseXML`
    * `status`
    * `statusText`
* [JQXHR方法](#載入JSON與處理AJAX錯誤)
    * `.done()`
    * `.fail()`
    * `.always()`
    * `.abort()`


### 運用JQUERY載入HTML內容至頁面

**EX:**
```javascript=

$('nav a').on('click', function(e) {                 // User clicks nav link
  e.preventDefault();                                // Stop loading new link
  var url = this.href;                               // Get value of href

  $('nav a.current').removeClass('current');         // Clear current indicator
  $(this).addClass('current');                       // New current indicator

  $('#container').remove();                          // Remove old content
  $('#content').load(url + ' #container').hide().fadeIn('slow'); // New content
});
```

* `.load`
    * 用於從伺服器端載入HTML內容
    * 當伺服器回應時，HTML內容便會載入至jQuery選取集合中
    * ` $('#content').load('jq-load3.html #content');`
        * `$('#content')` 建立一個jQuery物件，包含`id=content`的元件  `content`為外層
        * `jq-load3.html`指定想要載入的URL位址
        * `#content` 指定需載入的URL的HTML內容區塊
    * 它几乎与 `$.get(url, data, success)` 等价，不同的是它不是全局函数，并且它拥有隐式的回调函数 
    * 允许我们规定要插入的远程文档的某个部分，和`$get()`不同
* `e.preventDefault();`
    * 停止超連結，將user帶往新頁面的動作執行(先停止轉往目標頁面)
    * 是避免原本的動作執行
#### 點到哪個導覽列項目就換顏色
* `var url = this.href;`
    * 擷取href的屬性值
    * 名稱為url的變數，會儲存要載入頁面的URL位址
* `$('nav a.current').removeClass('current');`
    * 移除目前頁面元件的current類別
* ` $(this).addClass('current');  `
    * 將被點擊的元件加入current類別
---
* `$('#container').remove();`
    * 移除元件內有的內容
* `$('#content').load(url + ' #content');`
    * 後面的#content必須空 一格
    * `url`這個參數必須用到，所以用加上字串的方式寫參數
---
**Step:**
1. User clicks nav link
2. Stop loading new link
3. Get value of href
4. Clear current indicator
5. New current indicator
6. Remove old content
7. New content
---

### JQUERY和AJAX:資料請求 
#### `$.get()`
`p.392`

```javascript=
$('#selector a').on('click', function(e) {
  e.preventDefault();
  var queryString = 'vote=' + $(e.target).attr('id');
  $.get('votes.php', queryString, function(data) {
    $('#selector').html(data);
  });
});
```



### 運用AJAX傳送表單 
#### `$.post()`
`p.394--無法運行`
```javascript=
$('#register').on('submit', function(e) {           // When form is submitted
  e.preventDefault();                               // Prevent it being sent
  var details = $('#register').serialize();         // Serialize form data
  $.post('register.php', details, function(data) {  // Use $.post() to send it
    $('#register').html(data);                    // Where to display result
  });
});

```
### 載入JSON與處理AJAX錯誤
```javascript=
$('#exchangerates').append('<div id="rates"></div><div id="reload"></div>');

function loadRates() {
  $.getJSON('data/rates.json')
  .done( function(data){                                 // SERVER RETURNS DATA
    var d = new Date();                                  // Create date object
    var hrs = d.getHours();                              // Get hours
    var mins = d.getMinutes();                           // Get mins
    var msg = '<h2>Exchange Rates</h2>';                 // Start message
    $.each(data, function(key, val) {                    // Add each rate
      msg += '<div class="' + key + '">' + key + ': ' + val + '</div>';
    });
    msg += '<br>Last update: ' + hrs + ':' + mins + '<br>'; // Show update time
    $('#rates').html(msg);                               // Add rates to page
  }).fail( function() {                                  // THERE IS AN ERROR
    $('#rates').text('Sorry, we cannot load rates.');   // Show error message 
  }).always( function() {                                // ALWAYS RUNS
     var reload = '<a id="refresh" href="#">';           // Add refresh link
     reload += '<img src="img/refresh.png" alt="refresh" /></a>';
     $('#reload').html(reload);                          // Add refresh link
     $('#refresh').on('click', function(e) {             // Add click handler
       e.preventDefault();                               // Stop link
       loadRates();                                      // Call loadRates()
     });
  }); 
}

loadRates();  
```

* `$.getJSON()`
    * 載入JSON資料
* `$.getScipt()`
    * 載入JSONP
---
#### 成功/失敗
* `done()`
    * 當請求被正確完成後，便會啟動此事件
* `fail()`
    * 當請求未被正確完成後
* `always()`
    * 當請求完成時(無論成功失敗)
---

### 範例 (找時間實作) `example.html` 

```javascript=
$(function() {                                    // When the DOM is ready

  var times;                                      // Declare global variable
  $.ajax({                                        //建立請求設定
    beforeSend: function(xhr) {                   // Before requesting data(發送請求之前)
      if (xhr.overrideMimeType) {                 // If supported (如果支援此特性)
        xhr.overrideMimeType("application/json"); // set MIME to prevent errors(設定MIMR避免錯誤)
      }
    }
  });

  // FUNCTION THAT COLLECTS DATA FROM THE JSON FILE
  //從json收集資料
  //loadTimetable()會從example.json載入三個活動的JSON格式資料
  //活動資料會儲存於變數time中
  function loadTimetable() {                    // Declare function
    $.getJSON('data/example.json')              // Try to collect JSON data
    .done( function(data){                      // If successful
      times = data;                             // Store it in a variable
    }).fail( function() {                       // If a problem: show message
      $('#event').html('Sorry! We could not load the timetable at the moment');
    });
  }

  loadTimetable();                              // Call the function


  // CLICK ON THE EVENT TO LOAD A TIMETABLE 
  //點擊活動以載入活動課程時間表
  $('#content').on('click', '#event a', function(e) {  // User clicks on event

    e.preventDefault();                                // Prevent loading page
    var loc = this.id.toUpperCase();                   // Get value of id attr  會擷取被點擊連結的id屬性值

    var newContent = '';                               // Build up timetable by
    for (var i = 0; i < times[loc].length; i++) {      // looping through events
      newContent += '<li><span class="time">' + times[loc][i].time + '</span>'; //EX: <li><span class="time">9:00</span>
      newContent += '<a href="descriptions.html#'; //EX: <a href="descriptions.html#
      newContent += times[loc][i].title.replace(/ /g, '-') + '">'; //打在JSON中的"title": "Intro to 3D Modeling" 空格換成'-'(就變成descriptions.html的id) EX: Intro-to-3D-Modeling">
      newContent += times[loc][i].title + '</a></li>'; //EX: Intro to 3D Modeling</a></li>
    }
    //<li><span class="time">9:00</span><a href="descriptions.html#Intro-to-3D-Modeling">Intro to 3D Modeling</a></li>
    //<li><span class="time">10:00</span><a href="descriptions.html#Circuit-Hacking">Circuit Hacking</a></li>

    $('#sessions').html('<ul>' + newContent + '</ul>'); // Display times on page  顯示時間表

    $('#event a.current').removeClass('current');       // Update selected item  
    $(this).addClass('current');

    $('#details').text('');                             // Clear third column //清除右方行的內容
  });

  // CLICK ON A SESSION TO LOAD THE DESCRIPTION
  //點擊區域顯示描述
  $('#content').on('click', '#sessions li a', function(e) { // Click on session
    e.preventDefault();                                     // Prevent loading
    var fragment = this.href;                               // Title is in href 課程活動名稱會在href中 fragment = descriptions.html#Intro-to-3D-Modeling

    fragment = fragment.replace('#', ' #');                 // Add space after#  在#後加入空白字元，以組成正確的語法，讓jQuery.load()方法可載入部分(非全部)  fragment = descriptions.html #Intro-to-3D-Modeling
    $('#details').load(fragment);                           // To load info  載入課程描述資訊 

    $('#sessions a.current').removeClass('current');        // Update selected
    $(this).addClass('current');
  });
  // <a href="descriptions.html#Intro-to-3D-Modeling">Intro to 3D Modeling</a>
  // <div id="Intro-to-3D-Modeling">
  //     <h3>Intro to 3D Modeling</h3>
  //     <p>Come learn how to create 3D models of parts you can then make on our bus! You'll get to know the same 3D modeling software that is used worldwide in professional settings like engineering, product design, and more. Develop and test ideas in a fun and informative session hosted by Bella Stone, professional roboticist.</p>
  //  </div>


  // CLICK ON PRIMARY NAVIGATION
  //nav換顏色 
  $('nav a').on('click', function(e) {                       // Click on nav
    e.preventDefault();                                      // Prevent loading
    var url = this.href;                                     // Get URL to load

    $('nav a.current').removeClass('current');               // Update nav
    $(this).addClass('current');

    $('#container').remove();                                // Remove old part
    $('#content').load(url + ' #container').hide().fadeIn('slow'); // Add new
  });

});
```
* MIME
    * 給網際網路上傳輸的內容賦予的分類類型

* `toUpperCase()`
    * 方法将字符串小写字符转换为大写。
* `times[loc][i].time`
    * ```
        "CA": [
        {
            "time": "9:00",
            "title": "Intro to 3D Modeling"
        },
        ```
    * `loc = this.id.toUpperCase();`擷取被點擊連結的id屬性值
* `replace(/ /g, '-')`
    * `/g` 代表全局
### 自己上YT練習 (D:\alice\FrontEnd\Ajax\pratice)

```htmlmixed=
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    
</head>
<body>
    <header>
        <h1>animal</h1>
        <button id="btn">Fetch Info for 3 new</button>
        <div id="animal-info"></div>
    </header>
    <script src="ajax.js"></script>  
    <!-- Uncaught TypeError: Cannot read property ‘addEventListener’ of null
    调用js文件时，DOM还没有渲染完。把页面内引用的js放到最后就可以了。或者js里面用window.onload() -->
</body>
</html>
```

```javascript=
var page = 1;
var animalCon = document.getElementById('animal-info');
var btn = document.getElementById('btn');

btn.addEventListener("click",function(){
    var ourRequest = new XMLHttpRequest();

    ourRequest.open('GET','https://learnwebcode.github.io/json-example/animals-' +  page +  '.json');
    ourRequest.onload = function(){
        var ourData = JSON.parse(ourRequest.responseText);
        renderHTML(ourData);
    };

    ourRequest.send();
    page++;
    if(page > 3){
        btn.classList.add('hide-me');
    }
});

//按第一次顯示animal-1 第二次顯示animal-2
function renderHTML(data){
    var htmlString = "";
    for(i= 0; i < data.length; i++){
        htmlString += "<p>"  + data[i].name + 'is a ' + data[i].species +  "that likes to eat ";
        for(ii = 0; ii<data[i].foods.likes.length; ii++){
            if(ii == 0){
                htmlString += data[i].foods.likes[ii];
            }else{
                htmlString += " and " + data[i].foods.likes[ii];
            }
        }

        htmlString += 'and dislikes ';

        for(ii = 0; ii<data[i].foods.dislikes.length; ii++){
            if(ii == 0){
                htmlString += data[i].foods.dislikes[ii];
            }else{
                htmlString += " and " + data[i].foods.dislikes[ii];
            }
        }
    }
    animalCon.insertAdjacentHTML('beforeend',htmlString);
}

```
* `insertAdjacentHTML`
    * 在指定的地方插入html标签语句
    * `element.insertAdjacentHTML(position, text);`
        * position:
            * `beforebegin`: 在 element 之前。
            * `afterbegin`: 在 element 裡面，第一個子元素之前。
            * `beforeend`: 在 element 裡面，最後一個子元素之後。
            * `afterend`: 在 element 之後。