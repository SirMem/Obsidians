---
Project: "[[Linux]]"
Status: 🟩
tags:
  - Resources
Deadline: 2024-07-22
CreateTime: 2024-07-22
Connected:
---
## Introduce
Screen是终端会话管理工具,允许你在后台运行并重新连接到正在运行的会话

## how to use
### Create a conversation
```bash
screen -S name
```

### Reconnection
```bash
screen -r name
```

### show the list
```bash
screen -ls
```

### leaving screen
按 `Ctrl+a` 然后按 `d` 键