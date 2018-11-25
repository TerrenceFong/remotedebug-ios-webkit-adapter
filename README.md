# AK IOS Webview

AK IOS Webview 是基于 [RemoteDebug iOS WebKit Adapter](https://github.com/RemoteDebug/remotedebug-ios-webkit-adapter) 进行了二次封装。之前由于 RemoteDebug iOS WebKit Adapter 的 npm 和 github 版本不同步，按照文档使用的时会出现 ios 版本不同导致呈现的效果不同。clone 了原库进行 ios 版本兼容（最新的ios 12不支持，目测覆盖的测试是 ios 8 ~ 11）


PS：本文只教会怎么使用，如果想看更详细的内容，直接去[原仓库](https://github.com/RemoteDebug/remotedebug-ios-webkit-adapter)查看

## 开始

### 1) 安装依赖

在使用本库前，需要自行安装最新版的 [iTunes](http://www.apple.com/itunes/download/) 或 [iTools](https://www.itools.cn/)（效果一样，我们需要的只是它们提供的驱动：Apple Mobile Device Service 和 Bonjour 服务）。安装成功后在 win + r 里输入 services.msc。查看对应的驱动是否已启动，均显示"已启动"即可。

![](https://raw.githubusercontent.com/TerrenceFong/remotedebug-ios-webkit-adapter/master/.readme/5.png)

### 2) 安装 ak ios webview

```
npm install ak-ios-webview -g
```

### 3) 在 Safari 中启用远程调试

打开 iOS 设备的设置 => Safari => 高级 => 开启 Web 检查器

### 4) 让您的计算机信任您的 iOS 设备

iOS 设备连接到电脑的时候，请在 iOS 设备上点击 "信任"

### 5) 运行

```
ak-ios-webview
```

### 6) 设置 chrome

打开 chrome 的控制台，选择 "Remote devices" 面板

![](https://raw.githubusercontent.com/TerrenceFong/remotedebug-ios-webkit-adapter/master/.readme/1.png)

在 "Remote devices" 面板中勾选 "Port forwarding"，并且根据运行 ak-ios-webview 时监听的 port (默认为 9000) 新增 rule.

![](https://raw.githubusercontent.com/TerrenceFong/remotedebug-ios-webkit-adapter/master/.readme/2.png)

### 7) 最终效果

safari 访问任意一个网站后，使用 chrome 访问 [localhost:9000](http://localhost:9000) ，可以在此页面查看相应的信息。（ps：建议给 chrome 安装 JSONView 插件）

![](https://raw.githubusercontent.com/TerrenceFong/remotedebug-ios-webkit-adapter/master/.readme/3.png)

提供了 2 个访问的 url，两者的区别是前者用了在线的 chrome-devtools，后者使用的是本地提供的，复制相应的 url 到地址栏访问即可。

![](https://raw.githubusercontent.com/TerrenceFong/remotedebug-ios-webkit-adapter/master/.readme/4.png)

## 配置项

```
使用方法: ak-ios-webview --port=[num] --ios=[num]

可选项:
  --port    监听端口  [默认值: 9000]
  --ios     可选值：8 / 11    如果 iOS 设备的版本是 8 或 9，使用 8；如果 iOS 设备的版本是 10 或 11，使用 11.  [默认值: 8]

```

## 缺陷

### 1) iOS 10 / 11 设备的 screencast 可能会过大

### 2) 选择的元素可能不准

## 建议

本库的使用姿势建议为定位线上环境的错误，调整兼容性的样式和打打断点，再多的可能做不来。iOS 12 暂时不支持，需要等读取设备的库支持了才可支持。