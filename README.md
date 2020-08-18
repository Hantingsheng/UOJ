# Universal Online Judge

![UOJ](https://github.com/Hantingsheng/UOJ/blob/master/UOJ.png)

## 序言

这是UOJ的已归档版本. 在安装之前，请确保 [Docker](https://www.docker.com/) 已安装在您的操作系统上.

UOJ的docker映像是**64位**，因此**32位**主机操作系统可能会导致安装失败, 而且不建议在 `windows` 下安装.

如果发现了UOJ的Bug, 可以联系 `uojmanagers@groups.163.com` 或 `vfleaking@163.com`.

Universal OJ 开源群群号: `590822951`. 想研究源码以及安装有困难的各位, 在群里多交流哈～

## 安装

首先需要下载 [JDK7u76](http://www.oracle.com/technetwork/java/javase/downloads/java-archive-downloads-javase7-521261.html#jdk-7u76-oth-JPR) 和 [JDK8u31](http://www.oracle.com/technetwork/java/javase/downloads/java-archive-javase8-2177648.html#jdk-8u31-oth-JPR), 然后把它们放入 `docker/jdk-7u76-linux-x64.tar.gz` 和 `docker/jdk-8u31-linux-x64.tar.gz` 中. 这两个压缩文件供测评机测评Java. 如果您不想下载这两个大文件，则只需在其中放置两个空的 `.tar.gz` 文件.

接下来，您需要在终端中运行以下命令: (不是在 `docker` 目录中的命令)
```sh
./install
```
如果一切顺利，您将在输出的**最后一行**中看到 `Successfully built <image-id>` .

要启动您的UOJ主服务器，请运行:
```sh
docker run -it -p 80:80 -p 3690:3690 <image-id>
```
如果您在Mac OS上使用docker报错 `std: compile error. no comment`, 您可以使用以下命令替代:
```sh
docker run -it -p 80:80 -p 3690:3690 --cap-add SYS_PTRACE <image-id>
```

UOJ的**默认主机名**是 `local_uoj.ac`, 因此您需要在操作系统中修改主机文件，以将 `127.0.0.1` 映射到 `local_uoj.ac`. (在Linux上是 `/etc/hosts` ) 之后，您可以在Web浏览器中访问UOJ.

UOJ安装后注册的第一个用户将是**超户**(拥有访问网站的全部权限，并且能在上面进行任何操作). 如果您需要另一个超户, 请注册一个用户，并将其 `user group` 更改为 `user_info` 表中的 `<samp>S</samp>`.

运行
```sh
mysql app_uoj233 -u root -p
```
来在终端中登录mysql.

请注意, 如果您只需要一名测评用户, 那么现在无需再进行麻烦的测评用户添加程序了.

但是, 如果您想要更多的测评用户, 则需要一个一个地建立他们. 首先运行：
```sh
./config_judge_client
```
并填写信息:

* uoj测评用户ID: 主服务器的ID.
* uoj测评用户IP: 主服务器的IP地址.
* 测评用户的名字: 你可以取一个你喜欢的名字, 比如 `judger`, `judger_2`, `very_strong_judger`. (最好不要包含特殊字符)

之后, 给出一个sql命令.

接下来, 我们需要运行:
```sh
./install_judge_client
```

来构建docker映像. 如果要在同一台服务器上运行测评器, 则只需运行:
```sh
docker run -it <image-id>
```

并且，您需要使用测评器docker的IP地址完成刚才给出的sql命令, 并修改数据库. 如果你不知道如何获取docker测评器的IP地址, 则在终端中输入以下命令:
```sh
docker inspect --format '{{ .NetworkSettings.IPAddress }}' <container-id>
```

或者, 如果要在其他服务器上运行判断器, 则需要将映像复制到其他服务器上并运行
```sh
docker run -p 2333 -it <image-id>
```
同样, 您需要完成sql命令并修改数据库. 这次, 您需要填写测评机docker主机的IP地址.

在安装过程中可能会遇到很多问题, 祝你好运(*^▽^*).

## 备注

mysql默认密码: `root`

本地测评用户密码: `judger`

您可以在 `/var/www/uoj/app/.config.php` 中更改默认主机名和其他名称.

## 许可证

MIT License
