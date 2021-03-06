## 使用whistle
1. 代替fiddler和charlse实现代理、抓包、mock功能
2. 安装
    `npm install -g whistle`
3. 启动
    `w2 start -p 9000`
4. `whistle`其他命令
    `w2 help`
5. 打开浏览器`localhost:9000`即可使用`whistle`
6. 设置网页代理
    1. 在浏览器设置中进行设置
    2. 在网络设置中设置

    ![proxy1](https://github.com/bearnew/picture/blob/master/mardown/2019-09-25%20whistle/browser%20proxy1.png?raw=true)

    3. chrome安装插件`SwitchyOmega`,进行设置

    ![proxy2](https://github.com/bearnew/picture/blob/master/mardown/2019-09-25%20whistle/browser%20proxy2.png?raw=true)
    ![proxy3](https://github.com/bearnew/picture/blob/master/mardown/2019-09-25%20whistle/browser%20proxy3.png?raw=true)
7. 切换代理模式

    ![proxy switch](https://github.com/bearnew/picture/blob/master/mardown/2019-09-25%20whistle/proxy%20switch.png?raw=true)

8. 刷新在`switchyOmega`里匹配的域名，即可抓包

    ![capture](https://github.com/bearnew/picture/blob/master/mardown/2019-09-25%20whistle/capture.png?raw=true)

9. 在`whistle`里面进行mock数据

    * 使用`file://`的路径是相对`w2 start`的启动路径
    * 使用`file:///`的路径是指的跟路径`/`
    ![mock](https://github.com/bearnew/picture/blob/master/mardown/2019-09-25%20whistle/mock.png?raw=true)

10. 安装`whistle`的`Https`证书
    * 点击界面头部https进行下载安装
11. 如果公司只开放了`5389`端口，用`5389`启动`whistle`才能连接手机
12. 手机连电脑 wifi 热点，在配置了代理 ip 和端口之后，任然不能访问
    * 打开 Windows 防火墙，点击 “允许程序或功能通过 Windows 防火墙”，找到 Node.js 的所有进程（Node.js: Server-side JavaScript），勾选所有进程及进程里的 checkbox 即可。
13. 