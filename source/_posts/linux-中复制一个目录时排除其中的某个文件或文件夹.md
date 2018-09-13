---
title: linux 中复制一个目录时排除其中的某个文件或文件夹
date: 2018-09-13 15:51:38
tags: linux
description: 记一个将 todo-vuex 文件夹的内容（除 node_modules）复制到 get-answer 文件夹的 demo
---
linux 中复制一个目录时排除其中的某个文件或文件夹。
解决来源：
__https://zhidao.baidu.com/question/1604810301667156627.html__

```
# 当前目录： todo-vuex 
cp -r `ls | grep -v node_modules | xargs` ../../get-answer
```

记一个将 todo-vuex 文件夹的内容（除 node_modules）复制到 get-answer 文件夹的 demo

![](/images/201809/WX20180913-154021.png)

```
# 查看 get-answer 目录下得文件
{15:38}~/github ➭ cd get-answer
{15:38}~/github/get-answer:master ✓ ➭ ls
README.md

# 查看 todo-vuex 目录下得文件
{15:38}~/github/get-answer:master ✓ ➭ cd ../mine/todo-vuex/
{15:38}~/github/mine/todo-vuex:feature-todo-vuex ✓ ➭ ls
README.md         config            node_modules      package-lock.json src
build             index.html        npm-debug.log     package.json      static

# 复制 todo-vuex 目录下文件到 get-answer 目录
{15:38}~/github/mine/todo-vuex:feature-todo-vuex ✓ ➭ cp -r `ls | grep -v node_modules | xargs` ../../get-answer

# 查看结果
{15:39}~/github/mine/todo-vuex:feature-todo-vuex ✓ ➭ cd ../../get-answer
{15:39}~/github/get-answer:master ✗ ➭ ls
README.md         config            npm-debug.log     package.json      static
build             index.html        package-lock.json src
{15:39}~/github/get-answer:master ✗ ➭
```

---

---

羡慕和追逐强者，是男人生来融入血脉的天性。
-- 俗念亲 《每次跳楼，都看见那厮在铺救生气垫》
