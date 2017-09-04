如何在 Ubuntu16.04 下安装 Nginx
===============================
介绍
----
>Nginx (engine x) 是一个高性能的HTTP和反向代理服务器，也是一个IMAP/POP3/SMTP服务器。Nginx是由伊戈尔·赛索耶夫为俄罗斯访问量第二的Rambler.ru站点（俄文：Рамблер）开发的，第一个公开版本0.1.0发布于2004年10月4日。
 其将源代码以类BSD许可证的形式发布，因它的稳定性、丰富的功能集、示例配置文件和低系统资源的消耗而闻名。2011年6月1日，nginx 1.0.4发布。
 Nginx是一款轻量级的Web 服务器/反向代理服务器及电子邮件（IMAP/POP3）代理服务器，并在一个BSD-like 协议下发行。其特点是占有内存少，并发能力强，事实上nginx的并发能力确实在同类型的网页服务器中表现较好，中国大陆使用nginx网站用户有：百度、京东、新浪、网易、腾讯、淘宝等。

>这里我们将指导如何在 Ubuntu16.04 服务器安装 Nginx。

准备工作
--------
>开始指导前，先要有非 root 用户的```sudo```受权配置服务。指导如何为 Ubuntu 16.04 配置用户。
 首先要有个有效的非 root 用户，登录。

步骤 1：安装 Nginx
------------------
>Nginx 在 Ubuntu 系统默认有效的，所以安装是相当简单的。

>由于这是我们第一次交互```apt```系统包，我们将更新本地包。所以我们会访问很多包列表。然后，我们可以安装 Nginx：
```text
sudo apt-get update
sudo apt-get install nginx
```
>后面的步骤，apt-get 将安装 Nginx 和服务器必要的依赖。

步骤 2：调整防火墙
------------------
>在我们测试 Nginx 前，我们需要重新配置我们的防火墙软件，允许访问服务。Nginx 用 ufw 注册它自己服务
Nginx 在安装的时候，为它自己用```ufw```在防火墙上注册一服务。这样就可以相应容易的访问 Nginx。

We can list the applications configurations that ufw knows how to work with by typing:
>我们可以用```ufw```知道如何在工作台显示应用程序配置列表：
```text
sudo ufw app list
```
>你应该得到的应用程序列表如下：
```text
输出：
有效应用：
  Nginx Full
  Nginx HTTP
  Nginx HTTPS
  OpenSSH
```
>正如你所见的，三个有效的 Nginx 说明：
```text
Nginx Full: This profile opens both port 80 (normal, unencrypted web traffic) and port 443 (TLS/SSL encrypted traffic)
Nginx HTTP: This profile opens only port 80 (normal, unencrypted web traffic)
Nginx HTTPS: This profile opens only port 443 (TLS/SSL encrypted traffic)
```
It is recommended that you enable the most restrictive profile that will still allow the traffic you've configured. Since we haven't configured SSL for our server yet, in this guide, we will only need to allow traffic on port 80.

You can enable this by typing:
```text
sudo ufw allow 'Nginx HTTP'
```
You can verify the change by typing:
```text
sudo ufw status
```
You should see HTTP traffic allowed in the displayed output:

```text
Output
Status: active

To                         Action      From
--                         ------      ----
OpenSSH                    ALLOW       Anywhere
Nginx HTTP                 ALLOW       Anywhere
OpenSSH (v6)               ALLOW       Anywhere (v6)
Nginx HTTP (v6)            ALLOW       Anywhere (v6)
```
Step 3: Check your Web Server
-------
At the end of the installation process, Ubuntu 16.04 starts Nginx. The web server should already be up and running.

We can check with the systemd init system to make sure the service is running by typing:

