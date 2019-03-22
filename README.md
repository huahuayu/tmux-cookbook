# tmux cookbook

> prefix = ctrl + b

## tmux概念
一个ssh连接可有多个tmux session  
session里面有windows  
windows里面有panel  
ssh掉线重登录tmux中的现场还在  
tmux之所以能保存现场，因为其进程一直在后台运行  


## personal config
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

# 切分面板，-横切，|竖切
# unbind '"'
bind - splitw -v -c '#{pane_current_path}'
# unbind %
bind | splitw -h -c '#{pane_current_path}'

set-option -g mouse on

# 绑定hjkl键为面板切换的上下左右键
bind -r k select-pane -U # 绑定k为↑
bind -r j select-pane -D # 绑定j为↓
bind -r h select-pane -L # 绑定h为←
bind -r l select-pane -R # 绑定l为→

# 绑定大写KJHL键为面板上下左右调整边缘的快捷指令
bind -r K resizep -U 10 # 绑定K为往↑调整面板边缘10个单元格
bind -r J resizep -D 10 # 绑定J为往↓调整面板边缘10个单元格
bind -r H resizep -L 10 # 绑定H为往←调整面板边缘10个单元格
bind -r L resizep -R 10 # 绑定L往→调整面板边缘10个单元格

# 设置tmux的延迟，文档说当有干扰的时候可以设置这个参数，比如影响vim编辑的时候
set -s escape-time 1
```

## reload conf file

`prefix + :` 进入命令模式然后输入  
`source-file ~/.tmux.conf`  

按我个人配置，后续只要`prefix + r`即可  

## System operation
| command | usage | 
| -------- | -------- |
| prefix + ?     | 列出所有快捷键；按q返回     |
| prefix + d     | 脱离当前会话,返回Shell界面     |
| prefix + D     | 选择要脱离的会话；在同时开启了多个会话时使用     |
| prefix + Ctrl+z     | 挂起当前会话     |
| prefix + :     | 进入命令行模式,此时可以输入tmux支持的命令     |
| **prefix + [**     | **进入复制模式；此时的操作与vi相同，按q/Esc退出**     |


## Session management
### check tmux version
``` bash
tmux -V
```

### open a tmux session and name it myname
``` bash
tmux new -s myname
```

### attache to an existing tmux session
``` bash
tmux a -t session_name  
```

### detach tmux session
``` bash
# 方式一
exit   # 如果只有单个tmux窗口,这样退出后tmux session就不存在了

# 方式二
prefix + d # d = detach, session保留
```

### list sessions
``` bash
tmux ls
0: 1 windows (created Wed Dec  5 16:14:47 2018) [232x56]
2: 2 windows (created Wed Dec  5 19:50:16 2018) [265x58]
```

## Windows
### create window
```
prefix + c
```

### select window
```
prefix + [0~9]
```

### rename window
```
prefix + ,
```

### kill window
```
prefix + &
```

## Panes
水平分隔窗格
```
prefix + -
```

垂直分隔窗格
```
prefix + |
```

移动面板
```
prefix + hjkl
prefix + ↑↓←→
```

置换面板
```
prefix + { # 向前置换
prefix + } # 向后置换
```

调整面板边界
```
prefix + KJHL # 上下左右调整面板边界
```








