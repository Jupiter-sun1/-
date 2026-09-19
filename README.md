# 文件夹保险箱

这是一个面向 Windows 的本地文件夹加密工具。它把整个文件夹压缩后使用密码加密为一个 `.fvault` 文件；只有输入正确密码，程序才会还原出可访问的工作目录。

## 功能

- 新建保险箱，可选择在成功后移除原明文文件夹
- 输入密码解锁，并自动用资源管理器打开工作目录
- 保存修改并重新锁定，成功后移除明文工作目录
- 修改保险箱密码
- 检测密码错误和文件篡改
- 不保存密码

## 安装与启动

推荐使用 Python 3.10 或更高版本：

```powershell
cd folder_vault
python -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
.\.venv\Scripts\python.exe main.py
```

在当前工作区中，也可以直接双击 `start.bat`，它会优先使用本项目或上级目录的 `.venv`。

## 便携版与跨电脑使用

已经构建好的便携版位于 `portable\FolderVault`。将整个 `FolderVault` 目录复制到另一台 64 位 Windows 10/11 电脑，双击其中的 `FolderVault.exe` 即可，无需安装 Python。

请务必复制整个目录，包括 `_internal` 子目录，不能只复制 EXE。`.fvault` 保险箱文件可以单独复制到其他电脑，并使用原密码解锁。

重新构建便携版：

```powershell
.\.venv\Scripts\python.exe -m pip install -r requirements-build.txt
.\build_portable.ps1
```

## 使用流程

1. 点击“新建保险箱”，选择要加密的文件夹和 `.fvault` 保存位置。
2. 设置至少 8 个字符的密码。
3. 要真正锁定原文件夹，确认“加密成功后移除原明文文件夹”。
4. 以后选择 `.fvault` 文件，点击“解锁并打开”并输入密码。
5. 编辑完成后回到程序，点击“保存并锁定”。

默认情况下，`资料.fvault` 会解锁为同一目录下的 `资料` 文件夹。

## 安全说明

- 加密使用 AES-256-GCM，密钥通过 scrypt 从密码派生；文件名和目录结构也位于加密数据内部。
- 这不是 Windows 资源管理器的驱动级拦截。正确流程是打开 `.fvault`，输入密码后使用临时工作目录，再手动保存并锁定。
- 解锁期间，当前 Windows 账户和本机上有相应权限的软件可以访问明文文件。
- 程序退出时不会记住密码，因此不会自动锁定；退出前请点击“保存并锁定”。
- 删除明文文件不能保证在 SSD、云同步、备份或文件系统快照中不可恢复。
- 密码不会被保存，也没有找回机制。请先备份重要数据，并牢记密码。

## 运行测试

```powershell
.\.venv\Scripts\python.exe -m unittest discover -s tests -v
```
