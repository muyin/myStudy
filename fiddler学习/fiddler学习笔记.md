### Fiddler简介
+ Fiddler是位于客户端和服务器端的HTTP代理
+ 目前是最常用的http抓包工具之一
+ 功能非常强大，是web调试的利器
    - 监控浏览器所有的HTTP/HTTPS流量
    - 查看、分析请求内容细节
    - 伪造客户端请求和服务器响应
    - 测试网站的性能
    - 解密HTTPS的web会话
    - 全局、局部断点功能
    - 第三方插件
+ 场景使用场景
    - 接口调试、接口测试、线上环境调试、web性能分析
    - 判断前后端bug、开发环境hosts配置、mock、弱网断网测试

### B/S架构
+ 编写程序部署到web服务器
+ web服务器运行在服务器上，绑定ip地址并监听某端口，接收和处理http请求
+ 客户端通过http协议获取服务器上的网页、文档等资源

### Fiddler原理
+ fiddler一启动就会设置自己作为系统代理(服务器)，这样所有的http与https请求响应会先通过fiddler

### HTTP
+ Hyper Text Transfer Protocol(超文本传输协议)
+ 用于从万维网服务器传输超文本到本地浏览器的传送协议
+ HTTP协议是基于TCP的应用层协议，它不关心数据传输的细节，主要是用来**规定客户端和服务器的数据传输格式**，最初是用来向客户端传输HTML页面的内容，默认端口是80
+ http是基于请求与响应模式的、无状态的、应用层的协议

### HTTP请求报文和响应报文
+ HTTP请求报文主要由请求行、请求头、一行空行、请求体4部分组成。请求体可能没有。如get请求可以没有请求体
    - 请求行分成三部分：请求方法，URL，HTTP协议与版本号
        * 请求方法： 
            > get: 请求资源，post: 提交资源，head: 获取响应头， put: 替换资源， delete: 删除资源,  
            > options:允许客户端查看服务器的性能,trace:回显服务器收到的请求，用印测试或诊断
        * URL: 统一资源定位符(Uniform Resource Locator),用于描述网上的资源位置。
            > 格式：schema://host[:port#]/path/.../[?query-string]  
            > schema:协议（如ftp,http,https等），host:域名或ip地址，port:端口，path：资源路径，query-string:发送的参数。
            > 如：https://www.baidu.com/s?wd=中国
    - 请求头(Request Header)：数据格式，数据编码，请求地址，接收的语言等
        * Host:主机ip地址或域名
        * User-Agent: 客户端相关信息，如操作系统、浏览器等信息
        * Accept: 指定客户端接收信息类型。如 image/jpg, text/html, application/json等
        * Accept-Charset: 客户端接受的字符编码，如 gb2312,utf-8
        * Accept-Encoding: 可接受的内容编码，如 gzip
        * Accept-Language: 接受的语言，如 Accept-Language:zh-cn
        * Authorization: 客户端提供给服务端，进行权限认证的信息
        * Cookie: 携带的cookie信息
        * Referer: 当前文档的URL，即从哪个链接过来的。这个可以防止盗链或者统计
        * Content-Type: 请求体内容类型，如Content-Type: application/x-www-form-urlencoded
        * Content-Length: 数据长度,这个可以防止篡改
        * Cache-Control: 缓存机制，如Cache-Control:no-cache
        * Pragma: 防止页面被缓存，和Cache-Control:no-cache作用一样
+ HTTP响应报文主要有响应行、响应头，一行空行，响应体4个部分组成
    - 响应行包括 HTTP协议与版本号，状态码，响应。如HTTP/1.1 200 OK
        * 状态码：用以表示网页服务器HTTP响应状态的3位数字代码
            > 1xx: 提示信息，请求被成功接收  
            > 2xx: 成功，请求被成功处理 200  
            > 3xx: 重定向相关 304  
            > 4xx: 客户端错误 404  
            > 5xx: 服务端错误 500  
        * 响应头（Response Header）
            > Server: HTTP服务器的软件信息  
            > Date: 响应报文的时间  
            > Expires: 指定缓存过期时间  
            > set-Cookie: 设置Cookie  
            > Last-Modified: 资源最后修改时间  
            > Content-Type: 响应的类型和字符集。如Content-Type:text/html;charset=utf-8  
            > Content-Length: 内容长度  
            > Connection: 如Keep-Alive,表示保持tcp连接不关闭，不会永久保持连接，服务器可设置  
            > Location: 指明重定向的位置，新的URL地址，如304的情况  

