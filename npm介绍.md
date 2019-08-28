nodejs的出现，可以算是前端里程碑式的一个事件，它让前端攻城狮们摆脱了浏览器的束缚，踏上了一个更加宽广的舞台。前端的可能性，从此更加具有想象空间。

随着一系列基于nodes的应用/工具的出现，工作中与nodejs打交道的机会越来越多。无论在node应用的开发，还是使用中，包管理都扮演着一个很重要的作用。NPM（node package manager），作为node的包管理工具，极大地便利了我们的开发工作，很有必要了解一下。

### NPM是什么？
NPM（node package manager），通常称为node包管理器。顾名思义，它的主要功能就是管理node包，包括：安装、卸载、更新、查看、搜索、发布等。

npm的背后，是基于couchdb的一个数据库，详细记录了每个包的信息，包括作者、版本、依赖、授权信息等。它的一个很重要的作用就是：将开发者从繁琐的包管理工作（版本、依赖等）中解放出来，更加专注于功能的开发。

npm官网： https://npmjs.org/

npm官方文档： https://npmjs.org/doc/README.html

### NPM能做什么？
npm的安装、卸载、升级、配置
npm的使用：package的安装、卸载、升级、查看、搜索、发布
npm包的安装模式：本地 vs 全局
package.json：包描述信息
package版本：常见版本声明形式

### npm包安装模式
在具体介绍npm包的管理之前，我们首先得来了解一下npm包的两种安装模式。

本地安装 vs 全局安装（重要）

node包的安装分两种：本地安装、全局安装。两者的区别如下，后面会通过简单例子说明

本地安装：package会被下载到当前所在目录，也只能在当前目录下使用。
全局安装：package会被下载到到特定的系统目录下，安装的package能够在所有目录下使用。

###### 本地安装

1
$ npm install pkg //本地安装命令
运行如下命令，就会在当前目录下安装 grunt-cli （grunt命令行工具）

1
$ npm install grunt-cli
安装结束后，当前目录下回多出一个 node_modules 目录，grunt-cli就安装在里面。同时注意控制台输出的信息：

grunt-cli@0.1.9 node_modules/grunt-cli
├── resolve@0.3.1
├── nopt@1.0.10 (abbrev@1.0.4)
└── findup-sync@0.1.2 (lodash@1.0.1, glob@3.1.21)

简单说明一下：
grunt-cli@0.1.9：当前安装的package为grunt-cli，版本为0.19
node_modules/grunt-cli：安装目录
resolve@0.3.1：依赖的包有resolve、nopt、findup-sync，它们各自的版本、依赖在后面的括号里列出来

###### 全局安装

1
$ npm install -g pkg //全局安装命令
上面已经安装了grunt-cli，然后你跑到其他目录下面运行如下命令

1
$ npm install grunt-cli
果断提示你grunt命令不存在，为什么呢？因为上面只是进行了本地安装，grunt命令只能在对应安装目录下使用。
控制台输出的信息：

-bash: grunt: command not found

如果为了使用grunt命令，每到一个目录下都得重新安装一次，那不抓狂才怪。肿么办呢？
很简单，采用全局安装就行了，很简单，加上参数 -g 就可以了

1
$ npm install -g grunt-cli
于是，在所有目录下都可以无压力使用 grunt 命令了。这个时候，你会注意到控制台输入的信息有点不同。主要的区别在于安装目录，现在变成了 /usr/local/lib/node_modules/grunt-cli ， /usr/local/lib/node_modules/ 也就是之前所说的全局安装目录啦。

grunt-cli@0.1.9 /usr/local/lib/node_modules/grunt-cli
├── resolve@0.3.1
├── nopt@1.0.10 (abbrev@1.0.4)
└── findup-sync@0.1.2 (lodash@1.0.1, glob@3.1.21)

### npm包管理
npm的包管理命令是使用频率最高的，所以也是我们需要牢牢记住并熟练使用的。其实无非也就是几个动作：安装、卸载、更新、查看、搜索、发布等。

###### 安装最新版本的grunt-cli

1
$ npm install grunt-cli

###### 安装0.1.9版本的grunt-cli

1
$ npm install grunt-cli@"0.1.9"

###### 通过package.json进行安装

如果我们的项目依赖了很多package，一个一个地安装那将是个体力活。我们可以将项目依赖的包都在package.json这个文件里声明，然后一行命令搞定

