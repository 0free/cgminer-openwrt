
include $(TOPDIR)/rules.mk

PKG_NAME:=cgminer
PKG_VERSION:=4.11.1
PKG_RELEASE:=1

PKG_SOURCE:=cgminer-$(PKG_VERSION).tar.bz2
PKG_SOURCE_URL:=https://github.com/SaifAlSubhi/cgminer.git
PKG_HASH:=fab86431b481b129db39e5a968369ff24c5b6a3c9191425293f8388bc0aa19b4

PKG_SOURCE_PROTO:=git
PKG_SOURCE_VERSION:=$(PKG_VERSION)

PKG_FIXUP:=autoreconf
PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

define Package/cgminer
	TITLE:=cgminer
	SECTION:=utils
	CATEGORY:=Utilities
	ifeq ($(CONFIG_TARGET_brcm2708_RaspberryPi),)
		DEPENDS:=+libcurl +libpthread +jansson +udev +libusb-1.0
	else
		DEPENDS:=+libcurl +libpthread +jansson +udev +libusb-1.0 +libncurses
endif
endef

define Package/cgminer/description
Cgminer is a multi-threaded multi-pool GPU, FPGA and CPU miner with ATI GPU
monitoring, (over)clocking and fanspeed support for bitcoin and derivative
coins.
endef

define Package/cgminer/config
	menu "Configuration"
		depends on PACKAGE_cgminer
		source "$(SOURCE)/Config.in"
	endmenu
endef

TARGET_CFLAGS += 
TARGET_LDFLAGS += -L$(STAGING_DIR)/usr/lib

ifeq ($(CONFIG_CGMINER_AVALON1),y)
	CONFIGURE_ARGS += --enable-avalon
endif
ifeq ($(CONFIG_CGMINER_AVALON2),y)
	CONFIGURE_ARGS += --enable-avalon2
endif
ifeq ($(CONFIG_CGMINER_AVALON4),y)
	CONFIGURE_ARGS += --enable-avalon4
endif
ifeq ($(CONFIG_CGMINER_AVALON6),y)
	CONFIGURE_ARGS += --enable-avalon4
endif
ifeq ($(CONFIG_CGMINER_AVALON7),y)
	CONFIGURE_ARGS += --enable-avalon7 CFLAGS=-DSTRATUM_USER_AGENT=\\\"a7\\\"
endif
ifeq ($(CONFIG_CGMINER_AVALON8),y)
	CONFIGURE_ARGS += --enable-avalon8 CFLAGS=-DSTRATUM_USER_AGENT=\\\"a8\\\"
endif
ifeq ($(CONFIG_CGMINER_AVALON850),y)
	CONFIGURE_ARGS += --enable-avalon8 CFLAGS=-DSTRATUM_USER_AGENT=\\\"a8\\\"
endif
ifeq ($(CONFIG_CGMINER_AVALON8_LP),y)
	CONFIGURE_ARGS += --enable-avalon8 CFLAGS=-DSTRATUM_USER_AGENT=\\\"a8\\\"
endif
ifeq ($(CONFIG_CGMINER_AVALON9),y)
	CONFIGURE_ARGS += --enable-avalon9 CFLAGS=-DSTRATUM_USER_AGENT=\\\"a9\\\"
endif
ifeq ($(CONFIG_CGMINER_AVALON911),y)
	CONFIGURE_ARGS += --enable-avalon9 CFLAGS=-DSTRATUM_USER_AGENT=\\\"a9\\\"
endif
ifeq ($(CONFIG_CGMINER_AVALONLC3),y)
	CONFIGURE_ARGS += --enable-avalonlc3 CFLAGS=-DSTRATUM_USER_AGENT=\\\"a9\\\"
endif

ifeq ($(CONFIG_TARGET_brcm2708_RaspberryPi),)
	CONFIGURE_ARGS += --without-curses
endif

define Build/Compile
	$(call Build/Compile/Default)
	(cd $(PKG_BUILD_DIR) && $(TARGET_CC) -Icompat/jansson-2.13.1/src `pkg-config --cflags libusb-1.0` api-example.c -o cgminer-api;)
