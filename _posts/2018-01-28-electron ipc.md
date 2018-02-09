> 本来想用remote和webContents代替的, 但是, 那两个里面坑非常多. 没办法, 还是换回ipc. 顺便说一下, 那两个只是语法糖, 最终的实现还是ipc



其实还是很简单的分两个部分

- main
- renderer

###### main

```js
const {app, BrowserWindow,ipcMain} = require('electron')

//接收
ipcMain.on('close-settings-window', function () {
    if (settingsWindow) {
        settingsWindow.close();
      //异步发回响应信息, 此处是一个不优雅的地方. 发信息有这个和下面的直接发送
       event.sender.send('asynchronous-reply', 'pong')
    }
});

//发送(直接发送)  
contents.send(channel[, arg1][, arg2][, ...])
xxxWindow.webContents.send('global-shortcut', 0);
win.webContents.send('ping', 'whoooooooh!')
```

###### renderer

```js
const {ipcRenderer} = require('electron')
//接收
ipcRenderer.on('global-shortcut', function (a, b) {
	soundButtons[b].click()
});
require('electron').ipcRenderer.on('ping', (event, message) => {
      console.log(message)  // Prints 'whoooooooh!'
})
//发送
ipcRenderer.send('open-settings-window');
```

### 附录 remote示意

```js
const {Tray,Menu} = require('electron').remote
var path = require('path');
var trayIcon = null;
if (process.platform === 'darwin') {
    trayIcon = new Tray(path.join(__dirname, 'img/tray-iconTemplate.png'));
}
else {
    trayIcon = new Tray(path.join(__dirname, 'img/tray-icon-alt.png'));
}
var trayMenuTemplate = [
    {
        label: 'Sound machine',
        enabled: false
    },
    {
        label: 'Settings',
        click: function () {
            ipcRenderer.send('open-settings-window');
        }
    },
    {
        label: 'Quit',
        click: function () {
            ipcRenderer.send('close-main-window');
        }
    }
];
var trayMenu = Menu.buildFromTemplate(trayMenuTemplate);
trayIcon.setContextMenu(trayMenu)
```



### 附录 webcontents坑

比如这段

```js
//我滴神啊, 这个地方不是单引号, 是反引号, 我服了.....
const code=`const iframe=document.querySelector('iframe'); 
let content=iframe.textContent.replace(/\\u200C/mg, '\\n');
fs.writeFile('./xxx2.md', content, (err) => {
  if(err){
    log('An error ocurred creating the file '+ err.message);
  }
})`
win.webContents.executeJavaScript(code)
```

- webcontent 为毛要用反引号.
  - 明白了, 这个地方就是eval的问题了. 奶奶的果然学js不能漏过这么精华的内容...
  - 明白了为啥用反引号了, 因为多行, 一般的javascript字符串不支持多行. 奶奶的.
  - 因此, 我们需要做三件事之一
    - 行尾回车之前用\转义
    - 链接过得字符串, 比如用+加好就可以
    - 用反引号
  - 因此放弃这种写法, 还是改回ipc写法吧. 这种写法太过于绕了, 坑太多.