1
$ npm install

###### 其他package安装命令

运行如下命令，列出所有 npm install 可能的参数形式

1
$ npm install --help
输出如下，有兴趣的童鞋可以了解下

npm install <tarball file>
npm install <tarball url>
npm install <folder>
npm install <pkg>
npm install <pkg>@<tag>
npm install <pkg>@<version>
npm install <pkg>@<version range>

### 卸载package

比如卸载grunt-cli

1
$ npm uninstall grunt-cli
卸载0.1.9版本的grunt-cli

1
$ npm uninstall grunt-cli@"0.1.9"

### 查看package

运行如下命令，就可以查看当前目录安装了哪些package

1
$ npm ls
控制台输出如下：

1
2
3
4
5
6
7
8
9
10
11
12
13
/private/tmp/npm
└─┬ grunt-cli@0.1.9
  ├─┬ findup-sync@0.1.2
  │ ├─┬ glob@3.1.21
  │ │ ├── graceful-fs@1.2.3
  │ │ ├── inherits@1.0.0
  │ │ └─┬ minimatch@0.2.12
  │ │   ├── lru-cache@2.3.0
  │ │   └── sigmund@1.0.0
  │ └── lodash@1.0.1
  ├─┬ nopt@1.0.10
  │ └── abbrev@1.0.4
  └── resolve@0.3.1
同样，如果是要查看package的全局安装信息，加上 -g 就可以。

###### 查看特定package的信息

运行如下命令，输出grunt-cli的信息

1
$ npm ls grunt-cli
输出的信息比较有限，只有安装目录、版本，如下：

/private/tmp/npm
└── grunt-cli@0.1.9

如果要查看更详细信息，可以通过 npm info pkg，输出的信息非常详尽，包括作者、版本、依赖等。

1
$ npm info grunt-cli

### package更新

1
$ npm update grunt-cli

### package搜索

1
$ npm search grunt-cli
返回结果如下

npm http GET http://registry.npmjs.org/-/all/since?stale=update_after&startkey=1375519407838
npm http 200 http://registry.npmjs.org/-/all/since?stale=update_after&startkey=1375519407838
NAME DESCRIPTION AUTHOR DATE KEYWORDS
grunt-cli The grunt command line interface. =cowboy =tkellen 2013-07-27 02:24
grunt-cli-dev-exitprocess The grunt command line interface. =dnevnik 2013-03-11 16:19
grunt-client-compiler Grunt wrapper for client-compiler. =rubenv 2013-03-26 09:15 gruntplugin
grunt-clientside Generate clientside js code from CommonJS modules =jga 2012-11-07 01:20 gruntplugin

### npm发布

这个命令我自己也还没实际用过，不误导大家，语法如下，也可参考官方对于package发布的说明 https://npmjs.org/doc/developers.html

1
2
$ npm publish &lt;tarball&gt;
$ npm publish &lt;folder&gt;

### NPM配置

npm的配置工作主要是通过 npm config 命令，主要包含增、删、改、查几个步骤，下面就以最为常用的proxy配置为例。

### 设置proxy

内网使用npm很头痛的一个问题就是代理，假设我们的代理是 http://proxy.example.com:8080，那么命令如下：

1
$ npm config set proxy http://proxy.example.com:8080
由于 npm config set 命令比较常用，于是可以如下简写

1
$ npm set proxy http://proxy.example.com:8080

### 查看proxy

设置完，我们查看下当前代理设置

1
$ npm config get proxy
输出如下：

http://proxy.example.com:8080/

同样可如下简写：

1
$ npm get proxy

### 删除proxy

代理不需要用到了，那删了吧

1
$ npm delete proxy

### 查看所有配置

1
$ npm config list

### 修改配置文件

有时候觉得一条配置一条配置地修改有些麻烦，就直接进配置文件修改了

1
$ npm config edit
内容只是简单地把最常见的命令，以及一些需要了解的内容列了出来。如要进一步了解，可参考官网说明。此外， npm help 是我们最好的朋友，如果忘了有哪些命令，命令下有哪些参数，可通过help进行查看。

原文：http://www.cnblogs.com/chyingp/p/npm.html?utm_source=tuicool&utm_medium=referral
原文总结的很好，本人只是做了部分修改。
