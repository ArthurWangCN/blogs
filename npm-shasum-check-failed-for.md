---
title: 解决npm 的 shasum check failed for错误
date: 2017-04-27 17:41:02
tags: [工具, npm]
categories: 工具
---
使用npm安装一些包失败，类似如下报错情况：

```
C:\Program Files\nodejs>npm update npm
npm ERR! Windows_NT 10.0.14393
npm ERR! argv "C:\\Program Files\\nodejs\\node.exe" "C:\\Program Files\\nodejs\\node_modules\\npm\\bin\\npm-cli.js" "update" "npm"
npm ERR! node v6.2.0
npm ERR! npm  v2.14.19
npm ERR! shasum check failed for C:\Users\kuaima\AppData\Local\Temp\npm-18136-7181dc7d\registry.npmjs.org\npm\-\npm-4.0.5.tgz
npm ERR! Expected: a9c3c00c3c5bd6b0538c71109e019afd9d5b1403
npm ERR! Actual:   229ee9303b213d8ad584a6d4f65b971874d5b0e9
npm ERR! From:     https://registry.npmjs.org/npm/-/npm-4.0.5.tgz
npm ERR!
npm ERR! If you need help, you may report this error at:
npm ERR!     <https://github.com/npm/npm/issues>
npm ERR! Please include the following file with any support request:
npm ERR!     C:\Program Files\nodejs\npm-debug.log
```

找到了一篇良心之作—— [npm install 无响应解决方案](http://www.uedbox.com/npm-install-slow-solution/)，给出了3种解决方案，均可解决 npm install 慢、npm install 无响应、npm install 无法安装的问题。


**方法一：使用cnpm，用 [淘宝 NPM 镜像](https://npm.taobao.org/)**

安装

```
$ npm install cnpm -g --registry=https://registry.npm.taobao.org
```

上面这种镜像使用方式将配置写死，下次用的时候配置还在。

附：搜索镜像: <http://cnpmjs.org>

安装模块

```
$ cnpm install [name]
```

从 `registry.npm.taobao.org` 安装所有模块. 当安装的时候发现安装的模块还没有同步过来, 淘宝 NPM 会自动在后台进行同步, 并且会让你从官方 `NPMregistry.npmjs.org` 进行安装. 下次你再安装这个模块的时候, 就会直接从 淘宝 NPM 安装了.

同步模块 

```
$ cnpm sync [moduleName]
```

注意：cnpm支持 npm 除了 publish 之外的所有命令，也就是不支持publish，当然这并不影响我们使用，publish时换回npm即可，这样也能解决npm install无响应的问题。 


**方法二：使用smart-npm，用淘宝NPM镜像**

智能的 npm，让你在中国使用 npm 时，下载速度更快，使用更方便！

由于用 npm 时，默认它会访问国外资源，所以会非常卡，有时甚至会被墙。

`smart-npm` 可以在我们使用 `npm install` 时自动从国内的镜像下载资源，而在我们使用 `npm publish` 又能发布到官方的 `registry` 上。

安装

```
npm install --global smart-npm --registry=https://registry.npm.taobao.org/
```

如果 window 用户安装最新版本不成功的话，可以试试安装 `smart-npm@1`。这样做的原因是，npm 的升级使得在 mac 上无法通过 bin 别名的方式覆盖原来的 npm， 只能通过先删除原来的 npm link 文件，再创建一个新的，但这种方式在 window 上可能会有问题。 所以，如果你是 window 用户，并且通过上面脚本无法安装成功的话，可以用下面脚本再试试。

```
npm install --global smart-npm@1 --registry=https://registry.npm.taobao.org/
```

安装成功后默认会在你的 npm 用户配置文件 `~/.npmrc` 中添加淘宝的 `registry`。

卸载

```
npm smart uninstall   # 2.x.x 版本的 smart-npm 在卸载前需要先执行此脚本
npm uninstall --global smart-npm
```

注意：如果直接执行 `npm uninstall` 会导致找不到 npm 文件
Mac 或 Linux 用户可以使用下面命令恢复之前备份的 npm

```
mv $(which npm-original) $(dirname $(which npm-original))/npm
```

使用

1. 安装后系统的 npm 会被替换了，如果你要使用原生的 npm 命令，请用 npm-original 。

2. 新的 npm 会自动根据你使用的命令切换 registry，当你使用 publish, config, adduser, star 等命令时，会强制使用官方的 registry <https://registry.npmjs.org> ；当你使用其它命令时，都会使用淘宝的镜像 <https://registry.npm.taobao.org/> 。
	+ 如果要强制使用官方的 registry， 只要在命令后面加上 `--npm` 即可。
	+ 比如: `npm install jquery --npm`，就会使用官方的 registry 去拉取 jquery。（当镜像没有及时更新时，用此会选项很有效）
	+ 如果要强制使用某个 registry 时，只要在命令后面添加 registry 参数即可。
	+ 比如：`npm install jquery --registry= https://r.cnpmjs.org` ，就会使用你指定的 registry 去拉取 jquery。
	+ 如果你想修改默认的淘宝镜像或者官方的 registry，可以在你的环境变量中添加这两个参数：NPM_OFFICIAL_REGISTRY，NPM_MIRROR_REGISTRY，以此来修改默认的官方 registry 和 淘宝镜像 registry。 
	
	
**方法三：使用nrm**
nrm允许你快速地在如下 NPM 源间切换，现已支持now include: npm, cnpm, taobao,nj(nodejitsu), rednpm。注意：nrm只是一个源管理器，也不能使用publish命令。

安装

```
$ npm install -g nrm
```

示例

```
$ nrm ls
  npm ---- https://registry.npmjs.org/
  cnpm --- http://r.cnpmjs.org/
* taobao - https://registry.npm.taobao.org/
  nj ----- https://registry.nodejitsu.com/
  rednpm - http://registry.mirror.cqupt.edu.cn/
  npmMirror  https://skimdb.npmjs.com/registry/
  edunpm - http://registry.enpmjs.org/
$ nrm use cnpm  //switch registry to cnpm
Registry has been set to: http://r.cnpmjs.org/
```

使用

```
Usage: nrm [options] [command] 
  Commands: 
    ls                           List all the registries
    use                Change registry to registry
    add   [home]  Add one custom registry
    del                Delete one custom registry
    home  [browser]    Open the homepage of registry with optional browser
    test [registry]              Show the response time for one or all registries
    help                         Print this help
  Options: 
    -h, --help     output usage information
    -V, --version  output the version number
```

```
增加源 | nrm add [home]
删除源 | nrm del
测试速度 | nrm test 
```


参考文献：[解决npm 的 shasum check failed for错误](http://www.tuicool.com/articles/raI7jiV)