endef

define Package/cgminer/install

	$(INSTALL_DIR)		$(1)/usr/bin
	$(INSTALL_DIR)		$(1)/usr/lib
	$(INSTALL_DIR)		$(1)/etc/config
	$(INSTALL_DIR)		$(1)/etc/init.d
	$(INSTALL_DIR)		$(1)/etc/uci-defaults

	$(INSTALL_BIN)		./root-files/root/hasher.bit			$(1)

	$(INSTALL_BIN)		$(PKG_BUILD_DIR)/cgminer-api			$(1)/usr/bin
	$(INSTALL_BIN)		$(PKG_INSTALL_DIR)/usr/bin/cgminer		$(1)/usr/bin

	$(INSTALL_DATA)		./root-files/etc/uci-defaults/01-cgminer	$(1)/etc/uci-defaults

ifeq ($(CONFIG_CGMINER_AVALON1),y)
	$(INSTALL_BIN)		$(FILES_DIR)/cgminer-avalon1-monitor	$(1)/usr/bin/cgminer-monitor
	$(INSTALL_BIN)		$(FILES_DIR)/cgminer.avalon1.init		$(1)/etc/init.d/cgminer
	$(INSTALL_CONF)		$(FILES_DIR)/cgminer.avalon1.config		$(1)/etc/config/cgminer
endif

ifeq ($(CONFIG_CGMINER_AVALON2),y)
	$(INSTALL_BIN)		$(FILES_DIR)/cgminer-avalon2-monitor	$(1)/usr/bin/cgminer-monitor
	$(INSTALL_BIN)		$(FILES_DIR)/cgminer.avalon2.init		$(1)/etc/init.d/cgminer
	$(INSTALL_CONF)		$(FILES_DIR)/cgminer.avalon2.config		$(1)/etc/config/cgminer
endif

ifeq ($(CONFIG_CGMINER_AVALON4),y)
	$(INSTALL_BIN)		$(FILES_DIR)/cgminer-avalon4-monitor	$(1)/usr/bin/cgminer-monitor
	$(INSTALL_BIN)		$(FILES_DIR)/cgminer.avalon4.init		$(1)/etc/init.d/cgminer
	$(INSTALL_CONF)		$(FILES_DIR)/cgminer.avalon4.config		$(1)/etc/config/cgminer
	$(INSTALL_BIN)		$(FILES_DIR)/mm-avalon4-upgrade			$(1)/usr/bin/mmupgrade
endif

ifeq ($(CONFIG_CGMINER_AVALON6),y)
	$(INSTALL_BIN)		$(FILES_DIR)/cgminer-avalon6-monitor	$(1)/usr/bin/cgminer-monitor
	$(INSTALL_BIN)		$(FILES_DIR)/cgminer.avalon6.init		$(1)/etc/init.d/cgminer
	$(INSTALL_CONF)		$(FILES_DIR)/cgminer.avalon6.config		$(1)/etc/config/cgminer
	$(INSTALL_BIN)		$(FILES_DIR)/mm-avalon6-upgrade			$(1)/usr/bin/mmupgrade
endif

ifeq ($(CONFIG_CGMINER_AVALON7),y)
	$(INSTALL_BIN)		$(FILES_DIR)/cgminer-avalon7-monitor	$(1)/usr/bin/cgminer-monitor
	$(INSTALL_BIN)		$(FILES_DIR)/cgminer.avalon7.init		$(1)/etc/init.d/cgminer
	$(CP)				$(FILES_DIR)/cgminer.avalon7.config		$(1)/etc/config/cgminer
	$(INSTALL_BIN)		$(FILES_DIR)/mm-avalon7-upgrade			$(1)/usr/bin/mmupgrade
endif

ifeq ($(CONFIG_CGMINER_AVALON8),y)
	$(INSTALL_BIN)		$(FILES_DIR)/cgminer-avalon8-monitor	$(1)/usr/bin/cgminer-monitor
	$(INSTALL_BIN)		$(FILES_DIR)/cgminer.avalon8.init		$(1)/etc/init.d/cgminer
	$(CP)				$(FILES_DIR)/cgminer.avalon8.config		$(1)/etc/config/cgminer
	$(INSTALL_BIN)		$(FILES_DIR)/mm-avalon8-upgrade			$(1)/usr/bin/mmupgrade
