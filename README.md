# Manjaro i3wm
i3wm安装与配置记录，目前只是拿来自用，很多地方没做过多说明

- 版本：Manjaro-i3 20.1.1
- 内核：Linux 5.8.11-1


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

随便安装一个中文字体包即可，之后在打开 `manjaro-setings-manager`设置本地语言为`中文`

#### 文泉中文字体

```
sudo pacman -S wqy-microhei
```

### 安装图标字体

Font Awesome图标字体，会在配置i3blocks时用到

```
sudo pacman -S ttf-font-awesome
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

### 配置Xterm
该配置用来控制所有`GUI`程序的窗口配置项，和兼容运行程序，如想让程序中能使用`fcitx`输入法，则需要配置`.xprofile`中的一些环境变量。如果要设置`GUI`程序的窗口`DPI`则可以参考`.Xresources`文件。
- [.xprofile](I3wm/.xprofile)
- [.Xresources](I3wm/.Xresources)
> 通常直接覆盖这两个文件即可，在用户目录下。
>
> 可以视情况调整DPI

### 配置i3wm
参考[i3wm相关配置文件](I3wm/.i3/config)，直接覆盖文件或对比修改（不存在时手动创建即可）。
```
~/.i3/config
```
> i3wm有些项依赖软件，建议先安装下面的所有软件后，再配置。

### 配置picom
参考[picom相关配置文件](Software/Picom)，直接覆盖文件或对比修改（不存在时手动创建即可）。
```
~/.config/picom/config
```

同时配置`picom`自启动，参考[配置i3wm](#配置i3wm)
```
vim ~/.i3/config // 找到 # Autostart applications 区，加入或修改为以下行
exec_always --no-startup-id nitrogen --restore; sleep 1; picom -b --config ~/.config/picom/config
```


## 安装及配置常用软件

### Fcitx
五笔、拼音输入法

#### 安装
```
sudo pacman -S fcitx
```

#### 配置
参考[Fcitx相关配置文件](Software/Fcitx)，直接覆盖文件或对比修改（不存在时手动创建即可）。
```
~/.config/fcitx
├── addon
│   ├── fcitx-chttrans.conf
│   ├── fcitx-clipboard.conf
│   └── fcitx-fullwidth-char.conf
├── cached_layout
├── clipboard
├── conf
│   ├── fcitx-autoeng.config
│   ├── fcitx-chttrans.config
│   ├── fcitx-classic-ui.config
│   ├── fcitx-clipboard.config
│   ├── fcitx-imselector.config
│   ├── fcitx-keyboard.config
│   ├── fcitx-notify.config
│   ├── fcitx-pinyin.config
│   ├── fcitx-pinyin-enhance.config
│   ├── fcitx-quickphrase.config
│   ├── fcitx-spell.config
│   ├── fcitx-unicode.config
│   ├── fcitx-xim.config
│   └── fcitx-xkb.config
├── config
├── data
│   └── layout_override
├── profile
└── skin
    └── default
        └── fcitx_skin.conf
```

同时要修改`Xterm`相关配置，使所有`GUI`界面能够使用`fcitx`，参考[配置Xterm](#配置Xterm)

同时配置`fcitx`自启动，参考[配置i3wm](#配置i3wm)
```
vim ~/.i3/config // 找到 # Autostart applications 区，加入或修改为以下行
exec --no-startup-id fcitx
```
> 注意，修改`fcitx`配置要在退出`fcitx`的情况下进行，否则配置文件会被重写为初始值

### V2ray
代理工具使用的是V2ray-Desktop

#### 安装
```
sudo pacman -S v2ray-desktop
```

#### 配置
同时配置`V2ray-Desktop`自启动，参考[配置i3wm](#配置i3wm)
```
vim ~/.i3/config // 找到 # Autostart applications 区，加入或修改为以下行
exec --no-startup-id v2ray-desktop
```

### Google-Chrome

#### 安装
```
sudo pacman -S google-chrome
```

#### 使用
默认没开全局代理的情况下，需要在启用Chrome时手动指定代理（与v2ray配合），可以通过以下命令启动（通常情况下需要替换端口号）
```
google-chrome-stable --proxy-server="127.0.0.1:1080" // 更多请查看手册 man google-chrome-stable
```

#### 配置

##### 扩展SwitchyOmega
导入配置 [SwitchyOmega.bak](Software/Chrome/SwitchyOmega.bak)

##### 更换默认浏览器为Chrome
```
echo "export BROWSER=/usr/bin/google-chrome-stable" >> ~/.profile
```
同时编辑 ~/.config/mimeapps.list 替换所有关键字`userapp-Pale Moon`为`google-chrome`，因为`Pale Moon`是默认的浏览器


### Zsh

#### 安装
```
sudo pacman -S zsh
```

#### 配置

##### 更换默认shell
```
chsh -s path/bin/zsh 用户名 // 如: chsh -s /bin/zsh yesc
```

##### 安装oh-my-zsh
> 可能由于网络原因安装不成功，可以在能访问这个install.sh文件的电脑上获取到源代码或git pull这个源文件安装
```
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

