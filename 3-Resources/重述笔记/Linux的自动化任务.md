---
Project: "[[Linux|Linux]]"
Status: 🟩
tags:
  - Resources
Deadline: 
CreateTime: 2024-06-03
Connected: 
---


### `cron` 配置定时任务

`cron`是Linux系统中的一个用于定时执行任务的守护进程。它可以在指定时间间隔内运行预定的任务。

#### 基本概念

`cron`使用的配置文件称为`crontab`（cron table），每个用户都有自己的`crontab`文件。

#### `crontab` 文件的格式

每行表示一个任务，包含以下字段：

```
* * * * * command_to_execute
- - - - -
| | | | |
| | | | ----- 星期几 (0 - 7) (0 和 7 都表示星期日)
| | | ------- 月份 (1 - 12)
| | --------- 日期 (1 - 31)
| ----------- 小时 (0 - 23)
------------- 分钟 (0 - 59)
```

#### 示例任务
- `/path/to/command` 是一个占位符，表示您要执行的命令或脚本的完整路径。在实际使用中，您需要替换它为具体的命令或脚本的路径。

1. **每分钟执行一次任务**：
   ```plaintext
   * * * * * /path/to/command
   ```

2. **每天凌晨2点执行任务**：
   ```plaintext
   0 2 * * * /path/to/command
   ```

3. **每周一凌晨3点执行任务**：
   ```plaintext
   0 3 * * 1 /path/to/command
   ```

4. **每月1号凌晨4点执行任务**：
   ```plaintext
   0 4 1 * * /path/to/command
   ```

5. **每年1月1号凌晨5点执行任务**：
   ```plaintext
   0 5 1 1 * /path/to/command
   ```

#### 管理 `crontab`

1. **编辑`crontab`文件**：
   ```bash
   crontab -e
   ```

2. **查看当前用户的`crontab`任务**：
   ```bash
   crontab -l
   ```

3. **删除当前用户的`crontab`任务**：
   ```bash
   crontab -r
   ```

4. **使用特定的`crontab`文件**：
   ```bash
   crontab my_cron_file
   ```

#### 示例操作

1. **编辑当前用户的`crontab`文件**：
   ```bash
   crontab -e
   ```

   在编辑器中添加以下行：
   ```plaintext
   0 2 * * * /path/to/backup_script.sh
   ```

2. **查看当前用户的`crontab`任务**：
   ```bash
   crontab -l
   ```

3. **删除当前用户的`crontab`任务**：
   ```bash
   crontab -r
   ```

### `at` 配置一次性任务

`at`命令用于在指定时间执行一次性任务。

#### 基本用法

1. **调度任务**：
   ```bash
   at TIME
   ```

   `TIME`可以是具体的时间格式，如`HH:MM`，或是相对时间格式，如`now + 1 hour`。

2. **输入命令**：
   在进入`at`命令的交互模式后，输入需要执行的命令，一行一条。输入`Ctrl+D`结束输入。

#### 示例任务

1. **在当天的下午2点执行任务**：
   ```bash
   echo "backup_script.sh" | at 14:00
   ```

2. **在当前时间的一小时后执行任务**：
   ```bash
   echo "backup_script.sh" | at now + 1 hour
   ```

3. **在明天的上午10点执行任务**：
   ```bash
   echo "backup_script.sh" | at 10:00 AM tomorrow
   ```

#### 管理 `at` 任务

1. **查看所有已调度的任务**：
   ```bash
   atq
   ```

2. **取消任务**：
   ```bash
   atrm JOB_ID
   ```

#### 示例操作

1. **调度任务在下午3点执行**：
   ```bash
   at 15:00
   ```

   进入交互模式后，输入：
   ```plaintext
   /path/to/backup_script.sh
   ```
   按`Ctrl+D`结束输入。

2. **查看所有已调度的任务**：
   ```bash
   atq
   ```

3. **取消任务**（假设任务ID为2）：
   ```bash
   atrm 2
   ```

### `systemd` 定时任务

在现代Linux系统中，`systemd`也提供了定时任务功能，使用`timer`单元。

#### 创建定时任务

1. **创建服务单元文件**（如`/etc/systemd/system/mytask.service`）：
   ```plaintext
   [Unit]
   Description=My scheduled task

   [Service]
   ExecStart=/path/to/command
   ```

2. **创建定时器单元文件**（如`/etc/systemd/system/mytask.timer`）：
   ```plaintext
   [Unit]
   Description=Run mytask.service every day

   [Timer]
   OnCalendar=*-*-* 02:00:00
   Persistent=true

   [Install]
   WantedBy=timers.target
   ```

3. **启动并启用定时器**：
   ```bash
   sudo systemctl start mytask.timer
   sudo systemctl enable mytask.timer
   ```

#### 示例操作

1. **创建服务单元文件**：
   ```bash
   sudo nano /etc/systemd/system/mytask.service
   ```

   文件内容：
   ```plaintext
   [Unit]
   Description=My scheduled task

   [Service]
   ExecStart=/path/to/backup_script.sh
   ```

2. **创建定时器单元文件**：
   ```bash
   sudo nano /etc/systemd/system/mytask.timer
   ```

   文件内容：
   ```plaintext
   [Unit]
   Description=Run mytask.service every day

   [Timer]
   OnCalendar=*-*-* 02:00:00
   Persistent=true

   [Install]
   WantedBy=timers.target
   ```

3. **启动并启用定时器**：
   ```bash
   sudo systemctl start mytask.timer
   sudo systemctl enable mytask.timer
   ```

### 总结

通过`cron`、`at`和`systemd`，您可以在Linux系统中实现自动化任务管理，定时执行或一次性执行特定任务。掌握这些工具的使用方法，将大大提高您在Linux系统上的任务管理效率和灵活性。如果您有其他问题或需要进一步的解释，请随时提问！