### Fiddler使用
+ fiddler界面由菜单栏，工具条（上边），会话列表（左边），辅助标签+工具（右边），命令行+状态栏（下边）
+ 常用快捷键：
    - r:重发一次请求
    - shift+r: 重发指定次数请求
    - del: 删除选中的记录
    - shift+del: 删除未选中的记录
    - ctrl+z: 撤销刚才的删除
    - ctrl+x: 删除所有的记录
+ 流模式和缓存模式。使用fiddler时一般用缓存模式，这样才能使用断点等功能。
+ TextWizard: 打开文本编码解码工具
+ 常用命令：
    - bpu/bpafter lingmengban : 给包含lingmengban字段的会话列表的添加请求前/响应后断点
    - bpu : 取消会话列表的断点
    - bpafter
    - ?lingmengban: 高亮包含lingmengban字段的会话列表
    - >10000 : 高亮选择大于10000字节的会话列表
+ 辅助标签+工具：
    - Statistics(统计)：HTTP请求的性能和其他数据分析，如DNS解析的时间，建立TCP/IP连接的时间消耗等信息
    - Inspectors（检测器）: 可以多种方式查看请求的请求报文和响应报文相关信息。用得最多,上边是请求报文，下边是响应报文。
    - AutoResponder(自动响应器)：可用于拦截某一请求，进行如下操作：重定向到本地的资源，使用Fiddler的内置响应，自定义响应
        > 使用这个功能可以模拟后端接口（mock），调试工作环境的代码等
    - Composer(设计者):自己设计请求报文，然后发送。可以作为一个简单的接口测试工具。
    - Filters(过滤器)：域的过滤(广域网/局域网/指定域名),指定进程过滤，请求报文过滤，响应报文过滤，响应状态过滤，断点过滤，响应过滤
+ 断点
    - Rules -> Automatic BreakPoints 设置请求前断点(Before Requests)和响应后断点(After Responses)
+ 模拟弱网
    - Rules -> Performance -> Simulate Modem Speeds开启
+ 抓取HTTPS的会话
    - 因为HTTPS是加密了的，所以抓取HTTPS需要安装证书来进行加解密。
    - 默认fiddler只能抓取http的会话，需要抓紧https的会话，需要设置
    - Tools -> Fiddler Options... -> HTTPS ->勾选“Capture HTTPS CONNECTs”和“Decrypt HTTPS traffic”
    - 火狐浏览器默认不使用代理，导致fiddler无法抓包，故需要手动设置使用系统代理或指定fiddler的代理(127.0.0.1的8888接口)
    - 如果还是不行，需要点击刚才的HTTPS窗口下的actions重置一个新的证书，用新的证书来加解密HTTPS的信息
+ Android设备抓包
    - 确保手机与安装有fiddler的电脑在同一个局域网内
    - 电脑上fiddler设置Tools -> Fiddler Options... -> Connections -> 勾选“Allow remote computers to connect”
    - 设置手机的代理。点击代理 -> 手动，设置主机名为fiddler所在主机的ip,端口为fiddler的监听接口。
    - 打开Android的浏览器，访问http://fiddler主机的ipv4地址:fiddler的监听端口(8888)/，点击页面底部的FiddlerRoot certificate下载证书
    - 打开设置 -> 更多设置 -> 系统安全 -> 加密与凭据 -> 从存储设备安装 选择下载好的FiddlerRoot.cer进行安装
    - 注意：测试完毕，记得关闭代理，否则手机无法上网
    - iso设备多一步。安装证书成功后，在通用 -> 关于本机 -> 证书信任设置中，信任刚安装的Fiddler证书
+ 安装Fiddler插件
    - willow插件：可以按工程管理AutoResponder(自动响应)规则
