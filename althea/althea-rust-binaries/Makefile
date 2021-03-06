#
# Copyright (C) 2017-2018 Justin Kilpatrick, Stanislaw Drozd, Openwrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

# you must set $RUST_TRIPLE in your env
include $(TOPDIR)/rules.mk

PKG_NAME:=althea-rust-binaries
PKG_VERSION:=0.5.37
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/althea-mesh/althea_rs
PKG_SOURCE_VERSION:=2f20b205eb8a2d1a544768c86aea2a72ed4bf946
PKG_LICENSE:=AGPLv3

include $(INCLUDE_DIR)/package.mk

define Package/althea-rust-binaries
  SECTION:=net
  CATEGORY:=Utilities
  SUBMENU:=Althea
  TITLE:=Althea Rust executables
  URL:=https://github.com/althea-mesh/althea_rs
  MAINTAINER:= Justin Kilpatrick <justin@althea.net>
  DEPENDS:=@IPV6 +althea-babeld +ipset +kmod-wireguard +wireguard +rpcd +ca-bundle +ca-certificates +kmod-sched-cake +tc
  PKG_BUILD_DEPENDS:=openssl
endef

define Package/althea-rust-binaries/description
  Althea Rust executables
endef

ifeq ($(ARCH),arm)
    CROSS_PREFIX:=arm-openwrt-linux-muslgnueabi
    RUSTFLAGS:="-C linker=$(TARGET_CC_NOCACHE)"	
else ifeq ($(ARCH),mips)
    CROSS_PREFIX:=mips-openwrt-linux-musl
    RUSTFLAGS:="-C linker=$(TARGET_CC_NOCACHE)"	
else ifeq ($(ARCH),mips64)
    CROSS_PREFIX:=mips64-openwrt-linux
    RUSTFLAGS:="-C linker=$(TARGET_CC_NOCACHE)"	
else ifeq ($(ARCH),mipsel)
    CROSS_PREFIX:=mipsel-openwrt-linux-musl
    RUSTFLAGS:="-C linker=$(TARGET_CC_NOCACHE)"	
else ifeq ($(ARCH),x86_64)
    CROSS_PREFIX:=x86_64-openwrt-linux-musl
    RUSTFLAGS:="-C linker=$(TARGET_CC_NOCACHE)"	
else ifeq ($(ARCH),aarch64)
    CROSS_PREFIX:=aarch64-openwrt-linux-musl
    RUSTFLAGS:="-C linker=$(TARGET_CC_NOCACHE)"	
endif

RITA_PATH:="$(PKG_BUILD_DIR)/target/$(RUST_TRIPLE)/release/rita"

define Build/Compile
	(\
		cd $(PKG_BUILD_DIR)/rita && \
\
        OPENSSL_DIR=$(STAGING_DIR)/usr/ \
        TARGET=$(RUST_TRIPLE) \
        SQLITE3_LIB_DIR="$(STAGING_DIR)/usr/lib/" \
        CROSS_COMPILE=$(CROSS_PREFIX) \
        TARGET_CC=$(TARGET_CC_NOCACHE) \
        RUSTFLAGS=$(RUSTFLAGS) \
        OPENSSL_STATIC=1 \
\
		cargo build --release --target $(RUST_TRIPLE) --bin rita \
	)
	$(STRIP) $(RITA_PATH)
endef

define Package/althea-rust-binaries/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(RITA_PATH) $(1)/usr/sbin/rita
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/rita.init $(1)/etc/init.d/rita
endef

$(eval $(call BuildPackage,althea-rust-binaries))
