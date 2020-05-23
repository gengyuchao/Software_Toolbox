# Software_Toolbox - 搜狗输入法修复文件

搜狗输入法有时在 Qt5 Creator 无法切换输入法（fcitx），存在不能录入汉字问题。原因是 Qt5.4 后对之前 Qt5 版本不再二进制兼容，libfcitxplatforminputcontextplugin.so 需要编译最新的 fcitx-qt5

这里保存记录一下，我在 Ubuntu 20.04 上编译 fcitx-qt5 成功并使用的 libfcitxplatforminputcontextplugin.so 文件，
方便日后的使用。


下面是编译的说明，感兴趣的同学可以自己尝试编译
===

## 1. 步骤

### 1.1 编译 fcitx-qt5 源码

#### 1、编译 fcitx-qt 需要 cmake，安装 cmake 命令，如果已经安装，请略过;

```
sudo apt-get install cmake
```

#### 2、安装 `fcitx-libs-dev`;

```
sudo apt-get install fcitx-libs-dev
```

#### 3、设置 qmake 的环境变量，这一步很重要且环境变量的值因人而异

##### 3.1. 首先确定你的Qt的安装目录，我这里是 ~/software/Qt5.6.0/ ，你的或者可能在 /home/<用户名>/Qt5.6.0/

##### 3.2. 
`export PATH="/5.6/gcc_64/bin":$PATH`

#### 4、下载fcitx-libs 源码

##### 4.1. 在 github 下载源码

源码地址 `https://github.com/fcitx/fcitx-qt5`

##### 4.2. 

`git clone https://github.com/fcitx/fcitx-qt5`

#### 5、编译 fcitx-qt5

这一步遇到了很多依赖错误，每个错误都可以在网上找到解决方法。

```
cd fcitx-qt5
cmake .
make 
sudo make install
```

### 1.2. 拷贝 so 文件

#### 1、编译完成后，需要把编译得到的 libfcitxplatforminputcontextplugin.so 拷贝到 Qt5.5 安装目录的 Tools/QtCreator/bin/plugins/platforminputcontexts 或 Qt5.6 安装目录的 Tools/QtCreator/lib/Qt/plugins/platforminputcontexts，注意：两个目录根据你的Qt版本而定，Qt安装目录因人而异。

#### 2、复制：

Qt 5.5：cp platforminputcontext/libfcitxplatforminputcontextplugin.so /Tools/QtCreator/bin/plugins/platforminputcontexts
Qt 5.6：cp platforminputcontext/libfcitxplatforminputcontextplugin.so /Tools/QtCreator/lib/Qt/plugins/platforminputcontexts
我这里是：cp platforminputcontext/libfcitxplatforminputcontextplugin.so ~/software/Qt5.6.0/Tools/QtCreator/lib/Qt/plugins/platforminputcontexts

我自己使用的时候将系统 `/usr/lib/x86_64-linux-gnu/qt5/plugins/platforminputcontexts` 中的 libfcitxplatforminputcontextplugin.so 文件也替换了，希望能够对所有程序起作用，但是该方法我目前并没有验证它的有效性。

### 1.3 添加额外的环境变量

这一步我没有做。

```
echo 'export XMODIFIERS=@im=fcitx' >> .bashrc 
echo 'export QT_IM_MODULE=fcitx' >> .bashrc
```
大功告成！