---
Project: "[[4-Archives/Project of Archives/Linux|Linux]]"
Status: 
tags:
  - Resources
Deadline: 
CreateTime: 2024-06-03
Connected: 
---


#### `ps` 命令

`ps`（process status）命令用于显示当前系统上的进程信息。

基本用法：
```bash
ps [选项]
```

示例：

1. 显示当前终端的进程：
   ```bash
   ps
   ```
   输出示例：
   ```plaintext
     PID TTY          TIME CMD
   12345 pts/0    00:00:00 bash
   12346 pts/0    00:00:00 ps
   ```

2. 显示所有进程：
   ```bash
   ps -e
   ```
   或者：
   ```bash
   ps aux
   ```

3. 显示以树状结构显示进程：
   ```bash
   ps -ef --forest
   ```

#### `top` 命令

`top` 命令动态显示系统上所有正在运行的进程，按CPU使用率排序。

基本用法：
```bash
top
```

示例：
```plaintext
top - 12:34:56 up 1 day,  2:34,  2 users,  load average: 0.05, 0.03, 0.01
Tasks: 123 total,   1 running, 122 sleeping,   0 stopped,   0 zombie
%Cpu(s):  0.3 us,  0.1 sy,  0.0 ni, 99.5 id,  0.0 wa,  0.0 hi,  0.1 si,  0.0 st
KiB Mem :  8123456 total,  123456 free,  234567 used,  345678 buff/cache
KiB Swap:  4123456 total,  123456 free,  234567 used.  345678 avail Mem

  PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND
 1234 root      20   0  123456  23456   3456 S   0.7  0.3   0:12.34 bash
 5678 user      20   0  234567  34567   4567 S   0.3  0.4   0:23.45 top
```

在`top`命令界面，可以使用以下按键进行交互：
- **`q`**：退出`top`
- **`k`**：杀死进程（需要输入PID）
- **`h`**：显示帮助
- **`P`**：按CPU使用率排序
- **`M`**：按内存使用率排序

#### `htop` 命令

`htop` 是 `top` 的一个交互式替代工具，提供更友好的用户界面。

基本用法：
```bash
htop
```

可以使用上下箭头选择进程，使用 `F9` 键杀死进程。

#### `pgrep` 命令

`pgrep` 命令用于查找符合条件的进程ID。

基本用法：
```bash
pgrep [选项] 模式
```

示例：

1. 查找所有名为`bash`的进程：
   ```bash
   pgrep bash
   ```

2. 查找所有由用户`user`拥有的进程：
   ```bash
   pgrep -u user
   ```

### 控制进程

#### `kill` 命令

`kill` 命令用于发送信号给进程，通常用于终止进程。

基本用法：
```bash
kill [信号] PID
```

常用信号：
- **`-15` (SIGTERM)**：终止进程，默认信号。
- **`-9` (SIGKILL)**：强制终止进程。
- **`-1` (SIGHUP)**：重新加载进程。

示例：

1. 终止进程：
   ```bash
   kill 1234
   ```

2. 强制终止进程：
   ```bash
   kill -9 1234
   ```

3. 重新加载进程：
   ```bash
   kill -1 1234
   ```

#### `killall` 命令

`killall` 命令用于杀死所有符合条件的进程。

基本用法：
```bash
killall [选项] 进程名
```

示例：

1. 终止所有名为`bash`的进程：
   ```bash
   killall bash
   ```

2. 强制终止所有名为`bash`的进程：
   ```bash
   killall -9 bash
   ```

#### `pkill` 命令

`pkill` 命令根据模式匹配进程名称，并发送信号。

基本用法：
```bash
pkill [选项] 模式
```

示例：

1. 终止所有名为`bash`的进程：
   ```bash
   pkill bash
   ```

2. 强制终止所有名为`bash`的进程：
   ```bash
   pkill -9 bash
   ```

#### `nice` 命令

`nice` 命令用于以指定优先级启动一个新进程。优先级范围从-20（最高优先级）到19（最低优先级）。

基本用法：
```bash
nice -n 优先级 命令
```

示例：

1. 以低优先级启动一个进程：
   ```bash
   nice -n 10 command
   ```

#### `renice` 命令

`renice` 命令用于修改正在运行的进程的优先级。

基本用法：
```bash
renice 优先级 -p PID
```

示例：

1. 修改进程优先级：
   ```bash
   renice 10 -p 1234
   ```

### 示例操作

以下是一些实际操作示例，以帮助您更好地理解：

#### 查看进程

1. 使用`ps`查看当前终端的进程：
   ```bash
   ps
   ```

2. 使用`top`动态查看系统上所有进程：
   ```bash
   top
   ```

3. 使用`pgrep`查找名为`bash`的进程：
   ```bash
   pgrep bash
   ```

#### 控制进程

1. 使用`kill`命令终止进程：
   ```bash
   kill 1234
   ```

2. 使用`killall`命令终止所有名为`bash`的进程：
   ```bash
   killall bash
   ```

3. 使用`nice`命令以低优先级启动进程：
   ```bash
   nice -n 10 command
   ```

4. 使用`renice`命令修改进程优先级：
   ```bash
   renice 10 -p 1234
   ```

