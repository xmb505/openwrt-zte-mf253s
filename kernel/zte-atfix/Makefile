include $(TOPDIR)/rules.mk

PKG_NAME:=zte-atfix
PKG_VERSION:=1.0.0
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/xmb505/zte-mf253s.git
PKG_SOURCE_VERSION:=81a7e5fe796cbc88d9df2b1aad5b3c31818f832d
PKG_MIRROR_HASH:=skip

PKG_LICENSE:=GPL-2.0
PKG_MAINTAINER:=xmb505 <retromilia@foxmail.com>
PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/kernel.mk
include $(INCLUDE_DIR)/package.mk

define KernelPackage/zte-atfix
  SUBMENU:=USB Support
  TITLE:=ZTE ZX297510 (MF253S) AT fixup and link management
  DEPENDS:=+kmod-usb-serial
  FILES:=$(PKG_BUILD_DIR)/drivers/zte_atfix/zte_atfix.ko
  AUTOLOAD:=$(call AutoProbe,zte_atfix)
endef

define KernelPackage/zte-atfix/description
  usb-serial driver for the AT interfaces (if0/2/3) of the ZTE MF253S /
  ME3760V2 (ZX297510) modem: AT command fixups, ZTE Icera spoofing so
  stock ModemManager drives the ECM data port, private-port dial
  translation (AT+CGACT / AT+ZGACT), +ZGIPDNS IP cache and the keepalive
  polling the firmware needs.
endef

define Build/Compile
	+$(KERNEL_MAKE) $(PKG_JOBS) \
		M="$(PKG_BUILD_DIR)/drivers/zte_atfix" \
		modules
endef

$(eval $(call KernelPackage,zte-atfix))
