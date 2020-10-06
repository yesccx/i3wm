# Manjaro i3wm

## 基础配置

### 设置软件镜像源

#### 方式1: 可视化切换
打开pacman可视化的包管理工具->首选择->官方软件仓库->指定镜像源(China)->刷新镜像列表

#### 方式2: 执行命令
```
sudo pacman-mirrors -i -c China -m rank // 更新镜像排名
sudo pacman -Syu // 更新数据源
```

### 安装中文字体包

随便安装一个中文字体包即可，之后在打开 `manjaro-setings-manager`设置本地为`中文`

#### 文泉中文字体

```
sudo pacman -S wqy-microhei
```

### 配置常用软件源（Archlinux-cn镜像加速）

#### 清华源
打开`/etc/pacman.conf`在最底部加入以下内容，具体参考（[清华大学开源软件镜像站](https://mirror.tuna.tsinghua.edu.cn/help/archlinuxcn/)）
```
[archlinuxcn]
SigLevel = Optional TrustedOnly
Server= https://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch
```

配置源后再执行
```
sudo pacman -S archlinuxcn-keyring // 导入GPG key
sudo pacman -Syu // 更新数据源
```

## 安装及配置常用软件

### V2ray
代理工具使用的是V2ray-Desktop

#### 安装
```
sudo pacman -S v2ray-desktop
```


### Google-Chrome

#### 安装
```
sudo pacman -S google-chrome
```

#### 使用
默认没开全局代理的情况下，需要在启用chrome时手动指定，可以通过以下命令启动（通常情况下需要替换端口号）
```
google-chrome-stable --proxy-server="127.0.0.1:1080" // 更多请查看 man google-chrome-stable
```


### Zsh

#### 安装
```
sudo pacman -S zsh
```

#### 配置

更换用户的默认shell
```
chsh -s path/bin/zsh root // 如: chsh -s /bin/zsh yesc
```

安装oh-my-zsh
> 可能由于网络原因安装不成功，可以在能访问这个install.sh文件的电脑上获取到源代码或git pull这个源文件安装
```
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```


### Docker

```
sudo pacman -S docker docker-compose // 安装
sudo systemctl enable docker // 设置开机启动
```


## 其它设置

### 设置本地时间

打开 `manjaro-setings-manager` 在 `时间和日期中` 勾选 `自动设置时间和日期`