##### 配置及安装插件
```
cd ~/.oh-my-zsh/plugins
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git // 安装 zsh-syntax-highlighting 插件
```
之后编辑`~/.zshrc`文件，参考[zshrc配置文件](Software/Zsh/.zshrc)，主要配置项是
- 加入一些插件：`plugins` 第71行
- 使history显示时间：`HIST_STAMPS` 第61行

配置完成后执行`source ~/.zshrc` 重载或重启电脑

#### 更换oh-my-zsh主题
参考[custom-robbyrussell](Software/Zsh/oh-my-zsh/themes/custom-robbyrussell.zsh-theme)
```
~/.oh-my-zsh/themes/ // 放入该目录下，并修改zsh配置即可
```

### Xfce4-Terminal
Xfce终端，代替默认的终端

#### 安装
```
sudo pacman -S xfce4-terminal
```

#### 配置
参考[Xfce4-Terminal相关配置文件](Software/Xfce4-Terminal)，直接覆盖文件或对比修改（不存在时手动创建即可）。
```
~/.config/xfce4/terminal
├── accels.scm
└── terminalrc
```


#### 配置
同时配置`xfce-terminal`为默认快捷启用的终端，参考[配置i3wm](#配置i3wm)
```
vim ~/.i3/config // 找到 # start a terminal 区，加入或修改为以下行
```

### Rofi
代替dmenu，快捷应用启动器

#### 安装
```
sudo pacman -S rofi
```

#### 配置
参考[Rofi相关配置文件](Software/Rofi)，直接覆盖文件或对比修改（不存在时手动创建即可）。
```
~/.config/rofi/
├── config
```

同时配置`Rofi`为默认快捷启用器，参考[配置i3wm](#配置i3wm)
```
vim ~/.i3/config // 找到 # start program launcher 区，加入或修改为以下行
```

### I3blocks
代替i3status，在状态栏显示一些硬件信息、温度、系统时间等

#### 安装
```
sudo pacman -S i3blocks
```

#### 配置
参考[Rofi相关配置文件](Software/I3blocks)，直接覆盖文件或对比修改（不存在时手动创建即可）。
```
~/.config/i3blocks/
├── blocks
├── config
```

同时配置`i3blocks`为默认bar status，参考[配置i3wm](#配置i3wm)
```
vim ~/.i3/config // 找到并替换bar块中的status_command为如下
status_command i3blocks -c ~/.config/i3blocks/config
```

### Flameshot
截图软件

#### 安装
```
sudo pacman -S flameshot
```

### Clipit
通常不需要再次安装，manjaro-i3版自带

#### 安装
```
sudo pacman -S clipit
```

#### 配置
参考[Clipit相关配置文件](Software/Clipit)，直接覆盖文件或对比修改（不存在时手动创建即可）。
```
~/.config/clipit/
├── clipitrc
```

### Variety
管理和设置壁纸

#### 安装
```
sudo pacman -S variety
```

### Postman

#### 安装
```
sudo pacman -S postman
```

## 开发相关

### Node和Npm

#### 安装
```
sudo pacman -S nodejs && sudo pacman -S yarn
```

#### 配置
yarn 镜像加速，具体参考[淘宝镜像说明文件](https://developer.aliyun.com/mirror/NPM?from=tnpm)
```
yarn config set registry https://registry.npm.taobao.org
```

### Docker和Docker-Compose

#### 安装
```
sudo pacman -S docker docker-compose // 安装
sudo systemctl enable docker // 设置开机启动
```

#### 配置
docker镜像加速配置[手册](https://lug.ustc.edu.cn/wiki/mirrors/help/docker/)



## 其它设置

### 设置本地时间

打开 `manjaro-setings-manager` 在 `时间和日期中` 勾选 `自动设置时间和日期`


## 相关问题

### 软件出问题时，如何快速定位问题？
可以在端终运行这个软件，一些警告和错误会打印在终端控制台上

### 更改了配置后没生效？
如果更改了`i3wm`的配置文件需要重载`i3wm`，通常是`Alt` + `Shifht` + `R`，或者是重启电脑即可。如果是别的软件，可能需要重载配置项，如`zsh`可以是`source ~/.zshrc`。

### 笔记本Fn快捷键无效
需要安装对应的驱动，如果已有驱动的情况下还是无效，可以尝试逐个解决。如快捷键调节屏幕亮度无效时，可能需要在电脑`电源管理软件(如：xfce4-power-manager-settings)`中开启快捷键调节方式。

### 笔记本不显示电源管理图标？
默认使用的是`xfce4-power-manager`，需要在`~/.i3/config`中配置自启动

### 软件自启动?
前往`~/.i3/config`中配置，找到 # Autostart applications 区，加入要启动的程序，如下
```
exec --no-startup-id fcitx
```
具体参考[手册](https://i3wm.org/docs/userguide.html#exec)

### 开机后蓝牙没有开启？
具体参考[手册](https://wiki.archlinux.org/index.php/Bluetooth_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)#%E5%BC%80%E6%9C%BA%E5%90%8E%E8%87%AA%E5%8A%A8%E5%90%AF%E5%8A%A8)

### 扩展屏幕
具体参考[手册](https://wiki.archlinux.org/index.php/Xrandr_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87))

### 获取窗口信息
执行`xprop`然后鼠标点击一个窗口