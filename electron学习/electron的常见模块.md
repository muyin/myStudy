# electron的常见模块
## tray模块可以创建系统托盘或菜单栏图标
+ 代码：定义一个系统托盘图标，单击图标时，向用户弹出一个菜单
``` 定义一个系统托盘图标，单击图标时，向用户弹出一个菜单
    const path = require('path');
    const { Menu, Tray, app } = require('electron');
    const menu = Menu.buildFromTemplate([
        {
            label: 'Quit',
            click() { app.quit(); }
        }
    ]);
    let tray = new Tray( path.join(__dirname, '/Ico.png') );    // 传入图片路径参数，创建一个tray对象
    tray.on('click', tray.popUpContextMenu);    // 注册弹出菜单的click事件监听器
    tray.setToolTip('Clipmaster');  // 定义提示文本，当用户将鼠标移动到托盘图标上是，将它显示给用户
    tray.setContextMenu(menu);      // 将创建的菜单设置为单击托盘上的图标时弹出的上下文菜单
```
## clipboard模块提供了多种读写剪贴板内容的方法
+ clipboard.readText(): 从系统剪贴板读取文本内容
+ clipboard.writeText(content): 将content写入系统剪贴板 
## globalShortcut模块让electron应用可以注册全局键盘快捷键
+ globalShortcut.register()：注册全局快捷键
+ globalShortcut.unregister()：注销之前注册的全局快捷键
+ globalShortcut.unregisterAll()：注销所有全局快捷键
+ 代码：注册一个全局快捷键
``` 注册一个全局快捷键
    // globalShortcut.register()需要两个参数：定义的快捷键组合，按下快捷键后需要调用的函数
    // 全局注册快捷键CommandOrControl+Option+C,任何时候按下快捷键，都将调用注册的回调函数
    const gShortcut1 = globalShortcut.register('CommandOrControl+Option+C', function(){
        tray.popUpContextMenu();
    });
    // 如果注册失败，会返回一个“虚值”
    if (!gShortcut1) {
        console.error('全局快捷键注册失败！');
    }
```