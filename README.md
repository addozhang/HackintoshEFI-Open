## 写在最前

**`opencore` 的配置中三码信息是留空的，需要自己生成配置进去（修改配置可用 `PropertyTree`）。**

##正在用的硬件信息

| 硬件    | 名称                                          |
|-------|---------------------------------------------|
| 主机    | 华擎（ASRock）DeskMini 310/COM                  |
| CPU   | QN8H(I7-8700 ES)拆机器                         |
| 内存    | 十铨（Team）DDR4 2666 16GB 笔记本内存条 * 2           |
| 硬盘    | 西部数据（Western Digital）500GB SSD固态硬盘 M.2接口    |
| 网卡    | 无线网卡4.0蓝牙MAC黑苹果免驱 (BCM94360CS2 + 转卡 + 外置天线) |
| CPU风扇 | 原装英特尔铜芯cpu散热器 4线温控                          |

## 安装盘制作

### 制作安装镜像 dmg 并写入制作 U 盘启动

```shell
#不需要这一步，这一步用于创建 dmg 镜像
hdiutil create -o ~/Downloads/macOS\ Catalina -size 8500m -layout SPUD -fs HFS+J

#使用这一步可以直接刷入 U 盘
sudo /Applications/Install\ macOS\ Catalina.app/Contents/Resources/createinstallmedia --volume /Volumes/macOS\ Catalina
```

### 挂载 EFI 分区

```shell
git clone https://github.com/corpnewt/MountEFI
cd MountEFI
chmod +x MountEFI.command

#然后选择要挂载的盘
```

### 生成三码，用于 iService

```shell
git clone https://github.com/corpnewt/GenSMBIOS
cd GenSMBIOS
chmod +x GenSMBIOS.command
```

输入 `1` 再输入 `3`， 使用 `iMac9,1` 或者 `iMac9,1 20` （20：一次输出 20 组）
```
#######################################################
#               iMac19,1 SMBIOS Info                  #
#######################################################

Type:         iMac19,1
Serial:       xxxxxxx
Board Serial: xxxxxxx
SmUUID:       xxxxxxx
```

### BIOS设置/BIOS settings

* Load UEFI Defaults
* Advanced
    * Onboard HD Audio: Enabled
    * USB Configuration, XHCI Hand-off, Enabled  （关键）
    * Super IO Configuration, Serial Port, Disabled（必须）
* Security Secure Boot, Disabled(by default)
* CSM disable
* CFG Lock: Disabled (关键，在cpu的c-state/status设置里）
* BIOS版本必须在4.0及以上！