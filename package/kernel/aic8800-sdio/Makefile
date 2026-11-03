include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/kernel.mk

PKG_NAME:=aic8800-sdio
PKG_RELEASE:=1
PKG_BUILD_DEPENDS:=mac80211

include $(INCLUDE_DIR)/package.mk

define KernelPackage/aic8800-sdio
  SUBMENU:=Wireless Drivers
  TITLE:=AICSemi AIC8800D80 SDIO Wi-Fi
  DEPENDS:=+kmod-cfg80211 +@DRIVER_11AC_SUPPORT +@DRIVER_11AX_SUPPORT
  FILES:=$(PKG_BUILD_DIR)/aic8800_bsp/aic8800_bsp.ko \
         $(PKG_BUILD_DIR)/aic8800_fdrv/aic8800_fdrv.ko
  AUTOLOAD:=$(call AutoProbe,aic8800_bsp aic8800_fdrv)
endef

define Build/Prepare
	mkdir -p $(PKG_BUILD_DIR)
	$(CP) ./src/. $(PKG_BUILD_DIR)/
endef

NOSTDINC_FLAGS:= \
  $(KERNEL_NOSTDINC_FLAGS) \
  -I$(STAGING_DIR)/usr/include/mac80211-backport/uapi \
  -I$(STAGING_DIR)/usr/include/mac80211-backport \
  -I$(STAGING_DIR)/usr/include/mac80211/uapi \
  -I$(STAGING_DIR)/usr/include/mac80211 \
  -include backport/autoconf.h \
  -include backport/backport.h

define Build/Compile
	$(MAKE) -C "$(LINUX_DIR)" $(KERNEL_MAKE_FLAGS) \
	  M="$(PKG_BUILD_DIR)" \
	  NOSTDINC_FLAGS="$(NOSTDINC_FLAGS)" \
	  KCFLAGS="-Wno-error -DAIC_API_VERSION_CODE=459264" \
	  CONFIG_AIC_WLAN_SUPPORT=m \
	  modules
endef

$(eval $(call KernelPackage,aic8800-sdio))