endif

ifeq ($(CONFIG_CGMINER_AVALON850),y)
	$(INSTALL_BIN)		$(FILES_DIR)/cgminer-avalon8-monitor	$(1)/usr/bin/cgminer-monitor
	$(INSTALL_BIN)		$(FILES_DIR)/cgminer.avalon8.init		$(1)/etc/init.d/cgminer
	$(INSTALL_CONF)		$(FILES_DIR)/cgminer.avalon850.config	$(1)/etc/config/cgminer
	$(INSTALL_BIN)		$(FILES_DIR)/mm-avalon8-upgrade			$(1)/usr/bin/mmupgrade
endif

ifeq ($(CONFIG_CGMINER_AVALON8_LP),y)
	$(INSTALL_BIN)		$(FILES_DIR)/cgminer-avalon8-monitor	$(1)/usr/bin/cgminer-monitor
	$(INSTALL_BIN)		$(FILES_DIR)/cgminer.avalon8.init		$(1)/etc/init.d/cgminer
	$(INSTALL_CONF)		$(FILES_DIR)/cgminer.avalon8_lp.config		$(1)/etc/config/cgminer
	$(INSTALL_BIN)		$(FILES_DIR)/mm-avalon8-upgrade			$(1)/usr/bin/mmupgrade
endif

ifeq ($(CONFIG_CGMINER_AVALON9),y)
	$(INSTALL_BIN)		$(FILES_DIR)/cgminer-avalon9-monitor	$(1)/usr/bin/cgminer-monitor
	$(INSTALL_BIN)		$(FILES_DIR)/cgminer.avalon9.init		$(1)/etc/init.d/cgminer
	$(INSTALL_CONF)		$(FILES_DIR)/cgminer.avalon9.config		$(1)/etc/config/cgminer
	$(INSTALL_BIN)		$(FILES_DIR)/mm-avalon9-upgrade			$(1)/usr/bin/mmupgrade
endif

ifeq ($(CONFIG_CGMINER_AVALON911),y)
	$(INSTALL_BIN)		$(FILES_DIR)/cgminer-avalon9-monitor	$(1)/usr/bin/cgminer-monitor
	$(INSTALL_BIN)		$(FILES_DIR)/cgminer.avalon9.init		$(1)/etc/init.d/cgminer
	$(INSTALL_CONF)		$(FILES_DIR)/cgminer.avalon9.config		$(1)/etc/config/cgminer
	$(INSTALL_BIN)		$(FILES_DIR)/mm-avalon9-upgrade			$(1)/usr/bin/mmupgrade
endif

ifeq ($(CONFIG_CGMINER_AVALONLC3),y)
	$(INSTALL_BIN)		$(FILES_DIR)/cgminer-avalonlc3-monitor	$(1)/usr/bin/cgminer-monitor
	$(INSTALL_BIN)		$(FILES_DIR)/cgminer.avalonlc3.init		$(1)/etc/init.d/cgminer
	$(INSTALL_CONF)		$(FILES_DIR)/cgminer.avalonlc3.config	$(1)/etc/config/cgminer
	$(INSTALL_BIN)		$(FILES_DIR)/mm-avalonlc3-upgrade		$(1)/usr/bin/mmupgrade
endif

	$(CP)	./root-files/etc/avalon_version		$(1)/etc

	echo "Machine: " $(AVA_MACHINE)					>> $(1)/etc/avalon_version
	echo "Date: " $(DATE)							>> $(1)/etc/avalon_version
	echo "luci: " $(LUCI_GIT_VERSION)				>> $(1)/etc/avalon_version
	echo "cgminer: " $(GIT_VERSION)					>> $(1)/etc/avalon_version
	echo "cgminer-packages: " $(OW_GIT_VERSION)		>> $(1)/etc/avalon_version

endef

$(eval $(call BuildPackage,cgminer))
