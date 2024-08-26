---
Project: "[[Linux]]"
Status: 
tags:
  - Resources
Deadline: 
CreateTime: 
Connected:
---
`alternatives`命令是CentOS中的一个工具，用于管理不同软件的多个版本。它允许系统管理员方便地设置默认的程序版本，例如选择默认的Java版本或编辑器版本。这个工具通过维护符号链接来实现这一点，使得系统可以根据需求切换到不同的软件版本。

### 常用的alternatives命令

#### 查看已配置的alternatives

```bash
alternatives --display name
```

例如，要查看当前的Java配置，可以使用：

```bash
alternatives --display java
```

#### 配置新的alternative

```bash
alternatives --install /usr/bin/name name /path/to/version priority
```

例如，要添加一个新的Java版本，可以使用：

```bash
alternatives --install /usr/bin/java java /usr/java/jdk1.8.0_202/bin/java 2000
```

#### 设置默认版本

```bash
alternatives --set name /path/to/version
```

例如，要将Java的默认版本设置为某个特定版本，可以使用：

```bash
alternatives --set java /usr/java/jdk1.8.0_202/bin/java
```

#### 交互式选择默认版本

```bash
alternatives --config name
```

例如，要交互式地选择默认的Java版本，可以使用：

```bash
alternatives --config java
```

#### 移除alternative

```bash
alternatives --remove name /path/to/version
```

例如，要移除一个特定的Java版本，可以使用：

```bash
alternatives --remove java /usr/java/jdk1.8.0_202/bin/java
```

### 示例

以下是一个完整的示例，展示如何使用alternatives命令来管理Java版本：

1. **添加新的Java版本**

```bash
alternatives --install /usr/bin/java java /usr/java/jdk1.8.0_202/bin/java 2000
```

2. **查看当前的Java配置**

```bash
alternatives --display java
```

3. **设置默认的Java版本**

```bash
alternatives --set java /usr/java/jdk1.8.0_202/bin/java
```

4. **交互式选择默认的Java版本**

```bash
alternatives --config java
```

5. **移除某个Java版本**

```bash
alternatives --remove java /usr/java/jdk1.8.0_202/bin/java
```

通过这些命令，系统管理员可以方便地管理不同版本的软件，确保系统使用的是期望的默认版本。
