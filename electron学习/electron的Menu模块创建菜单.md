### 创建应用菜单
```
    // 使用自己的定制菜单替换掉electron提供的默认菜单，并将键盘快捷键绑定到菜单项
    // electron提供了两个用于创建菜单的模块：Menu和MenuItem，为了简化菜单的创建过程，
    // Menu提供了buildFromTemplate()方法，他接收一个js对象数组作为参数，基于传入的数组创建多个MenuItem
    const { app, BrowserWindow, Menu, shell } = require('electron');
    const showFileFn = function(item, focusedWindow){ ... };
    // 创建一个模板数组，它描述了整个菜单的完整结构
    const template = [
        {
            label: 'Edit',
            submenu: [
                // accelerator属性定义键盘快捷键，CommandOrControl对应windows和linux相当于control键，对应macOS系统相当于Command键
                // role属性关联到操作系统内置的常用功能，如剪切，复制，粘贴，全选，最小化，关闭窗口等。
                {label: 'Cut', accelerator: 'CommandOrControl+X', role: 'Cut'},
                {type: 'separator'},    // 菜单分隔符
                {label: 'Paste', accelerator: 'CommandOrControl+V', role: 'paste'},
            ]
        }, { 
            label: 'Window',
            submenu: [
                // 可以不设置快捷键
                {label: 'Minimize', role: 'minimize'},
                // es6的方式绑定菜单项的单击方法，item:所属的菜单项，focusedWindow：当前聚焦的窗口对象
                {label: 'Quit', accelerator: 'CommandOrControl+Q', click(item, focusedWindow) {focusedWindow.webContent.send('quit')} },
                // 选择时，调用showFileFn()方法；检测filePath是否为空，空则禁用该按钮
                {label: 'Show File', accelerator: 'CommandOrControl+S', click: showFileFn, enabled: !!filePath },    
            ]
        }
    ];
    var applicationMenu = Menu.buildFromTemplate(template);
    Menu.setApplicationMenu(applicationMenu);   // 将菜单设置为应用菜单
```
### 创建上下文菜单
``` 渲染进程中
    const {remote} = require('electron');
    const Menu = remote.Menu;
    const mdContextMenu = Menu.buidFromTemplate([
        { label: 'Open File', click(){ mainProcess.getFileFromUser() }; },
        {label: 'Show File', accelerator: 'CommandOrControl+S', click: showFileFn, enabled: !!filePath },    
        { type: 'separator' },
        { label: 'Copy', role:'Copy' },
        { label: 'Select All', role: 'selectall' },
        { label: 'Paste', role:'paste' },
    ]);
    markdownView.addEventListener('contextmenu', (event) => {
        event.preventDefault();
        // 显示上下文菜单。popup()方法有4个参数：BrowserWindow对象，上下文菜单的x轴位置，y轴位置，positioningItem对象
        // 以前浏览器中是用在鼠标点击处显示原本隐藏的dom,来模拟上下文菜单
        mdContextMenu.popup();
    });
```