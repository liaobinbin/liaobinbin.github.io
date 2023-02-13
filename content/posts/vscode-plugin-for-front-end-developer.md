---
title: "VScode 前端开发常用插件以及配置"
date: 2018-12-25T12:44:09+08:00
toc: true
categories: ["工具"]
tags: ["vscode", "前端开发"]
---

## Auto Rename Tag

自动重命名标签插件，html 标签使成对存在的，通常修改表签名的时候希望改一个会自动修改两个标签，以提交开发效率， 并且减少错误的发生。

<!--more-->

## Breacket Pair Colorizer

用于修改`()`,`[]`,`{}`的颜色，方便快速定位代码块

## Vetur

Vue 语法高亮，格式化，自动补全等功能

## Setting Sync

多台电脑开发的用户选用
配置文件同步功能，利用 gist 进行多设备统一同步 vscode 配置文件

## EditorConfig for VS Code

用于同步整个项目的开发编辑统一。例如缩进定义等。

## Git History

图形化查看 Git 的提交，详细文件修改等

## GitLens

增强 VsCode git 功能，并且可以在文件中的代码块显示修改记录，便于定位代码作者并联系。

## Guides

为代码块提高分割线， 可以快速定位代码块

## Path Authcomplete

路径补全增强

## Prettier

代码格式化工具，需要配置实现自动保存格式化代码。 并且可以配置`eslint`等工具进行规则配置。

### 我的常用配置文件

```json
{
  /* 字体设置 */
  "editor.fontFamily": "'Operator Mono','Fira Code', Menlo, Monaco, 'Courier New', monospace",
  /* 取消类型检查js文件中 flow相关 */
  "javascript.validate.enable": false,
  /* 字体大小设置 */
  "editor.fontSize": 12,
  /* 字体行号设置 */
  "editor.lineHeight": 25,
  /* 连体字支持，主要用于Fira Code中的大于等于小于等特殊符号 */
  "editor.fontLigatures": true,
  /* 超出宽度自动换行 */
  "editor.wordWrap": "on",
  /* 当文件移动更新import的路径 */
  "javascript.updateImportsOnFileMove.enabled": "always",
  /* 确认删除提示 */
  "explorer.confirmDelete": false,
  "gitlens.advanced.messages": {
    "suppressLineUncommittedWarning": true
  },
  "breadcrumbs.enabled": true,
  /* 主题和文件图标主题 */
  "workbench.colorTheme": "Dracula",
  "workbench.iconTheme": "vscode-icons",
  "files.associations": {
    "*.cjson": "jsonc",
    "*.wxss": "css",
    "*.wxs": "javascript"
  },
  /* emmet的wxml语法支持 */
  "emmet.includeLanguages": {
    "wxml": "html"
  },
  "minapp-vscode.disableAutoConfig": true,
  /* 快速小地图关闭 节省空间 */
  "editor.minimap.enabled": false,
  /*  启用eslint整合 */
  "prettier.eslintIntegration": true,
  /* 自动格式化在保存后 */
  "eslint.autoFixOnSave": true,
  /* 添加vue支持 */
  "eslint.validate": [
    "javascript",
    "javascriptreact",
    {
      "language": "vue",
      "autoFix": true
    },
    {
      "language": "html",
      "autoFix": true
    }
  ],
  /*  强制单引号 */
  "prettier.singleQuote": true,
  /*  尽可能控制尾随逗号的打印 */
  "prettier.trailingComma": "all",
  /* 使用插件格式化 html */
  "vetur.format.defaultFormatter.html": "js-beautify-html",
  /*  格式化插件的配置 */
  "vetur.format.defaultFormatterOptions": {
    "js-beautify-html": {
      /*    属性强制折行对齐 */
      "wrap_attributes": "force-aligned"
    }
  },
  "editor.renderIndentGuides": false,
  "editor.renderWhitespace": "all",
  /* 格式化工具的tab长度定义 */
  "prettier.tabWidth": 4,
  /* vue的tab定义长度 */
  "vetur.format.options.tabSize": 4,
  /* 编辑器配置 在保存的时候自动格式化代码 */
  "editor.formatOnSave": true,
  /* 编辑器的缩进量，支持所有语言 */
  "editor.tabSize": 4,
  "python.pythonPath": "/usr/local/opt/python/bin/python3.7",
  /* 初始化路径 */
  "workbench.startupEditor": "welcomePage"
}
```
