#
# Copyright (C) 2006-2016 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/uclibc++.mk

PKG_NAME:=smartmontools
PKG_VERSION:=7.0
PKG_RELEASE:=3

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=@SF/smartmontools
PKG_HASH:=e5e1ac2786bc87fdbd6f92d0ee751b799fbb3e1a09c0a6a379f9eb64b3e8f61c

PKG_MAINTAINER:=Maxim Storchak <m.storchak@gmail.com>
PKG_LICENSE:=GPL-2.0-or-later
PKG_LICENSE_FILES:=COPYING

PKG_BUILD_PARALLEL:=1
PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

define Package/smartmontools/Default
  SECTION:=utils
  CATEGORY:=Utilities
  TITLE:=S.M.A.R.T Monitoring
  URL:=https://www.smartmontools.org/
endef

define Package/smartmontools
  $(call Package/smartmontools/Default)
  DEPENDS:=$(CXX_DEPENDS)
  TITLE+= Tool
endef

define Package/smartmontools/description
  smartmontools contains utility programs (smartctl) to
  control/monitor storage systems using the Self-Monitoring, Analysis
  and Reporting Technology System (S.M.A.R.T.) built into most modern
  ATA and SCSI disks. It is derived from smartsuite.
endef

define Package/smartd
  $(call Package/smartmontools/Default)
  DEPENDS:=$(CXX_DEPENDS)
  TITLE+= Daemon
endef

define Package/smartd/description
  smartmontools contains utility programs (smartd) to
  control/monitor storage systems using the Self-Monitoring, Analysis
  and Reporting Technology System (S.M.A.R.T.) built into most modern
  ATA and SCSI disks. It is derived from smartsuite.
endef

define Package/smartmontools-drivedb
  $(call Package/smartmontools/Default)
  TITLE+= Drive database
  PKGARCH=all
endef

define Package/smartmontools-drivedb/description
  Database of known drives and USB bridges for smartctl and smartd.
endef

ifeq ($(CONFIG_USE_UCLIBCXX),y)
TARGET_LDFLAGS +=-nodefaultlibs
else
CONFIGURE_VARS += with_cxx11_regex=yes
endif
TARGET_LDFLAGS += -flto

TARGET_CXXFLAGS +=-fno-rtti -flto
MAKE_FLAGS +=BUILD_INFO='"(localbuild)"'

CONFIGURE_ARGS += \
	--disable-fast-lebe \
	--without-gnupg \
	--without-libcap-ng \
	--without-libsystemd

#lower file size vs. CONFIGURE_ARGS
CONFIGURE_VARS += \
	with_smartdplugindir=no \
	with_systemdenvfile=no \
	with_systemdsystemunitdir=no \
	with_update_smart_drivedb=no

define Package/smartmontools/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/sbin/smartctl $(1)/usr/sbin/
endef

define Package/smartd/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/sbin/smartd $(1)/usr/sbin/
	$(INSTALL_DIR) $(1)/etc
	$(INSTALL_DATA) ./files/smartd.conf $(1)/etc/
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/smartd.init $(1)/etc/init.d/smartd
endef

define Package/smartmontools-drivedb/install
	$(INSTALL_DIR) $(1)/usr/share/smartmontools/
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/share/smartmontools/drivedb.h $(1)/usr/share/smartmontools/
endef

define Package/smartd/conffiles
/etc/smartd.conf
endef

$(eval $(call BuildPackage,smartmontools))
$(eval $(call BuildPackage,smartd))
$(eval $(call BuildPackage,smartmontools-drivedb))
