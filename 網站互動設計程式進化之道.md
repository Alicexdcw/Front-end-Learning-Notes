---
tags: javascript, 前端
---
`參考書籍:javascript & jQuery 網站互動設計程式進化之道`
`#190129 #190130 #190201`
# Java Script自學筆記 - CH1.特性、事件、方法 

# 前言
## 運用Javascript可以增添網頁的互動性
1. 可<font color="red">**選取(access)、增加或刪除(modify)**</font>html網頁中任何
    * 元件(e.g:`<h1>`)
    * 屬性(Attributes)(`class` or `id`)
    * 文字(e.g:`<h1>`內的文字)
    2. <font color="red">**計畫**</font>運作規則 (Program rules)
    * e.g:計畫一個房貸計算程式腳本，功能為蒐集使用者於表單所填寫的資料後進行計算，並顯示每月應償還的金額
    * e.g:計畫一個動畫程式腳本，功能為偵測瀏覽器視窗的範圍大小後，將圖片移動致瀏覽器可瀏覽範圍，一般稱為可視區域(viewport)的底部。
    3. <font color="red">**回應**</font>網頁事件(React to events)
    * e.g:當按鈕被按下時，觸發腳本執行
![](https://i.imgur.com/UQdD7hw.jpg)
![](https://i.imgur.com/ztuKhbM.jpg)


# CH1-B 特性、事件、方法
* 程式設計師利用資料來製造模型
* 每個物件(模型)都有自己的 `e.g:物件類型:汽車`
    * 特性(Properties) `e.g:.currentSpeen:20mph`
    * 事件(Events) `e.g:事件->accelerate(汽車加速) 發生情境->駕駛人加速` 
    * 方法(Methods) `e.g:方法->changeSpeen() 執行工作->增加或減少crrentSpeen特性值`
    * 舉例:汽車物件
        * 當駕駛人加速時，就啟動accelerate事件
        * accelerate事件會呼叫changeSpeen()方法，他會增加currentSpeed特性值
        * currentSpeed可反映出目前行車速度
## 網頁瀏覽器也會為他所顯示的網頁建立網頁模型，也會為呈現網頁的視窗建立瀏覽器視窗模型
* windows物件
        * 瀏覽器代表每一個視窗或頁籤正在使用window物件。window物件的location特性會告知你目前頁面的URL位址
    * document物件
        * 將網頁載入至視窗的動作，是透過document物件進行
        * 利用document物件的title，可得知在該頁面的`<title>、</title>`之間的標題文字內容
        ![](https://i.imgur.com/eCplNH5.jpg)
        * 利用document物件，可以存取並改變頁面內容，並對使用者與頁面的互動進行回應
        * document物件也具有
            * 特性 
                * 特性可以描述目前網頁的特徵`e.g.頁面的標題`
            * 事件
                * 可以回應事件 `e.g.使用者點擊元件，或以手指觸碰元件`
            * 方法
                * 發法會針對目前載入至瀏覽器中的文件，執行工作任務 `e.g.自特定元件中取得資料，或是加入新的內容`
            ![](https://i.imgur.com/8oy0M1a.jpg)
        * document物件只是在主流覽器中所支援的物件的其中之一。當瀏覽器創造出一個網頁的模型，他不只是建立了一個document物件，同時也為頁面上所有元建建立了新的物件。這些物件都被定義於文建物建模型(Document Object Model)

## 瀏覽器如何看待網頁?
 ![](https://i.imgur.com/uXOIWTI.jpg)

## 如何使用物件與方法

```javascript
document.write('Hello world');
```

document ->物件
write ->成員members
. ->成員運算子(memver operator)
write() ->方法
Hello world ->參數


