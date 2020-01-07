```
// electron的shell模块，可以用在主进程和渲染进程中。
// shell模块可调用操作系统的原生功能：删除文件到回收站，创建和读取快捷方式，默认方式打开文件，打开文件所在目录,让系统发出一声蜂鸣等。
const { shell, BrowserWindow } = require('electron');
const hasOneOrMoreWindows = BrowserWindow.getAllWindows().length;   // BrowserWindow.getAllWindows()查看是否有任何打开的窗口
const focusedWindow = BrowserWindow.getFocusedWindow();             // 获取当前的焦点窗口，没有则返回null
// 调用操作系统的原生文件浏览器，将指定的文件路径显示在一个新的文件浏览器窗口中
shell.showItemInFolder(filePath);
// 请求操作系统使用用户指定的默认程序打开当前文件
shell.openItem(filePath)    
```
+ 如果要实现的功能对于应用而言至关重要，那么最好将它放在UI界面上。反之，UI界面上位置有限，对于使用较少，锦上添花的功能，最好放到上下文菜单中。
+ 点击按钮弹出错误提示，算不上是很好的用户体验。在上下文菜单和应用菜单项不可用时直接禁用它们，这样用户直接就能知道现在不能执行这些操作，这样更好。
+ 相较于禁用某些菜单项，大部分时候，在需要显示上下文菜单前，基于当前状态生成新的菜单，或在应用状态发生变化时生成新的应用菜单，往往是更简单的方案。

