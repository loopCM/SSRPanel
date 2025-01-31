# SSRPanel
Support but not limited to: Shadowsocks,ShadowsocksR,ShadowsocksRR,V2Ray

- [Demo](https://demo.ssrpanel.com)
- [Telegram](https://t.me/ssrpanel)
- [Issues](https://github.com/ssrpanel/SSRPanel/issues)
- [WIKI](https://github.com/ssrpanel/SSRPanel/wiki)


## Requirements
- PHP 7.1.3 +  这个必须是7.1.x ，注意是7.1版本，不能高了，高了就有问题，必须是7.1.x
- Mysql 5.5 +
- Memory 1G +
- Disk 10G +
- PHP Extensions: ZIP、XML、CURL、GD2、FileInfo、OpenSSL、Mbstring、PDO、Tokenizer、Ctype、JSON、BCMath
- Laravel 5.6



## 项目描述
````
1.SSR多节点账号管理面板，兼容SS、SSRR，需配合SSR或SSRR版后端使用
2.支持V2Ray
3.内含简单的购物、卡券、邀请码、推广返利&提现、文章管理、工单（回复带邮件提醒）等模块
4.用户、节点标签化，不同用户可见不同节点
5.SS配置转SSR(R)配置，轻松一键导入导出SS账号
6.单机单节点日志分析功能
7.账号、节点24小时和本月的流量监控
8.流量异常、节点宕机邮件或ServerChan及时通知
9.账号临近到期、流量不够会自动发邮件提醒，自动禁用到期、流量异常的账号，自动清除日志等各种强大的定时任务
10.后台一键添加加密方式、混淆、协议、等级
11.屏蔽常见爬虫、屏蔽机器人
12.支持单端口多用户
13.支持节点订阅功能，可自由更换订阅地址、封禁账号订阅地址、禁止特定型号设备订阅
14.支持多国语言，自带英日韩繁语言包
15.订阅防投毒机制
16.自动释放端口机制，防止端口被大量长期占用
17.有赞云支付、TrimePay支付
18.可以阻止大陆或者海外访问
19.中转节点（开发中）
20.强大的营销管理：PushBear群发消息
21.telegram机器人（开发中）
22.防墙监测，节点被墙自动提醒、自动下线（TCP阻断）
````

## 演示&交流
````
官方站：http://www.ssrpanel.com
演示站：http://demo.ssrpanel.com
telegram订阅频道：https://t.me/ssrpanel
````
官网搭建于Azure，由代理商 [@LesHutt](https://t.me/LesHutt) 提供，需要流量机器，价格优惠需要的联系他。


[VPS推荐&购买经验](https://github.com/ssrpanel/SSRPanel/wiki/VPS%E6%8E%A8%E8%8D%90&%E8%B4%AD%E4%B9%B0%E7%BB%8F%E9%AA%8C)

## 安装
#### 环境要求
````
PHP 7.1 （必须）
MYSQL 5.5 （推荐5.6+）
内存 1G+ 
磁盘空间 10G+
PHP必须开启zip、xml、curl、gd2、fileinfo、openssl、mbstring组件
安装完成后记得编辑.env中 APP_DEBUG 改为 false
````

#### 拉取代码
````
cd /home/wwwroot/
git clone https://github.com/ssrpanel/ssrpanel.git  如果这个地址不能用，可以换成自己github仓库备份的地址
chown -R www:www ssrpanel
chmod -R a+x ssrpanel
````

#### 配置数据库
````
1. 创建一个utf8mb4的数据库，建议名称为ssrpanel,方便记忆管理
3. 导入 sql/db.sql 数据库至ssrpanel
4. cd ssrpanel/
5. cp .env.example .env （复制.env.example模板文件，然后 vim .env 修改数据库的连接信息）
````

#### 安装面板
````
cd ssrpanel/
php composer.phar install
php artisan key:generate
chown -R www:www storage/
chmod -R 755 storage/
````

#### 加入NGINX的URL重写规则，其实就是设置网站伪静态
````
location / {
    try_files $uri $uri/ /index.php$is_args$args;
}
````

#### 编辑php.ini
````
找到php.ini
vim /usr/local/php/etc/php.ini

搜索disable_function
删除proc_开头的所有函数
````

#### 出现500错误
````
理论上操作到上面那些步骤完了应该是可以正常访问网站了，如果网站出现500错误，请看WIKI，很有可能是fastcgi的错误
请看WIKI：https://github.com/ssrpanel/ssrpanel/wiki/%E5%87%BA%E7%8E%B0-open_basedir%E9%94%99%E8%AF%AF
修改完记得重启NGINX和PHP-FPM
````

#### 密码错误
````
如果正确安装完成后发现admin无法登陆，请到SSRPanel目录下执行如下命令：
php artisan upgradeUserPassword

admin的密码将被改为admin
````

#### 重启NGINX和PHP-FPM
````
service nginx restart
service php-fpm restart
````

## 定时任务
````
crontab加入如下命令（请自行修改php、ssrpanel路径）：
* * * * * php /home/wwwroot/ssrpanel/artisan schedule:run >> /dev/null 2>&1

注意运行权限，必须跟ssrpanel项目权限一致，否则出现各种莫名其妙的错误
例如用lnmp的话默认权限用户组是 www:www，则添加定时任务是这样的：
crontab -e -u www
另外要在home目录下要创建一个www文件夹，并授予权限 chmod -R 777 www； 否则www用户的定时身份可能无法运行
````

## 邮件配置
###### SMTP
````
编辑 .env 文件，修改 MAIL_ 开头的配置
````

###### 使用Mailgun发邮件
````
编辑 .env 文件
将 MAIL_DRIVER 值改为 mailgun

然后编辑 config/services.php

请自行配置如下内容
'mailgun' => [
    'domain' => 'mailgun发件域名',
    'secret' => 'mailgun上申请到的secret',
],
````

###### 发邮件失败处理
````
出现 Connection could not be established with host smtp.exmail.qq.com [Connection timed out #110] 这样的错误
因为smtp发邮件必须用到25,26,465,587这四个端口，故需要允许这四个端口通信
````

## 英文版
````
修改 .env 的 APP_LOCALE 值为 en
语言包位于 resources/lang 下，可自行更改
````

## 日志分析（仅支持单机单节点）
````
找到SSR服务端所在的ssserver.log文件
进入ssrpanel所在目录，建立一个软连接，并授权
cd /home/wwwroot/ssrpanel/storage/app
ln -S ssserver.log /root/shadowsocksr/ssserver.log
chown www:www ssserver.log
````

## IP库
```
本项目使用的是纯真IP库，如果需要更新IP库文件，请上纯真官网把qqwry.dat下载并覆盖至 storage/qqwrt.dat 文件
项目里还自带了IPIP的IP库，但是未使用，有开发能力的请自行测试。
```

## HTTPS
```
将 .env 文件里的 REDIRECT_HTTPS 值改为true，则全站强制走https
```

## SSR(R)后端部署教程参考①
#### 手动部署

- 无上报IP版本：
````
wget https://github.com/ssrpanel/shadowsocksr/archive/V3.2.2.tar.gz
tar zxvf V3.2.2.tar.gz
cd shadowsocksr
sh ./setup_cymysql2.sh
配置 usermysql.json 里的数据库链接，
NODE_ID就是节点ID，前端SSRpanel面板里添加节点时会出现ID，
所以请先把前端面板搭好，搭好后进后台添加节点，自动就出现节点ID
````

- 会上报在线IP版本：
```
https://github.com/ssrpanel/shadowsocksr
```
- SSRpanel后端配置   [参考这里](#修改SSR后端配置数据库配置和SSRpanel前端面板连接信息)
## SSR(R)后端部署教程参考②
#### 手动部署
- 安装必要环境
````
yum -y update
yum install -y gcc
yum install -y wget
#以上命令如果报错，可能是因为当前系统已安装。无需理会，继续下一步。
````
- 安装加密模块，让服务器支持更多的加密模式（不是必须要安装的），如果安装过了，可以不用安装
````
wget https://github.com/jedisct1/libsodium/releases/download/1.0.16/libsodium-1.0.16.tar.gz
tar xf libsodium-1.0.16.tar.gz && cd libsodium-1.0.16
./configure && make -j2 && make install
echo /usr/local/lib > /etc/ld.so.conf.d/usr_local_lib.conf
ldconfig
````
- 安装SSR
````
cd ~
git clone https://github.com/LEE-Blog/shadowsocksr.git
cd shadowsocksr
sh setup_cymysql2.sh
sh initcfg.sh
````
##### 修改SSR后端配置（数据库配置和SSRpanel前端面板连接信息）
 - 编辑数据库信息usermysql.json,修改成自己的真实数据库连接信息
 ````
{
"host": "xxx.abc.com",
"port": 3306,
"user": "ssrpanel",
"password": "123456",
"db": "ssrpanel",
"node_id": 1,
"transfer_mul": 2.0,
"ssl_enable": 0,
"ssl_ca": "",
"ssl_cert": "",
"ssl_key": ""
}

host 数据库地址，如果是本机就是127.0.0.1
port 数据库连接端口
user 数据库连接用户，不推荐使用root
password 数据库连接密码
db  ssrpanel面板所在数据库
node_id 节点ID，对应面板里的 节点列表 最左侧的id（请先将面板搭建好，然后创建一个节点，就有节点ID了）
transfer_mul 节点流量计算比例，默认1.0，填1也可以，1表示：用了100M算100M，10表示用了100M算1000M，0.1表示用了100M算10M。
 ````
- 修改SSRpanel前端面板连接信息,编辑文件user-config.json
````
vi user-config.json

"password": "m",
"method": "aes-256-cfb",  // 这项要与前端面板ssrpaenl保持一致
"protocol": "auth_aes128_md5",  // 这项要与前端面板ssrpaenl保持一致
"protocol_param": "5#",  // 这项要与前端面板ssrpaenl保持一致，限制客户端同时连接数量,5#表示只允许5个设备同时连接
"obfs": "tls1.2_ticket_auth",  // 这项要与前端面板ssrpaenl保持一致
"obfs_param": "www.amazon.com,kdp.amazon.com, php.net",  // 建议与前端面板ssrpaenl保持一致，好像客户端可自定义不同的混淆域名
"speed_limit_per_con": 0,
"speed_limit_per_user": 0,
其它保持不变，也可根据情况做修改，不知道什么意思，不要乱改动。

````
user-config设置信息需要跟 SSRPanel 添加的相应节点设置保持一致。  
method = 加密方式；  
protocol = 加密协议；  
obfs = 混淆协议。  
password = 连接密码，保持默认m密码即可，这个不需要修改。  
在不了解的情况下请勿改动，保持默认即可

一切准备就绪，你就可以使用客户端测试节点了。
如果运行后无法使用，请自行检查 usermysql.json 与 user-config.json 两个配置文件。

```
后端节点测试运行：
cd /root/shadowsocksr
python server.py 

如果正常的话，改为后台运行：
cd /root/shadowsocksr
./run.sh
```

## 单端口多用户
````
编辑节点的 user-config.json 文件：
vim user-config.json

将 "additional_ports" : {}, 改为以下内容：
"additional_ports" : {
    "80": {
        "passwd": "统一认证密码", // 例如 SSRP4ne1，推荐不要出现除大小写字母数字以外的任何字符
        "method": "统一认证加密方式", // 例如 aes-128-ctr
        "protocol": "统一认证协议", // 可选值：orgin、verify_deflate、auth_sha1_v4、auth_aes128_md5、auth_aes128_sha1、auth_chain_a
        "protocol_param": "#", // #号前面带上数字，则可以限制在线每个用户的最多在线设备数，仅限 auth_chain_a 协议下有效，例如： 3# 表示限制最多3个设备在线
        "obfs": "tls1.2_ticket_auth", // 可选值：plain、http_simple、http_post、random_head、tls1.2_ticket_auth
        "obfs_param": ""
    },
    "443": {
        "passwd": "统一认证密码",
        "method": "统一认证加密方式",
        "protocol": "统一认证协议",
        "protocol_param": "#",
        "obfs": "tls1.2_ticket_auth",
        "obfs_param": ""
    }
},

保存，然后重启SSR(R)服务。
客户端设置：

远程端口：80
密码：password
加密方式：aes-128-ctr
协议：auth_aes128_md5
混淆插件：tls1.2_ticket_auth
协议参数：1026:@123 (SSR端口:SSR密码)

或

远程端口：443
密码：password
加密方式：aes-128-ctr
协议：auth_aes128_sha1
混淆插件：tls1.2_ticket_auth
协议参数：1026:SSRP4ne1 (SSR端口:SSR密码)

经实测，节点后端使用auth_sha1_v4_compatible，可以兼容auth_chain_a
注意：如果想强制所有账号都走80、443这样自定义的端口的话，记得把 user-config.json 中的 additional_ports_only 设置为 true
警告：经实测单端口下如果用锐速没有效果，很可能是VPS供应商限制了这两个端口
提示：配置单端口最好先看下这个WIKI，防止踩坑：https://github.com/ssrpanel/ssrpanel/wiki/%E5%8D%95%E7%AB%AF%E5%8F%A3%E5%A4%9A%E7%94%A8%E6%88%B7%E7%9A%84%E5%9D%91

````
## 代码更新
````
进入到SSRPanel目录下
1.手动更新： git pull
2.强制更新： sh ./update.sh 

如果你更改了本地文件，手动更新会提示错误需要合并代码（自己搞定），强制更新会直接覆盖你本地所有更改过的文件
如果更新完代码各种错误，请先执行一遍 php composer.phar install
````

## 校时
````
如果架构是“面板机-数据库机-多节点机”，请务必保持各个服务器之间的时间一致，否则会产生：节点的在线数不准确、最后使用时间异常、单端口多用户功能失效等。
推荐统一使用CST时间并安装校时服务：
vim /etc/sysconfig/clock 把值改为 Asia/Shanghai
cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime

重启一下服务器，然后：
yum install ntp
ntpdate cn.pool.ntp.org
````

## 二开规范
````
如果有小伙伴要基于本程序进行二次开发，自行定制，请谨记一下规则（欢迎提PR，我会免费拉你入小群的）
1.数据库表字段请务必使用蟒蛇法，严禁使用驼峰法
2.写完代码最好格式化，该空格一定要空格，该注释一定要注释，便于他人阅读代码
3.本项目中ajax返回格式都是 {"status":"fail 或者 success", "data":[数据], "message":"文本消息提示语"}
````


## 鸣谢
- [@shadowsocks](https://github.com/shadowsocks)
- [@breakwa11](https://github.com/breakwa11)
- [@glzjin](https://github.com/esdeathlove)
- [@orvice](https://github.com/orvice)
- [@ToyoDAdoubi](https://github.com/ToyoDAdoubi)
- [@91yun](https://github.com/91yun)
- [@Akkariiin](https://github.com/shadowsocksrr)
- [@tonychanczm](https://github.com/tonychanczm)
- [@aiyahacke](https://github.com/aiyahacke)
- [@ipcheck](https://ipcheck.need.sh)
- [@cz88](http://www.cz88.net/index.shtml)
- [@ip.sb](https://www.ip.sb)
- [@aiyahacke](https://github.com/aiyahacke)
- [@pch18](https://github.com/pch18/shadowsocksr)
