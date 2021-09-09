---
tags: 問題和解決
---

Tailwindc CSS & daisyUI 遇到問題未解決 <b style="color:red;">(棄用daisyUI)</b>
===


## daysiUI 遇到問題未解決

### **結論**
:::danger
<span style="color:red;">**改用 [tailwind css component](https://tailwindcomponents.com/) 或是手刻**</span>
:::

目前就是直接不要用，有請龍哥跟學長看過，研判可能是tailwindcss 跟 daisyUI 版本的問題，目前先記錄下錯誤，專案結束再研究是不是tailwindcss 新舊版本不同的關係。

---

### **安裝過程及錯誤紀錄**

**0. 按照官網安裝的方式**
+ [daisyUI](https://daisyui.com/docs/install)   
+ [daisyUI | GitHub](https://github.com/saadeghi/daisyui)   


<b>1. 使用 `yarn add daisyui`，然後在 tailwindcss.config.js 中 plugins: 加上 `require('daisyui')`</b>


```javascript
module.exports = {

    plugins: [
      require('daisyui'),
    ],

  }
```

**2. 然後在 `foreman start` 後就出現錯誤**

:::spoiler <b style="color:red;"> &nbsp; 📌 &nbsp; 點擊看錯誤訊息</b>
```console
20:19:07 webpack.1 | ✖ ｢wdm｣: Hash: b55f13a9bc09dd598d1b
20:19:07 webpack.1 | Version: webpack 4.46.0
20:19:07 webpack.1 | Time: 14491ms
20:19:07 webpack.1 | Built at: 09/09/2021 8:19:07 PM
20:19:07 webpack.1 |                                      Asset       Size       Chunks                         Chunk Names
20:19:07 webpack.1 |     js/application-872072252c8585ad640d.js    654 KiB  application  [emitted] [immutable]  application
20:19:07 webpack.1 | js/application-872072252c8585ad640d.js.map    695 KiB  application  [emitted] [dev]        application
20:19:07 webpack.1 |                              manifest.json  364 bytes               [emitted]              
20:19:07 webpack.1 | 
20:19:07 webpack.1 | ERROR in ./app/javascript/stylesheets/application.scss (./node_modules/css-loader/dist/cjs.js??ref--6-1!./node_modules/postcss-loader/src??ref--6-2!./node_modules/sass-loader/dist/cjs.js??ref--6-3!./app/javascript/stylesheets/application.scss)
20:19:07 webpack.1 | Module build failed (from ./node_modules/postcss-loader/src/index.js):
20:19:07 webpack.1 | ParserError: Syntax Error at line: 1, column 19
20:19:07 webpack.1 |     at /Users/wanganqi/Desktop/Stork-dev/Stork/app/javascript/stylesheets/application.scss:3:1
20:19:07 webpack.1 |     at Parser.error (/Users/wanganqi/Desktop/Stork-dev/Stork/node_modules/postcss-values-parser/lib/parser.js:127:11)
20:19:07 webpack.1 |     at Parser.operator (/Users/wanganqi/Desktop/Stork-dev/Stork/node_modules/postcss-values-parser/lib/parser.js:162:20)
20:19:07 webpack.1 |     at Parser.parseTokens (/Users/wanganqi/Desktop/Stork-dev/Stork/node_modules/postcss-values-parser/lib/parser.js:245:14)
20:19:07 webpack.1 |     at Parser.loop (/Users/wanganqi/Desktop/Stork-dev/Stork/node_modules/postcss-values-parser/lib/parser.js:132:12)
20:19:07 webpack.1 |     at Parser.parse (/Users/wanganqi/Desktop/Stork-dev/Stork/node_modules/postcss-values-parser/lib/parser.js:51:17)
20:19:07 webpack.1 |     at parse (/Users/wanganqi/Desktop/Stork-dev/Stork/node_modules/postcss-custom-properties/index.cjs.js:47:30)
20:19:07 webpack.1 |     at /Users/wanganqi/Desktop/Stork-dev/Stork/node_modules/postcss-custom-properties/index.cjs.js:333:24
20:19:07 webpack.1 |     at /Users/wanganqi/Desktop/Stork-dev/Stork/node_modules/postcss-loader/node_modules/postcss/lib/container.js:194:18
20:19:07 webpack.1 |     at /Users/wanganqi/Desktop/Stork-dev/Stork/node_modules/postcss/lib/container.js:55:18
20:19:07 webpack.1 |     at Rule.each (/Users/wanganqi/Desktop/Stork-dev/Stork/node_modules/postcss/lib/container.js:41:16)
20:19:07 webpack.1 | ℹ ｢wdm｣: Failed to compile.

```
:::

<br>

**VSCode 也有出現錯誤**

```console
Could not find a declaration file for module 'daisyui'. '/Users/wanganqi/Desktop/Stork-dev/Stork/node_modules/daisyui/index.js' 
implicitly has an 'any' type.

Try `npm i --save-dev @types/daisyui` 
if it exists or add a new declaration (.d.ts) file containing `declare module 'daisyui';`ts(7016)
```
![](https://i.imgur.com/AuAcFAB.png)



<b>3. 有先確定把 `require('daisyui'),` 刪掉後會恢復正常，然後在不刪掉情況下，錯誤訊息中有出現: </b>

```console
ERROR in ./app/javascript/stylesheets/application.scss 
(./node_modules/css-loader/dist/cjs.js??ref--6-1!./node_modules/postcss-loader/src??ref--6-2!
./node_modules/sass-loader/dist/cjs.js??ref--6-3!./app/javascript/stylesheets/application.scss)
```

<b>4. 檢查 `package.json`及 `node_modules` 資料夾下有出現 `daysiui`。</b>


<b>5. 檢查 `./app/javascript/stylesheets/application.scss` 中是否有錯誤。 刪掉註解，有 run 一次 `foreman start`，依然錯誤，每一行比如 `@import "tabmenu";` 拿掉測試，確定執行到 `@import "tailwindcss/utilities";` 就出現錯誤訊息。</b>

```scss
@import "tailwindcss/base";
@import "tailwindcss/components";
@import "tailwindcss/utilities";
/* import all css files */
@import "./_temp_layout";
@import "home";
@import "channels";
@import "contentmenu";
@import "member";
@import "pages";
@import "slidebar";
@import "spaces";
@import "tabmenu";

```


