---
title: "Doom Emacs 给中文指定字体"
date: 2022-07-13T09:58:09+08:00
draft: false
---

因为我使用的是 1080p 的屏幕，所以这里我设置的是中文字体是 `WenQuanYi Zen Hei Sharp`;

使用点阵字体可以获得更佳的体验效果，英文字体我使用的是`Terminus`

<!--more-->

```elisp
;; Set font for chinese characters
;; Font should be twice the width of asci chars so that org tables align
;; This will break if run in terminal mode, so use conditional to only run for GUI.
(if (display-graphic-p)
    (dolist (charset '(kana han cjk-misc bopomofo))
      (set-fontset-font (frame-parameter nil 'font)
                        charset (font-spec :family "WenQuanYi Zen Hei Sharp" :size 14))))
```

### 预览效果

![预览图](./2022-07-13_10-02.png)
