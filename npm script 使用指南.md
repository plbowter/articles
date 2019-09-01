# npm scripts 使用指南

作者：果果
日期：2019年9月1日 学习笔记
[阮一峰老师博客原文链接](http://www.ruanyifeng.com/blog/2016/10/npm_scripts.html)

Node 开发离不开 npm，而脚本功能是 npm 最强大、最常用的功能之一。

本文介绍如何使用 npm 脚本（npm scripts）。

![](http://www.ruanyifeng.com/blogimg/asset/2016/bg2016101101.jpg)

## 一、什么是 npm 脚本？

npm 允许在 `package.json` 文件里面，使用 `scripts` 字段定义脚本命令。
```
{
// ...
"scripts": {
	"build":"node build.js"
 }
}
```
上面的代码是 `package.json` 文件的一个片段，里面的 `scripts` 字段是一个对象。它的每一个属性，对应一段脚本。比如，`build` 命令对应的脚本是 `node build.js`。

命令行下使用 `npm run` 命令，就可以执行这段脚本。

```
$ npm run build
# 等同于执行
$ node build.js

```
这些定义在 `package.json` 里面的脚本，就称为 npm 脚本。它的优点很多。

- 项目的相关脚本，可以集中在一个地方。
- 不同项目的脚本命令，只要功能相同，就可以有同样的对外接口。用户不需要知道怎么测试你的项目，只要运行 npm run test 即可。
- 可以利用 npm 提供的很多辅助功能。

查看当前项目的所有 npm 脚本命令，可以使用不带任何参数的 `npm run` 命令。
```
$ npm run
```
## 二、原理
npm 脚本的原理非常简单。每当执行 `npm run`，就会自动新建一个 shell，在这个 shell 里面执行指定的脚本命令。因此，只要是 shell（一般是 Bash）可以运行的命令，就可以写在 npm 脚本里面。

