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
npm 脚本的原理非常简单。每当执行 `npm run`，就会自动新建一个 shell，在这个 Shell 里面执行指定的脚本命令。因此，只要是 Shell（一般是 Bash）可以运行的命令，就可以写在 npm 脚本里面。

比较特别的是，`npm run` 新建的这个 Shell，会将当前目录的 `node_modules/.bin` 子目录加入 `PATH` 变量，执行结束后，再将 `PATH` 变量恢复原样。

这意味着，当前目录的 `node_modules/.bin` 子目录里面的所有脚本，都可以直接用脚本名调用，而不必加上路径。比如，当前项目的依赖里面有 Mocha,只要直接写 `mocha test` 就可以了。

```
"test": "mocha test"
```

而不用写成下面这样。

```
"test": "./node_modeles/.bin/mocha test"
```
由于 npm 脚本的唯一要求就是可以在 Shell 执行，因此它不一定是 Node 脚本，任何可执行文件都可以写在里面。

npm 脚本的退出码，也遵守 Shell 脚本规则。如果退出码不是 `0`，npm 就认为这个脚本执行失败。

## 通配符
由于 npm 脚本就是 Shell 脚本，因为可以使用 Shell 通配符

```
"lint": "jshint *.js"
"lint": "jshint **/*.js"
```

上面代码中，`*`表示任意文件名，`**`表示任意一层子目录。

如果要将通配符传入原始命令，防止被 Shell 转义，要将星号转义。

```
"test": "tap test/\*.js"
```

## 传参
向 npm 脚本传入参数，要使用 `--`标明。

```
"lint": "jshint **.js"
```

向上面的 `npm run lint` 命令传入参数，必须写成下面这样。

```
$ npm run lint -- --reporter checkstyle > checkstle.xml
```

也可以在 `package.json` 里面再封装一个命令。

```
"lint": "jshint **.js",
"lint:checkstyle": "npm run lint -- --reporter checkstyle > checkstyle.xml"
```

## 执行顺序

如果 npm 脚本里面需要执行多个任务，那么需要明确它们的执行顺序。

如果是并行执行（即同时的平行执行），可以使用 `&`符号。

```
$ npm run script1.js & npm run script2.js
```

如果是继发执行（即只有前一个任务成功，才执行下一个任务），可以使用 `&&` 符号。

```
$ npm run script1 && npm run script2.js
```

这两个符号是 Bash 的功能。此外，还可以使用 node 的任务管理器模块：[script-runner](https://github.com/paulpflug/script-runner)、[npm-run-all](https://github.com/mysticatea/npm-run-all)、[redrun](https://github.com/coderaiser/redrun)。

## 默认值

一般来说，npm 脚本由用户提供。但是， npm 对两个脚本提供了默认值。也就是说，这两个脚本不用定义，就可以直接使用。
```
"start": "node server.js",
"install": "node-gyp rebuild"
```

上面代码中，`npm run start` 的默认值是 `node server.js`，前提是项目根目录下有 `server.js` 这个脚本；

`npm run install` 的默认值是 `node-gyp rebuild`，前提是项目根目录下有 `binding.gyp`文件。



 











