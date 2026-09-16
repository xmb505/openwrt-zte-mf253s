include $(TOPDIR)/rules.mk

PKG_NAME:=zte-atfix
PKG_VERSION:=1.0.1
PKG_RELEASE:=2

PKG_SOURCE:=zte-mf253s-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://github.com/xmb505/zte-mf253s/archive/refs/tags/
PKG_SOURCE_URL_FILE:=v$(PKG_VERSION).tar.gz
PKG_HASH:=1b3cdd3b56dce8bf8099f25690cb69f9e163e8d3de492bf35891c44d9db98701

PKG_LICENSE:=GPL-2.0
PKG_MAINTAINER:=xmb505 <retromilia@foxmail.com>
PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/kernel.mk
include $(INCLUDE_DIR)/package.mk

TAR_OPTIONS+= --strip-components 1
TAR_CMD=$(HOST_TAR) -C $(1) $(TAR_OPTIONS)

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
