
include $(TOPDIR)/rules.mk

PKG_NAME:=alljoyn-lighting-service_framework
PKG_VERSION:=14.12.00

PKG_SOURCE_PROTO:=git
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_URL:=https://github.com/fonlabs/service_framework
PKG_SOURCE_VERSION:=252743190461543dbdb476c3c6cd799714747173
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION)-$(PKG_SOURCE_VERSION).tar.gz

# SCons supports parallel builds but does not support make's jobserver
MAKE_J:=-j$(CONFIG_PKG_BUILD_JOBS)

ifneq ($(CONFIG_CCACHE),)
  TARGET_CC=$(TARGET_CC_NOCACHE)
  TARGET_CXX=$(TARGET_CXX_NOCACHE)
endif

include $(INCLUDE_DIR)/package.mk

define Package/$(PKG_NAME)
  TITLE:=AllJoyn Lighting service library
  SECTION:=net
  CATEGORY:=Network
  DEPENDS:=alljoyn +alljoyn-about +alljoyn-config +alljoyn-notification +alljoyn-services_common
  URL:=http://www.allseenalliance.org
  MAINTAINER:=AllSeen Alliance <allseen-core@lists.allseenalliance.org>
endef

define Package/$(PKG_NAME)/description
AllJoyn Lighting service library.
endef

ifeq ($(CONFIG_DEBUG),y)
  ALLJOYN_BUILD_VARIANT:=debug
else
  ALLJOYN_BUILD_VARIANT:=release
endif


PKG_INSTALL_DIR:=$(PKG_BUILD_DIR)/build/openwrt/openwrt/$(ALLJOYN_BUILD_VARIANT)/dist/cpp

TARGET_CFLAGS+=-fPIC
TARGET_CPPFLAGS+=-fPIC

define Build/Configure/Default
# Override default to do nothing
endef

define Build/Compile
	scons -C $(PKG_BUILD_DIR)/ \
		$(PKG_JOBS) \
		WS=off \
		V=1 \
		CPU=openwrt \
		OS=openwrt \
		"VARIANT=$(ALLJOYN_BUILD_VARIANT)" \
		BR=off \
		BT=off \
		ICE=off \
		"TARGET_CC=$$(TARGET_CC)" \
		"TARGET_CXX=$$(TARGET_CXX)" \
		"TARGET_CFLAGS=$$(TARGET_CFLAGS)" \
		"TARGET_CPPFLAGS=$$(TARGET_CPPFLAGS)" \
		"TARGET_PATH=$$(TARGET_PATH)" \
		"TARGET_LINKFLAGS=$$(TARGET_LDFLAGS)" \
		"TARGET_LINK=$$(TARGET_CC)" \
		"TARGET_AR=$$(TARGET_AR)" \
		"TARGET_RANLIB=$$(TARGET_RANLIB)" \
		"STAGING_DIR=$$(STAGING_DIR)"
endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include/alljoyn/
	$(CP) $(PKG_INSTALL_DIR)/inc/* $(1)/usr/include/
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/lib/* $(1)/usr/lib/
endef


define Package/$(PKG_NAME)/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/lib/*.so $(1)/usr/lib/
	$(INSTALL_DIR) $(1)/usr/bin
	$(CP) $(PKG_INSTALL_DIR)/bin/* $(1)/usr/bin/
endef


$(eval $(call BuildPackage,$(PKG_NAME)))
