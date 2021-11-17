---
title: Git 修改遠端 remote url
tags:
  - github
id: '40'
categories:
  - - Git
date: 2019-01-15 16:54:34
---

感謝微軟買了 Github，開放了私有 Repo 的功能，雖然自己寫的不是什麼多了不起的程式碼，但畢竟是客戶的東西還是不好意思直接公開，

就直接從 Bitbuket 跳槽回 Github 吧，本地的檔案只要修改最後要 push 的遠端地址就好，其他都不用變動～

1、先檢查目前的 remote 名稱

```
git remote -v
```

2、幹掉現有的 remote 名稱

```
git remote remove origin
```

3、新增新的 remote url

```
git remote add origin https://github.com/m615926/myrepo.git
```

4、push 前記得要先 commit

```
git add .
git commit -m ‘跳槽啦～'
git push --set-upstream origin master
```