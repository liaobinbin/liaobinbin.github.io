---
title: "Jest e2e测试 Puppeteer 自动点击 Alert 确定"
date: 2022-07-18T10:02:54+08:00
draft: false
---

> 在编写 e2e 测试的时候，有的业务会有浏览器自带的 alert 弹出，利用 puppeteer 的按键点击事件无效。

## 思路一

给`page`添加事件监听，当有对话框弹出的时候，自动进行对话框的确认。

代码如下

```javascript
page.on("dialog", async (dialog) => {
  await dialog.accept();
});
```

## 思路二

将自带的`alert`方法进行重写，在启动`puppeteer`的 page 后，将该 api 进行重写，让其不会调用原始的`alert`,也就可以保证不会弹出对话框来进行阻塞进程。

同时还可以利用`console.log`来进行替换该方法，在控制台中打印警告信息。
