# 为 NekoBox Linux 设置无感 Tun 提升

免输入密码使用 VPN / Tun 模式。

## TL;DR

1. `sudo setcap cap_net_admin=ep /path/to/nekobox_core`
2. 替换 pkexec 为自己的 wrapper 脚本，实现免输入密码。
3. 修改 .desktop 文件

## 第一步

此步骤为 `nekobox_core` 设置网络管理员权限，普通用户的进程拥有此权限就可以创建 Tun 接口。

`sudo setcap cap_net_admin=ep /path/to/nekobox_core`

!!! note "提示"

    当您更新 nekoray 后，可能需要重新进行这一步。

## 第二步

此步骤替换 pkexec 为自己的 wrapper 脚本，因为目前 NekoBox 没有适配其他提权方式。

参考脚本内容

```shell
#!/bin/sh

if [ $1 = "--help" ]; then
  exit
fi

TO_EXEC="$@"

if [ $1 = "--keep-cwd" ]; then
  TO_EXEC="${@:2}"
fi

$TO_EXEC
```

在终端执行以下步骤

1. 将脚本命名为 `pkexec` 并写入某个位置，比如 `/opt/nekoray/pkexec`
2. 设置执行权限 `chmod 0755 /opt/nekoray/pkexec`
3. 设置环境变量 `export PATH=/opt/nekoray:$PATH`
4. 运行 `pkexec --help` 若无任何输出和报错，则前面步骤成功。
5. 使用此终端启动 NekoBox，应该可以正常使用 VPN 模式，且无需输入密码。

## 保存快捷方式和开机启动

以“跟随系统启动”功能生成的 .desktop 文件为例

文件位置：`~/.config/autostart/nekoray.desktop`

修改的地方：

1. 运行时要带上 PATH `PATH=/opt/nekoray:$PATH`
2. 添加 flag 使 NekoBox 认为处于 Admin 状态 `-flag_linux_run_core_as_admin`

```ini
[Desktop Entry]
Name=nekoray
Exec=sh -c "PATH=/opt/nekoray:$PATH /path/to/nekoray -tray -flag_linux_run_core_as_admin"
Terminal=false
Categories=Network
Type=Application
StartupNotify=false
X-GNOME-Autostart-enabled=true
```
