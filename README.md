# openwrt-zte-mf253s

OpenWrt feed for the **ZTE MF253S / ME3760V2** (Sanechips ZX297510) 4G modem —
turns the operator-retired mSATA module into a native Linux mobile data card.

驱动源码、逆向笔记与完整文档：<https://github.com/xmb505/zte-mf253s>

## 软件包

| 包名 | 说明 | 依赖 |
|---|---|---|
| `kmod-zte-ecm` | ECM 数据口（if1）usbnet 驱动 | `kmod-usb-net` `kmod-usb-net-cdc-ether` |
| `kmod-zte-atfix` | AT 修复 + Icera 伪装 + 拨号翻译 + 链路管理 | `kmod-usb-serial` |

上层推荐搭配 stock `modemmanager`（NetworkManager 的移动数据界面、通知走
ModemManager GUI 即可）。

## 用法（源码构建）

在 OpenWrt / ImmortalWrt / iStoreOS 源码树中：

```bash
echo 'src-git zte https://github.com/xmb505/openwrt-zte-mf253s.git' >> feeds.conf.default
./scripts/feeds update zte
./scripts/feeds install -a -p zte
make menuconfig   # Kernel modules -> USB Support -> kmod-zte-ecm / kmod-zte-atfix
```

选上两个 kmod 和 `modemmanager` 后正常编译固件即可。

## OpenWrt 集成注意

装机组合：两个 kmod + 官方 feed 的 `modemmanager` + `luci-proto-modemmanager`
（LuCI 自带「Cellular Network」状态页），接口 proto 选 `modemmanager`。

实测（ImmortalWrt 24.10.6 / 内核 6.6.133）踩到 4 个**与驱动无关**的集成坑：
MM 的 hotplug 事件缓存、netifd 的 proto 表刷新、开机竞争（proto 先于 MM 探测）、
`/32` 承载的默认路由。细节与补丁全部记录在主仓 README 的
「在 OpenWrt / ImmortalWrt 上集成（实测记录）」一节：
<https://github.com/xmb505/zte-mf253s>

## 已验证平台

| 平台 | 内核 | 状态 |
|---|---|---|
| ImmortalWrt 24.10.6 (x86_64) | 6.6.133 | ✅ 实测通过：kmod 编译/安装、开机自启、MM 识别、拨号上网（中国移动，ping/HTTP 全通） |

## 许可

`Makefile` 与文档为 MIT；所打包的内核模块为 GPL-2.0（见源码头部 SPDX）。
