---
title: "Vite 多入口lib模式打包配置"
date: 2022-08-08T10:20:31+08:00
categories: ["前端"]
tags: ["vite", "build"]
---

## 注意

> _Note:_ 无法使用 vite.config.\*

无法在`vite.config.*`中构建多个打包的入口，这时候需要自己编写一个打包的脚本，我将其放置在`scripts/build.js`中。

<!--more-->

将 `vite` 中的 `build` 方法 import 进来。 手动执行打包;

## 代码演示

```javascript
import { build } from "vite";

// make library list, the name for folder
const components = ["text", "input"];

const librarys = components.map((name) => {
  return {
    entry: `src/components/${name}/index.ts`,
    name,
    // output filename;
    filename: `xxx-${name}.js`,
  };
});

librarys.forEach(async (lib) => {
  await build({
    configFile: false,
    sourcemap: true,
    build: {
      lib,
      assetsDir: "",
      emptyOutDir: false,
      rollupOptions: {},
    },
  });
});
```
