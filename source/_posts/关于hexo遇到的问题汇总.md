---
title: 关于hexo遇到的问题汇总
date: 2020-02-15 19:02:17
categories: bugfix
Tags: [windows, macOS, npm]
---

一些迁移、安装hexo时遇到的问题及解决方法。

<!-- more -->

#### Q1: gyp: No Xcode or CLT version detected!

近日在将hexo从win迁移到mac时，卡在了npm安装上：

运行如下命令时：

```bash
npm install
```

出现如下错误：

```bash
> node install

node-pre-gyp WARN Tried to download(404): https://fsevents-binaries.s3-us-west-2.amazonaws.com/v1.2.4/fse-v1.2.4-node-v67-darwin-x64.tar.gz
node-pre-gyp WARN Pre-built binaries not found for fsevents@1.2.4 and node@11.14.0 (node-v67 ABI, unknown) (falling back to source compile with node-gyp)
No receipt for 'com.apple.pkg.CLTools_Executables' found at '/'.

No receipt for 'com.apple.pkg.DeveloperToolsCLILeo' found at '/'.

No receipt for 'com.apple.pkg.DeveloperToolsCLI' found at '/'.

gyp: No Xcode or CLT version detected!
gyp ERR! configure error
gyp ERR! stack Error: `gyp` failed with exit code: 1
gyp ERR! stack     at ChildProcess.onCpExit (/usr/local/lib/node_modules/npm/node_modules/node-gyp/lib/configure.js:351:16)
gyp ERR! stack     at ChildProcess.emit (events.js:193:13)
gyp ERR! stack     at Process.ChildProcess._handle.onexit (internal/child_process.js:255:12)
gyp ERR! System Darwin 19.3.0
gyp ERR! command "/usr/local/Cellar/node/11.14.0/bin/node" "/usr/local/lib/node_modules/npm/node_modules/node-gyp/bin/node-gyp.js" "configure" "--fallback-to-build" "--module=/Users/louis/Documents/GitHub/jiangluyu.github.io/node_modules/fsevents/lib/binding/Release/node-v67-darwin-x64/fse.node" "--module_name=fse" "--module_path=/Users/louis/Documents/GitHub/jiangluyu.github.io/node_modules/fsevents/lib/binding/Release/node-v67-darwin-x64" "--napi_version=4" "--node_abi_napi=napi"
gyp ERR! cwd /Users/louis/Documents/GitHub/jiangluyu.github.io/node_modules/fsevents
gyp ERR! node -v v11.14.0
gyp ERR! node-gyp -v v5.0.7
gyp ERR! not ok
node-pre-gyp ERR! build error
node-pre-gyp ERR! stack Error: Failed to execute '/usr/local/Cellar/node/11.14.0/bin/node /usr/local/lib/node_modules/npm/node_modules/node-gyp/bin/node-gyp.js configure --fallback-to-build --module=/Users/louis/Documents/GitHub/jiangluyu.github.io/node_modules/fsevents/lib/binding/Release/node-v67-darwin-x64/fse.node --module_name=fse --module_path=/Users/louis/Documents/GitHub/jiangluyu.github.io/node_modules/fsevents/lib/binding/Release/node-v67-darwin-x64 --napi_version=4 --node_abi_napi=napi' (1)
node-pre-gyp ERR! stack     at ChildProcess.<anonymous> (/Users/louis/Documents/GitHub/jiangluyu.github.io/node_modules/fsevents/node_modules/node-pre-gyp/lib/util/compile.js:83:29)
node-pre-gyp ERR! stack     at ChildProcess.emit (events.js:193:13)
node-pre-gyp ERR! stack     at maybeClose (internal/child_process.js:999:16)
node-pre-gyp ERR! stack     at Process.ChildProcess._handle.onexit (internal/child_process.js:266:5)
node-pre-gyp ERR! System Darwin 19.3.0
node-pre-gyp ERR! command "/usr/local/Cellar/node/11.14.0/bin/node" "/Users/louis/Documents/GitHub/jiangluyu.github.io/node_modules/fsevents/node_modules/node-pre-gyp/bin/node-pre-gyp" "install" "--fallback-to-build"
node-pre-gyp ERR! cwd /Users/louis/Documents/GitHub/jiangluyu.github.io/node_modules/fsevents
node-pre-gyp ERR! node -v v11.14.0
node-pre-gyp ERR! node-pre-gyp -v v0.10.0
node-pre-gyp ERR! not ok
Failed to execute '/usr/local/Cellar/node/11.14.0/bin/node /usr/local/lib/node_modules/npm/node_modules/node-gyp/bin/node-gyp.js configure --fallback-to-build --module=/Users/louis/Documents/GitHub/jiangluyu.github.io/node_modules/fsevents/lib/binding/Release/node-v67-darwin-x64/fse.node --module_name=fse --module_path=/Users/louis/Documents/GitHub/jiangluyu.github.io/node_modules/fsevents/lib/binding/Release/node-v67-darwin-x64 --napi_version=4 --node_abi_napi=napi' (1)
...以下省略
```

实际上，在尝试过使用如下命令

```bash
xcode-select --install
```

进行修复时，并不成功。



**解决方案如下：**

```bash
sudo rm -rf $(xcode-select -print-path)
xcode-select --install
```



#### Q2: hexo迁移

去年在原先的笔记本上建立的项目，彼时对github了解甚少，没想过在不同电脑上更新的需求，甚至还因此断更了一年（笑），迁移步骤如下：

ssh配置好后，克隆github上yourname.github.io项目到本地：

```bash
git clone https://github.com/yourname/yourname.github.io.git
```

删除除了.git以外的所有文件；

将blog源文件中的所有文件复制到xxx.github.io的文件夹中，并将theme文件夹中各个主题中的.git文件夹删除；

创建hexo分支，并切换到hexo：

```bash
git checkout -b hexo
```

将复制的文件提交到暂存区，并提交，推送至github：

```bash
git add .
git commit -m 'new branch source files'
git push --set-upstream origin hexo
```

更新blog时，只需要照常即可：

```bash
hexo clean
hexo d -g
```

切换至新电脑时，只需要执行如下命令：

```bash
git clone -b hexo https://github.com/yourname/yourname.github.io
```

在ssh配置正确，npm安装依赖后，即可更新。



#### Q3: 无法备份/themes

在删掉themes中的所有主题的.git之后，发现使用如下命令：

```bash
git add .
git comment -m 'write your comment here'
git push
```

依然无法成功将自己修改过的主题提交到github上。



##### 解决方案如下：

在确保删除了各个主题的.git文件后，执行如下命令：

```bash
git rm --cached -r themes
git add themes
git commit -m 'write your comment here'
git push
```

