include $(TOPDIR)/rules.mk

PKG_NAME:=zte-ecm
PKG_VERSION:=1.0.1
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/xmb505/zte-mf253s.git
PKG_SOURCE_VERSION:=ff3cc05c54f37fe27ef6b8de809fb64fc38dfbfb
PKG_MIRROR_HASH:=skip

PKG_LICENSE:=GPL-2.0
PKG_MAINTAINER:=xmb505 <retromilia@foxmail.com>
PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/kernel.mk
include $(INCLUDE_DIR)/package.mk

define KernelPackage/zte-ecm
  SUBMENU:=USB Support
  TITLE:=ZTE ZX297510 (MF253S) ECM data interface driver
  DEPENDS:=+kmod-usb-net +kmod-usb-net-cdc-ether
  FILES:=$(PKG_BUILD_DIR)/drivers/zte_ecm/zte_ecm.ko
  AUTOLOAD:=$(call AutoProbe,zte_ecm)
endef

define KernelPackage/zte-ecm/description
  usbnet driver for the ECM data interface (if1) of the ZTE MF253S /
  ME3760V2 (ZX297510) modem.  IPv6 frames are dropped in the data path;
  the IPv4 settings are learned from the +ZGIPDNS URC (handled by
  kmod-zte-atfix), there is no DHCP on the interface.
endef

define Build/Compile
	+$(KERNEL_MAKE) $(PKG_JOBS) \
		M="$(PKG_BUILD_DIR)/drivers/zte_ecm" \
		modules
endef

$(eval $(call KernelPackage,zte-ecm))
