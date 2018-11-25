# AK IOS Webview

AK IOS Webview 是基于 [RemoteDebug iOS WebKit Adapter](https://github.com/RemoteDebug/remotedebug-ios-webkit-adapter) 进行了二次封装。之前由于 RemoteDebug iOS WebKit Adapter 的 npm 和 github 版本不同步，按照文档使用的时会出现 ios 版本不同导致呈现的效果不同。clone 了原库进行 ios 版本兼容（最新的ios 12不支持，目测覆盖的测试是 ios 8 ~ 11）

![](https://raw.githubusercontent.com/TerrenceFong/remotedebug-ios-webkit-adapter/master/.readme/overview.png)

PS：本文只教会怎么使用，如果想看更详细的内容，直接去[原仓库](https://github.com/RemoteDebug/remotedebug-ios-webkit-adapter)查看

## 开始

### 1) 安装依赖

在使用本库前，需要自行安装最新版的 [iTunes](http://www.apple.com/itunes/download/) 或 [iTools](https://www.itools.cn/)（效果一样，我们需要的只是它们提供的驱动：Apple Mobile Device Service 和 Bonjour 服务）。安装成功后在 win + r 里输入 services.msc。查看对应的驱动是否已启动，均显示"已启动"即可。

#### Linux

Follow the instructions to install [ios-webkit-debug-proxy](https://github.com/google/ios-webkit-debug-proxy#installation)  and [libimobiledevice](https://github.com/libimobiledevice/libimobiledevice)

#### Windows
All dependencies should be bundled. You should be good to go. 

**iOS 10 & 11 on Windows**: Please be aware that iOS11 debugging might not work on Windows as the bundled version of [/ios-webkit- debug-proxy-win32](https://github.com/artygus/ios-webkit-debug-proxy-win32) may be out of date.

**Windows 64-bit environments**:
Install the x64 version with [scoop](http://scoop.sh/)
```
scoop bucket add extras
scoop install ios-webkit-debug-proxy
```

#### OSX/Mac
Make sure you have Homebrew installed, and run the following command to install [ios-webkit-debug-proxy](https://github.com/google/ios-webkit-debug-proxy) and [libimobiledevice](https://github.com/libimobiledevice/libimobiledevice)

```
brew update
brew unlink libimobiledevice ios-webkit-debug-proxy
brew uninstall --force libimobiledevice ios-webkit-debug-proxy
brew install --HEAD libimobiledevice
brew install --HEAD ios-webkit-debug-proxy
```

### 2) Install latest version of the adapter

```
npm install remotedebug-ios-webkit-adapter -g
```

### 3) Enable remote debugging in Safari
In order for your iOS targets to show up, you need to enable remote debugging.

Open iOS Settings => Safari preferences => enable "Web Inspector"

### 4) Make your computer trust your iOS device.

On MacOS you can use Safari to inspect an iOS Safari tab. This will ensure the device is trusted.

On Windows starting iTunes could prompt the "Trust this computer" dialog.

### 5) Run the adapter from your favorite command line

```
remotedebug_ios_webkit_adapter --port=9000
```

BTW: `ios-webkit-debug-proxy` will be run automatically for you, no need to start it separately.


### 6) Open your favorite tool

Open your favorite tool such as Chrome DevTools or Visual Studio Code and configure the tool to connect to the protocol adapter.

## Configuration

```
Usage: remotedebug_ios_webkit_adapter --port [num]

Options:
  -p, --port  the adapter listerning post  [default: 9000]
  --version   prints current version

```

## Usage
### Usage with Chrome (Canary) and Chrome DevTools

You can have your iOS targets show up in Chrome's `chrome://inspect` page by leveraging the new network discoverbility feature where you simple add the IP of computer running the adapter ala `localhost:9000`.

![](.readme/chrome_inspect.png)

### Using with Mozilla debugger.html

You can have your iOS targets show up in [Mozila debugger.html](https://github.com/devtools-html/debugger.html), by starting `remotedebug_ios_webkit_adapter --port=9222` and selecting the Chrome tab.

![](.readme/debugger_html.png)

### Using with Microsoft VS Code

Install [VS Code](https:/code.visualstudio.com), and the [VS Code Chrome Debugger](https://marketplace.visualstudio.com/items?itemName=msjsdiag.debugger-for-chrome), then create a `launch.json` configuration where `port` is set to 9000, like below:

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "iOS Web",
            "type": "chrome",
            "request": "attach",
            "port": 9000,
            "url": "http://localhost:8080/*",
            "webRoot": "${workspaceRoot}/src"
        }
    ]
}
```

## Architecture
The protocol adapter is implemented in TypeScript as Node-based CLI tool which starts an instance of [ios-webkit-debug-proxy](https://github.com/google/ios-webkit-debug-proxy), detects the connected iOS devices, and then starts up an instance of the correct protocol adapter depending on the iOS version.

![](.readme/architecture.png)

## How to contribute

```
npm install
npm start
```

## Diganostics logging

```
DEBUG=remotedebug npm start
```

### License
MIT
