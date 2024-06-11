---
Project: "[[4-Archives/Project of Archives/Linux|Linux]]"
Status: 
tags:
  - Resources
Deadline: 
CreateTime: 2024-06-03
Connected: 
---
好的，接下来我们将详细讲解Linux的用户和权限管理，包括用户账户管理、组管理和文件权限管理。

### 用户管理

#### 添加用户

1. **`adduser`**：添加新用户并自动创建用户的主目录
   ```bash
   sudo adduser username
   ```

   示例：
   ```bash
   sudo adduser user1
   ```

   系统会提示输入和确认密码，并填写一些用户信息。

2. **`useradd`**：添加新用户，需要手动创建主目录
   ```bash
   sudo useradd -m username
   ```

   示例：
   ```bash
   sudo useradd -m user2
   sudo passwd user2  # 设置用户密码
   ```
3. 设置用户 `john` 的密码：`sudo passwd john`

#### 删除用户

1. **`deluser`**：删除用户
   ```bash
   sudo deluser username
   ```

   示例：
   ```bash
   sudo deluser user1
   ```

   使用`--remove-home`选项同时删除用户的主目录：
   ```bash
   sudo deluser --remove-home user1
   ```

2. **`userdel`**：删除用户
   ```bash
   sudo userdel username
   ```

   示例：
   ```bash
   sudo userdel user2
   ```

   使用`-r`选项同时删除用户的主目录：
   ```bash
   sudo userdel -r user2
   ```

#### 修改用户

1. **`usermod`**：修改用户账户信息
   ```bash
   sudo usermod [选项] username
   ```

   示例：
   - 更改用户名：
     ```bash
     sudo usermod -l new_username old_username
     ```

   - 更改用户的主目录：
     ```bash
     sudo usermod -d /new/home/directory -m username
     ```

   - 添加用户到组：
     ```bash
     sudo usermod -aG groupname username
     ```

### 组管理

#### 添加组

1. **`addgroup`**：添加新组
   ```bash
   sudo addgroup groupname
   ```

   示例：
   ```bash
   sudo addgroup group1
   ```

2. **`groupadd`**：添加新组
   ```bash
   sudo groupadd groupname
   ```

   示例：
   ```bash
   sudo groupadd group2
   ```

#### 删除组

1. **`delgroup`**：删除组
   ```bash
   sudo delgroup groupname
   ```

   示例：
   ```bash
   sudo delgroup group1
   ```

2. **`groupdel`**：删除组
   ```bash
   sudo groupdel groupname
   ```

   示例：
   ```bash
   sudo groupdel group2
   ```

#### 修改组

1. **`gpasswd`**：管理组成员
   - 添加用户到组：
     ```bash
     sudo gpasswd -a username groupname
     ```

     示例：
     ```bash
     sudo gpasswd -a user1 group1
     ```

   - 从组中移除用户：
     ```bash
     sudo gpasswd -d username groupname
     ```

     示例：
     ```bash
     sudo gpasswd -d user1 group1
     ```

### 权限管理


Linux使用文件权限来控制对文件和目录的访问。每个文件和目录都有三个权限设置：读（r）、写（w）、执行（x），分别适用于所有者（User）、所属组（Group）和其他用户（Other）。

#### 查看权限

1. **`ls -l`**：显示文件和目录的详细信息，包括权限
   ```bash
   ls -l
   ```

   输出示例：
   ```plaintext
   -rw-r--r-- 1 user group 4096 Jun 1 12:34 file1.txt
   ```

   解释：
   - `-rw-r--r--`：文件权限
     - 第一个字符表示文件类型（`-`表示普通文件，`d`表示目录）
     - 接下来的九个字符表示权限，分别为用户、组和其他用户的权限

#### 修改权限

1. **`chmod`**：改变文件或目录的权限
   ```bash
   chmod 755 file_name  # 设置文件权限为755（rwxr-xr-x）
   通过`chmod 755 file_name`，我们将文件`file_name`的权限设置为：
- 所有者：读、写、执行（7）
- 所属组：读、执行（5）
- 其他用户：读、执行（5）
   ```

   示例：
   ```bash
   chmod 644 file1.txt  # 设置文件权限为644（rw-r--r--）
   ```

   数字权限：
   - `r`（读）= 4
   - `w`（写）= 2
   - `x`（执行）= 1

   权限组合：
   - `7`（rwx）= 4 + 2 + 1
   - `6`（rw-）= 4 + 2
   - `5`（r-x）= 4 + 1
   - `4`（r--）= 4

2. **`chown`**：改变文件或目录的所有者
   ```bash
   chown user:group file_name  # 改变文件所有者和所属组
   ```

   示例：
   ```bash
   chown user1:usergroup file1.txt  # 将file1.txt的所有者改为user1，组改为usergroup
   ```


### 示例操作

以下是一些实际操作示例，以帮助您更好地理解：

#### 添加用户和组

1. 添加用户：
   ```bash
   sudo adduser newuser
   ```

2. 创建组：
   ```bash
   sudo addgroup newgroup
   ```

3. 将用户添加到组：
   ```bash
   sudo usermod -aG newgroup newuser
   ```

#### 设置权限

1. 设置文件权限为755：
   ```bash
   chmod 755 file1.txt
   ```

2. 给文件所有者添加写权限：
   ```bash
   chmod u+w file1.txt
   ```

3. 设置目录及其所有内容的权限：
   ```bash
   chmod -R 755 directory_name
   ```

#### 修改所有者和组

1. 修改文件的所有者为`newuser`，组为`newgroup`：
   ```bash
   sudo chown newuser:newgroup file1.txt
   ```

2. 修改目录及其所有内容的所有者和组：
   ```bash
   sudo chown -R newuser:newgroup directory_name
   ```

### 总结

通过这些命令，您可以有效地管理Linux系统中的用户、组和文件权限。掌握这些基本操作将帮助您更好地控制和管理系统的访问权限，确保系统安全性和多用户协作。如果您有其他问题或需要进一步的解释，请随时提问！