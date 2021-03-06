# 定制 OSS Browser

通过修改此目录下的配置，目前可以较容易的自定义logo，app名称，版本号，更新地址等。

如果需要修改更多内容，请直接修改oss browser代码。

下面介绍如何修改配置，如何重新build，如何发布。

* 注意：build oss browser 请使用 Mac，或者 ubuntu，不要用 windows（可能会有一些兼容性的问题要解决）。

## 1. 安装环境

本工具使用 [Electron](https://electron.atom.io/) 编写，依赖 [Node.js](https://nodejs.org) >= 7.9.0.

所以先要安装 Node.js

### (1) Node.js

Node.js 从官网下载最新版本安装即可。

### (2) 安装 cnpm（npm的中国镜像，加快依赖下载速度）。

```
sudo npm install -g cnpm --registry=https://registry.npm.taobao.org
```

### (3) 获取 oss-browser 源码

先到 https://github.com/aliyun/oss-browser ，Fork 一份到你自己的仓库，然后clone：

```
git clone {git地址}
cd oss-browser
```

## 2. 开始尝试启动

```
make i   # 安装 node 模块依赖
```

启动界面：
```
make run  # 开发模式启动
```

这时，你可以看到界面了（开发模式，可以按 command+r 刷新)。


## 3. 修改 custom 配置

```
oss-browser/
  |-- custom
```

修改 custom 中的 index.js 配置 和 图标即可。

## 4. build

```
make all # 将会在 releases 目录，生成几个压缩包。这几个压缩包即安装文件。
```

* 注意： Makefile中的mac_build命令，--icon=需要指定自定义的图标

## 5. 发布

我们可以将安装包上传到oss的某个目录下，将这个目录设置为公共读。

> 升级检测原理:

custom/index.js 的 upgrade_url 配置 upgrade.json 的 https 地址, 供客户端升级检测。

upgrade.json 样例:

```json
{
  "version": "1.3.0",
  "package_url": "http://luogc.oss-cn-hangzhou.aliyuncs.com/oss-browser-publish/"
}
```

* version 表示最新版本。

* package_url：安装包下载地址前缀。可以修改根据实际情况修改。

oss-browser-publish 目录结构:
```
.../oss-browser-publish/
      |-- 1.3.0
            |-- oss-browser-linux-x64.zip    # linux 64位
            |-- oss-browser-win32-ia32.zip   # windows 32位
            |-- oss-browser-win32-x64.zip    # windows 64位
            |-- oss-browser.dmg              # mac 64位
      |-- 1.2.5
      |-- ...
```


将build好的安装包，上传到 oss-browser-publish 的版本目录下。

文件上传完成后，修改upgrade.json中的version，旧版本的客户端即可查询到有版本更新。
