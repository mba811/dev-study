# Vagrant

之前直接在 Mac 上搭建环境，所以需要安装不同版本的 ruby apache nginx... 再学习搞一个新项目，又要安装不少工具。Hey，我这个机器也要用来看片上网做图片和干其他很多事情的，总之系统上的东西越来越多，让人很不放心。并且，服务器上跑的也不是 Mac 是 Linux 呀，两个环境不一样，也是麻烦不断。

![](http://7q5cfr.com1.z0.glb.clouddn.com/@/vagrant/01.png)

### 系统安装

以前安装一套新系统要去 Ubuntu 官网下载一个镜像文件，然后新建一个 vitualbox 虚拟机，然后需要有人值守的去完成整个系统安装过程，很是繁琐。但用了 Vagrant 以后这个过程变成无人值守的了，意思就是一个命令搞定一切。过程是这样：

首先，保证我的系统上有两个基础软件，一个是 vagrant 一个就是 virtualbox ，安装过程就是双击然后下一步下一步而已，没啥好说的。装好之后，到命令行中就有 vagrant 这个命令了。那么这个时候是不是就要去下载系统光盘了呢？ NO，有一个网站叫做[https://vagrantcloud.com/](https://vagrantcloud.com/)，到上面搜一下 ubuntu，排名第一的这个 `ubuntu / trusty64` 就是 ubuntu 公司提供的 ubuntu 14.04 的64位系统镜像文件。

新建一个目录，执行 vagrant init
    
    mkdir myproject
    cd myproject
    vagrant init ubuntu/trusty64
    

这样目录中就会多出一个文件叫 Vagrantfile 所有的机关也就都在这里了，比如这里面有一句 `config.vm.box = "ubuntu/trusty64"`这样，执行
    
    vagrant up
    

就会到 vagrant cloud 网站上下载 box 进行安装了。如果是第一次下载，可以需要等十来分钟，所以一般我是早上起来干这个活，网速比较快。但是，现在的情况是我之前以经在另外一个项目中执行过这个操作了，那么也就是这个 box 已经存在我本地机器上了。这时候，vagrant 导入这个 box 进来，只需要几秒钟就可以在创建出一个新的 virtualbox 虚拟系统了，vagrant 的基本思路是为每一个项目创建自己的一个虚拟机。而且这个系统和之前的系统是完全隔离的。如果我过一段时间不需要这台虚拟机了，执行 `vagrant destroy` 就都清理干净了，而且其他同样使用这个 box 虚拟机也不会受到影响。

打开 virtuabox 的图形界面可以看到又多了一个虚拟机，也可以去修改配置，比如默认内存大小是 512，我一般用 2048 。Vigrant 有一个好处是所有一切都在命令行和配置文件中搞定，这样比图形界面用起来方便的多，所以一般是不需要启动 virtualbox 图形界面的。

### 基本配置

就拿修改内存为例子。打开 Vagrantfile 添加
    
    config.vm.provider "virtualbox" do |v|
      v.memory = 2048
    end
    

这样运行 `vagrant reload` 就修改成功了。

创建新用户。`vagrant ssh` 登陆进来的用户名是 vagrant，这个用户挺好，执行 sudo 是不需要输入密码的，开发中实际使用挺好用的。不过如果我非要创建自己的用户也是可以的。
    
    sudo adduser peter --ingroup sudo
    

这样 Ctrl-D 退出虚拟机，如何用这个用户的身份来登录呢？运行 `vagrant up` 的时候，可以看到有输出
    
    ==> default: Forwarding ports...
    default: 22 => 2222 (adapter 1)
    

所以登录命令可以这样：
    
    ssh peter@127.0.0.1 -p 2222
    

输入密码就可以登录成功了。可以把这个命令作为一个 alias 放到自己的 .bashrc 或者 .zshrc 文件中。

设置 IP 。添加
    
    config.vm.network :private_network, ip: "192.168.33.21"
    

这样，就可以到 /etc/hosts 文件下面填写
    
    192.168.33.21 myproject
    

以后可以浏览器中用 myproject 来访问里面的网站了。

共享文件夹。默认情况下，Vagrantfile 所在的这个文件夹，会自动对应虚拟机里面的 `/vagrant` 这个目录。这个意味着我不需要在虚拟机里面配置 sublimeText vim git 搜狗输入法 这些工具了，Mac 依旧是我写代码的环境。ubuntu 虚拟机是项目的安装运行环境。这个分工是太合理了！

通常的做法，项目源码就放在 myproject 目录下，把 Vagrantfile 和源码一起用 git 统一控制。我这里先创建一个 index.html 文件。然后去设置 apache
    
    cd /etc/apache2/sites-avaiable
    sudo vim myproject.conf
    

填入下面内容：
    
    <VirtualHost *:80>
      ServerName myproject.dev
      DocumentRoot /vagrant/
    </VirtualHost>
    

在 sites-enable 下面创建符号链接并且加载新配置
    
    sudo a2ensite myproject.conf
    cd ..
    sudo rm sites-enabled/000-default.conf
    sudo service apache2 reload
    

但是浏览器中访问 myproject.dev 却报错
    
    You don't have permission to access / on this server.
    

原因是在虚拟机里面 /etc/apache2/apache2.conf 里面有这样的设置：
    
    <Directory />
      Options FollowSymLinks
      AllowOverride None
      Require all denied
    </Directory>
    

除了 `/var/www` 等特殊声明的目录外，其他位置的访问一律 'denied' 。可以这样来解决：
    
    sudo rm  -rf /var/www
    sudo ln -fs /vagrant /var/www
    

然后到 myproject.conf 中，更改 `DocumentRoot /vagrant/` 为 `DocumentRoot /var/www`
    
    sudo service apache2 reload
    

再次访问 myproject.dev 一切正常了。

### 自定义脚本

实际开发情况是复杂的，从 vagrant cloud 下载的 box 中不可能包含我所需要的所有工具。这样，我可以制作自己的 box 上传的 vagrant cloud 上面，以备后面自己用，或者分享给朋友。但是另外一个更为灵活的方式是：我就锁定一个干净的 ubuntu 系统 box 。每个项目的虚拟机都基于这个 box 来做，vagrant 有个叫法 Base Box 。  
然后我在上面执行脚本来自定制系统。chef puppet 都是可以的，但是有一定的学习难度，最简单的是直接写 shell 脚本。具体操作是这样。

打开 Vagrantfile 文件，在 do-end 块内部添加
    
    config.vm.provision "shell", inline: $script, privileged: false
    

`privileaged: false` 保证脚本是以普通用户身份执行，去掉后脚本执行者为 root 。

在文件开始位置添加
    
    $script = <<SCRIPT
        echo "start inline script here"
    SCRIPT
    

接下来就可以写要执行的脚本了。
    
    sudo apt-get install -y wget
    wget http://haoduoshipin.com
    

如果把这个 project 放到一个全新的系统中，首次执行 `vagrant up` 当虚拟机系统安装完毕之后这些脚本也会自动执行。

如果像现在我的情况，机器正处于运行状态，执行命令
    
    vagrant provision
    

可以执行脚本。然后登陆到机器上查看，会发现 wget 操作成功了。

举一个实际例子，在我的项目 happycasts 里面也添加了 [Vagrantfile](https://github.com/happypeter/happycasts/blob/master/Vagrantfile) 可以达成的效果是这样，你在你自己的 Mac 或者 Windows 机器上装好 vagrant 和 virtualbox ，然后下载 happycasts 源码，只需要
    
    cd happycasts/
    vagrant up
    

这样就可以成功安装 ubuntu 14.04 并且整个的开发运行环境也都配置好了，到浏览器中输入 Vagrantfile 给定的 ip 地址，就可以看到项目已经跑起来了，怎么样，方便吧。

好，更多内容可以参考[Vagrant Doc](https://docs.vagrantup.com/v2/) 。谢谢收看，下周再见啦！


>rails 核心开发团队用的 vagrant 脚本：[https://github.com/rails/rails-dev-box/blob/master/bootstrap.sh](https://github.com/rails/rails-dev-box/blob/master/bootstrap.sh)


```
# The output of all these installation steps is noisy. With this utility
# the progress report is nice and concise.
function install {
    echo installing $1
    shift
    apt-get -y install "$@" >/dev/null 2>&1
}

echo updating package information
apt-add-repository -y ppa:brightbox/ruby-ng >/dev/null 2>&1
apt-get -y update >/dev/null 2>&1

install 'development tools' build-essential

install Ruby ruby2.2 ruby2.2-dev
update-alternatives --set ruby /usr/bin/ruby2.2 >/dev/null 2>&1
update-alternatives --set gem /usr/bin/gem2.2 >/dev/null 2>&1

echo installing Bundler
gem install bundler -N >/dev/null 2>&1

install Git git
install SQLite sqlite3 libsqlite3-dev
install memcached memcached
install Redis redis-server
install RabbitMQ rabbitmq-server

install PostgreSQL postgresql postgresql-contrib libpq-dev
sudo -u postgres createuser --superuser vagrant
sudo -u postgres createdb -O vagrant activerecord_unittest
sudo -u postgres createdb -O vagrant activerecord_unittest2

debconf-set-selections <<< 'mysql-server mysql-server/root_password password root'
debconf-set-selections <<< 'mysql-server mysql-server/root_password_again password root'
install MySQL mysql-server libmysqlclient-dev
mysql -uroot -proot <<SQL
CREATE USER 'rails'@'localhost';
CREATE DATABASE activerecord_unittest  DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
CREATE DATABASE activerecord_unittest2 DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
GRANT ALL PRIVILEGES ON activerecord_unittest.* to 'rails'@'localhost';
GRANT ALL PRIVILEGES ON activerecord_unittest2.* to 'rails'@'localhost';
GRANT ALL PRIVILEGES ON inexistent_activerecord_unittest.* to 'rails'@'localhost';
SQL

install 'Nokogiri dependencies' libxml2 libxml2-dev libxslt1-dev
install 'ExecJS runtime' nodejs

# Needed for docs generation.
update-locale LANG=en_US.UTF-8 LANGUAGE=en_US.UTF-8 LC_ALL=en_US.UTF-8

echo 'all set, rock on!'
```