include $(TOPDIR)/rules.mk

PKG_NAME:=ath11k-firmware
PKG_SOURCE_DATE:=2023-03-31
PKG_SOURCE_VERSION:=a039049a9349722fa5c74185452ab04644a0d351
PKG_MIRROR_HASH:=ed401e3f6e91d70565b3396139193f7e815f410db93700697205ac8ed1b828c5
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/quic/upstream-wifi-fw
PKG_SOURCE_VERSION:=main
PKG_MIRROR_HASH:=skip

include $(INCLUDE_DIR)/package.mk

define generate-ath11k-firmware

  define Package/$(PKG_NAME)-$(1)
    SECTION:=firmware
    CATEGORY:=Firmware
    TITLE:=ath11k firmware binaries for $(call toupper,$(1))
  endef

  define Package/$(PKG_NAME)-$(1)/description
    ath11k firmware binaries for $(call toupper,$(1))
  endef

  define Package/$(PKG_NAME)-$(1)/install
	$$(INSTALL_DIR) $$(1)/lib/firmware/$(if $(2),$(2),$(call toupper,$(1)))
	$$(INSTALL_DATA) \
		$$(PKG_BUILD_DIR)/ath11k-firmware/*$(call toupper,$(1))*/hw*.0/$(PKG_VERSION)/WLAN.HK.$(PKG_VERSION)-$(PKG_RELEASE)-QCAHKSWPL_SILICONZ-1/$(3)*.* \
		$$(1)/lib/firmware/$(if $(2),$(2),$(call toupper,$(1)))/
  endef

  ALL_PACKAGE+=$(PKG_NAME)-$(1)

endef

$(eval $(call generate-ath11k-firmware,ipq5018))
$(eval $(call generate-ath11k-firmware,ipq6018))
$(eval $(call generate-ath11k-firmware,ipq8074))
$(eval $(call generate-ath11k-firmware,qcn6122,qcn6122,qcn6122/))
$(eval $(call generate-ath11k-firmware,qcn9074,qcn9074))

define Build/Compile
endef

QCN9074_BOARD_REV:=8e140c65f36137714b6d8934e09dcd73cb05c2f6
QCN9074_BOARD_FILE:=board-2.bin.$(QCN9074_BOARD_REV)

define Download/qcn9074-board
  URL:=https://github.com/kvalo/ath11k-firmware/raw/master/QCN9074/hw1.0/
  URL_FILE:=board-2.bin
  FILE:=$(QCN9074_BOARD_FILE)
  HASH:=dbf0ca14aa1229eccd48f26f1026901b9718b143bd30b51b8ea67c84ba6207f1
endef
$(eval $(call Download,qcn9074-board))

define Package/ath11k-firmware-ipq8074/install
	$(INSTALL_DIR) $(1)/lib/firmware/IPQ8074
	$(INSTALL_DATA) \
		$(PKG_BUILD_DIR)/ath11k-firmware/IPQ8074/hw2.0/2.9.0.1/WLAN.HK.2.9.0.1-01837-QCAHKSWPL_SILICONZ-1/* \
		$(1)/lib/firmware/IPQ8074/
endef

define Package/ath11k-firmware-qcn9074/install
	$(INSTALL_DIR) $(1)/lib/firmware/ath11k/QCN9074/hw1.0
	$(INSTALL_DATA) \
		$(PKG_BUILD_DIR)/ath11k-firmware/QCN9074/hw1.0/2.9.0.1/WLAN.HK.2.9.0.1-01837-QCAHKSWPL_SILICONZ-1/* \
		$(1)/lib/firmware/ath11k/QCN9074/hw1.0/
	$(INSTALL_BIN) \
		$(DL_DIR)/$(QCN9074_BOARD_FILE) $(1)/lib/firmware/ath11k/QCN9074/hw1.0/board-2.bin
endef

$(eval $(call BuildPackage,ath11k-firmware-ipq8074))
$(eval $(call BuildPackage,ath11k-firmware-qcn9074))
