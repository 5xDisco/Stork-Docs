---
tags: 技術資料統整
---

# Stimulus JS
[David Heinemeier Hansson - Stimulus 源起 · Stimulusjs 手冊-繁體中文](https://andyyou.gitbooks.io/stimulusjs/content/00_the_origin_of_stimulus.html)



## 安裝及使用 stimulus
>參考：[How to Add Stimulus.js to a Rails 6 Application](https://betterprogramming.pub/how-to-add-stimulus-js-to-a-rails-6-application-4201837785f9)

### 終極簡單 stimulus 安裝！會幫你下面的事情 by 龍哥
>使用下列這段指令取代掉 Steps 1 ~ 4    
```console
$ rails webpack:install:stimulus
```

#### Steps
1. 移至專案目錄安裝他
```
yarn add stimulus
```
2. 在 app/javascript/ 創建一個名為 controllers 資料夾
```
mkdir app/javascript/controllers
```
3. 在裡面新增 index.js 檔案，檔案內容如下
```
touch app/javascript/controllers/index.js

//內容
import { Application } from "stimulus"
import { definitionsFromContext } from "stimulus/webpack-helpers"

const application = Application.start()
const context = require.context(".", true, /\.js$/)
application.load(definitionsFromContext(context))

```
4. 去 app/javascript/packs/application.js 檔案增加一行文字
```
import 'controllers'
```
---

### 使用它
- 使用 stimulus 檔案都要放在 app/javascript/controllers 底下，然後需要底下格式
- 命名 名字_controller.js
```
touch app/javascript/controllers/hello_controller.js
```
- js檔案內容起手式
```
    //導入套件
    import { Controller } from "stimulus";
    
    //內容要寫在這裡面
    export default class extends Controller {
    
    // queryselector 變數
    static targets = [ "page" ]

    //內建含式可以看到，檢查有沒有連接上此腳本
    connect() {
        console.log(`new.js.erb do stimulus js`);
    }
    
    //自己的內容...
    //要這樣使用 targets 的變數，例如："page"，要這樣用 "this.pageTarget"
    close(){
    
        this.pageTarget.remove();
        console.log(`new.js.erb destory self`)
    }
}

```
- html.erb檔內連接使用語法
```
<div class="wallpaper" data-controller="channelbox" data-channelbox-target="page">
    <div class="channel-box">
        <button data-action="click->channelbox#close" class="close" >X</button>
        ...xxx
    </div>
</div>
```
- 指定哪個 controller 
```
<!-- data-controller="channelbox" 指 channelbox_controller 檔 -->
```
- 增加 click 或其他 action
```
<!-- data-action="click->channelbox#close" 指上圖 close(){} 方法 -->
```
- 使用變數
```
<!-- data-channelbox-target="page" 指上圖 static targets = [ "page" ] -->
```

### 說明

> 參考：https://stimulus.hotwired.dev/reference/actions

#### data-channelbox-target="page"
- 原生 javascript 比較像這樣：
```
const page = document.querySelector('div');
```

#### data-action="click->channelbox#close"

- 原生 javascript 比較像這樣： 
```
const btn = document.querySelector('button');

btn.addEventListener('click',(e)=>{
    close();
});

function close(){
    console.log('123')
}
```

