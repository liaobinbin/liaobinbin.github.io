---
title: "git无法pull仓库refusing to merge unrelated histories"
date: 2018-05-14T10:04:24+08:00
---
相信很多童鞋都遇到过，在Github创建仓库的时候，勾选上了创建License。

然后为了把本地的仓库上传。因为两个仓库内容不一致，首先要进行pull，把远端的内容拉取下来，然后和并提交。

但是却发现`refusing to merge unrelated histories`这个错误，无法进行pull。


所以就需要用到以下的参数：

```bash
git pull origin master --allow-unrelated-histories
```

剩下的操作得以继续进行。
