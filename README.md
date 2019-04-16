# tmux 烹饪书
## tmux安装
## ubuntu安装
`apt install tmux`

### centos安装
`yum install tmux`安装的是1.8版，很老的版本，可以使用以下脚本安装2.8稳定版  
install-tmux2.8.sh (https://gist.github.com/pokev25/4b9516d32f4021d945a140df09bf1fde)  
``` bash
# Install tmux 2.8 on Centos

# install deps
yum install gcc kernel-devel make ncurses-devel

# DOWNLOAD SOURCES FOR LIBEVENT AND MAKE AND INSTALL
curl -LOk https://github.com/libevent/libevent/releases/download/release-2.1.8-stable/libevent-2.1.8-stable.tar.gz
tar -xf libevent-2.1.8-stable.tar.gz
cd libevent-2.1.8-stable
./configure --prefix=/usr/local
make
make install

# DOWNLOAD SOURCES FOR TMUX AND MAKE AND INSTALL

curl -LOk https://github.com/tmux/tmux/releases/download/2.8/tmux-2.8.tar.gz
tar -xf tmux-2.8.tar.gz
cd tmux-2.8
LDFLAGS="-L/usr/local/lib -Wl,-rpath=/usr/local/lib" ./configure --prefix=/usr/local
make
make install

# pkill tmux
# close your terminal window (flushes cached tmux executable)
# open new shell and check tmux version
tmux -V
```
> prefix = ctrl + b

## tmux概念
一个ssh连接可有多个tmux session  
session里面有windows  
windows里面有panel  
ssh掉线重登录tmux中的现场还在  
tmux之所以能保存现场，因为其进程一直在后台运行  


## 个人配置
config file
```
# .tmux.conf
# setw -g mode-keys vi

# 使用`代替ctrl+b,按两下`输出`字符
unbind ^b
bind-key ` send-prefix
set -g prefix `

# `+r 应用tmux.conf文件
bind r source-file ~/.tmux.conf \; display-message "Config reloaded"

# 切分窗口，-横切，|竖切
# unbind '"'
bind - splitw -v -c '#{pane_current_path}'
# unbind %
bind | splitw -h -c '#{pane_current_path}'

set-option -g mouse on

# 绑定hjkl键为窗格切换的上下左右键
bind -r k select-pane -U # 绑定k为↑
bind -r j select-pane -D # 绑定j为↓
bind -r h select-pane -L # 绑定h为←
bind -r l select-pane -R # 绑定l为→

# 绑定大写KJHL键为窗格上下左右调整边缘的快捷指令
bind -r K resizep -U 10 # 绑定K为往↑调整面板边缘10个单元格
bind -r J resizep -D 10 # 绑定J为往↓调整面板边缘10个单元格
bind -r H resizep -L 10 # 绑定H为往←调整面板边缘10个单元格
bind -r L resizep -R 10 # 绑定L往→调整面板边缘10个单元格

# 设置tmux的延迟，文档说当有干扰的时候可以设置这个参数，比如影响vim编辑的时候
set -s escape-time 1
```

## 热加载配置

`prefix + :` 进入命令模式然后输入  
`source-file ~/.tmux.conf`  

按我个人配置，后续只要`prefix + r`即可  

## 系统操作
| command | usage | 
| -------- | -------- |
| prefix + ?     | 列出所有快捷键；按q返回     |
| prefix + d     | 脱离当前会话,返回Shell界面     |
| prefix + D     | 选择要脱离的会话；在同时开启了多个会话时使用     |
| prefix + Ctrl+z     | 挂起当前会话     |
| prefix + :     | 进入命令行模式,此时可以输入tmux支持的命令     |
| **prefix + [**     | **进入复制模式；此时的操作与vi相同，按q/Esc退出**     |


## Session管理
### 新建session并命名
``` bash
tmux new -s myname
```

### 打开已存在的session
``` bash
tmux a -t session_name  
```

### 断开session
``` bash
# 方式一
exit   # 如果只有单个tmux窗口,这样退出后tmux session就不存在了

# 方式二
prefix + d # d = detach, session保留
```

### 查看session列表
``` bash
tmux ls
0: 1 windows (created Wed Dec  5 16:14:47 2018) [232x56]
2: 2 windows (created Wed Dec  5 19:50:16 2018) [265x58]
```

## 窗口（如同浏览器tab）
### 新建窗口
```
prefix + c
```

### 切换窗口
```
prefix + [0~9]
```

### 重命名窗口
```
prefix + ,
```

### 删除窗口
```
prefix + &
```

## 窗格（一个大窗分成几个格）
水平分隔窗格
```
prefix + -
```

垂直分隔窗格
```
prefix + |
```

移动窗格
```
prefix + hjkl
prefix + ↑↓←→
```

置换窗格
```
prefix + { # 向前置换
prefix + } # 向后置换
```

调整窗格边界
```
prefix + KJHL # 上下左右调整面板边界
```