```text
systemctl status nginx
```
```text
Output
● nginx.service - A high performance web server and a reverse proxy server
   Loaded: loaded (/lib/systemd/system/nginx.service; enabled; vendor preset: enabled)
   Active: active (running) since Mon 2016-04-18 16:14:00 EDT; 4min 2s ago
 Main PID: 12857 (nginx)
   CGroup: /system.slice/nginx.service
           ├─12857 nginx: master process /usr/sbin/nginx -g daemon on; master_process on
           └─12858 nginx: worker process
```
As you can see above, the service appears to have started successfully. However, the best way to test this is to actually request a page from Nginx.

You can access the default Nginx landing page to confirm that the software is running properly. You can access this through your server's domain name or IP address.

If you do not have a domain name set up for your server, you can learn how to set up a domain with DigitalOcean here.

If you do not want to set up a domain name for your server, you can use your server's public IP address. If you do not know your server's IP address, you can get it a few different ways from the command line.

Try typing this at your server's command prompt:
```text
ip addr show eth0 | grep inet | awk '{ print $2; }' | sed 's/\/.*$//'
```
You will get back a few lines. You can try each in your web browser to see if they work.

>修改网卡的名称：```etc/udev/rules.d/70-persistent-net.rules```
```text
sudo vi /etc/udev/rules.d/70-persistent-net.rules
```


An alternative is typing this, which should give you your public IP address as seen from another location on the internet:
```text
sudo apt-get install curl
curl -4 icanhazip.com
```
When you have your server's IP address or domain, enter it into your browser's address bar:
```html
http://server_domain_or_IP
```
You should see the default Nginx landing page, which should look something like this:

Nginx default page

This page is simply included with Nginx to show you that the server is running correctly.

Step 4: Manage the Nginx Process
-------
Now that you have your web server up and running, we can go over some basic management commands.

To stop your web server, you can type:
```text
sudo systemctl stop nginx
```
To start the web server when it is stopped, type:
```text
sudo systemctl start nginx
```
To stop and then start the service again, type:
```text
sudo systemctl restart nginx
```
If you are simply making configuration changes, Nginx can often reload without dropping connections. To do this, this command can be used:
```text
sudo systemctl reload nginx
```
By default, Nginx is configured to start automatically when the server boots. If this is not what you want, you can disable this behavior by typing:
```text
sudo systemctl disable nginx
```
To re-enable the service to start up at boot, you can type:
```text
sudo systemctl enable nginx
```

Step 5: Get Familiar with Important Nginx Files and Directories
-----
Now that you know how to manage the service itself, you should take a few minutes to familiarize yourself with a few important directories and files.

### Content
>/var/www/html: The actual web content, which by default only consists of the default Nginx page you saw earlier, is served out of the /var/www/html directory. This can be changed by altering Nginx configuration files.

### Server Configuration
>/etc/nginx: The nginx configuration directory. All of the Nginx configuration files reside here.

>/etc/nginx/nginx.conf: The main Nginx configuration file. This can be modified to make changes to the Nginx global configuration.

>/etc/nginx/sites-available/: The directory where per-site "server blocks" can be stored. Nginx will not use the configuration files found in this directory unless they are linked to the sites-enabled directory (see below). Typically, all server block configuration is done in this directory, and then enabled by linking to the other directory.

>/etc/nginx/sites-enabled/: The directory where enabled per-site "server blocks" are stored. Typically, these are created by linking to configuration files found in the sites-available directory.

>/etc/nginx/snippets: This directory contains configuration fragments that can be included elsewhere in the Nginx configuration. Potentially repeatable configuration segments are good candidates for refactoring into snippets.

### Server Logs
>/var/log/nginx/access.log: Every request to your web server is recorded in this log file unless Nginx is configured to do otherwise.

>/var/log/nginx/error.log: Any Nginx errors will be recorded in this log.

### Conclusion
Now that you have your web server installed, you have many options for the type of content to serve and the technologies you want to use to create a richer experience.

Learn how to use Nginx server blocks here. If you'd like to build out a more complete application stack, check out this article on how to configure a LEMP stack on Ubuntu 16.04.

参考资料
--------
- [How To Install Nginx on Ubuntu 16.04](https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-16-04)