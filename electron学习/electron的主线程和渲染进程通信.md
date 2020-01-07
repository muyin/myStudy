```
// 主进程中：向渲染进程show-file频道发送一条消息
win.webContents.send('file-opened', file, content);  // 这儿win是某渲染窗口对象的引用
// 渲染进程中：监听指定频道，接收到跨进程消息
const { remote, ipcRenderer, shell } = require('electron');
ipcRenderer.on('file-opend', showFile);  // 接收到来自主进程的file-opend频道消息时，调用showFile(file, content)函数
```