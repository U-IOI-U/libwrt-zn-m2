include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/image.mk

ifeq ($(SUBTARGET),arm)
  KERNEL_LOADADDR := 0x41208000
  DTS_CPPFLAGS-y += -D__ARM32__
else
  KERNEL_LOADADDR := 0x41080000
  DTS_CPPFLAGS-y += -D__AARCH64__
endif

DTS_CPPFLAGS-y += -D__IPQ_MEM_PROFILE_256_MB__
DTS_CPPFLAGS-$(CONFIG_ATH11K_MEM_PROFILE_256M) += -DCONFIG_ATH11K_MEM_PROFILE_256M

define Device/Default
  PROFILES := Default
  DEVICE_DTS_DIR := ../dts
  DEVICE_DTS = $$(SOC)-$(lastword $(subst _, ,$(1)))
  DTS_CPPFLAGS += $(DTS_CPPFLAGS-y)
  IMAGES := sysupgrade.tar
  IMAGE/sysupgrade.tar := sysupgrade-tar | append-metadata
endef

define Device/FitImage
  KERNEL_SUFFIX := -fit-uImage.itb
  KERNEL = kernel-bin | lzma | fit lzma $$(KDIR)/image-$$(DEVICE_DTS).dtb
  KERNEL_NAME := Image
endef

define Device/UbiFit
  KERNEL_IN_UBI := 1
  IMAGES := nand-factory.ubi nand-sysupgrade.bin
  IMAGE/nand-factory.ubi := append-ubi
  IMAGE/nand-sysupgrade.bin := sysupgrade-tar | append-metadata
endef

define Device/cmcc_rax3000q
  $(call Device/FitImage)
  $(call Device/UbiFit)
  SOC := ipq5000
  DEVICE_VENDOR := CMCC
  DEVICE_MODEL := RAX3000Q
  BLOCKSIZE := 128k
  PAGESIZE := 2048
  DEVICE_DTS_CONFIG := config@mp02.1
  IMAGES := nand-factory.ubi nand-sysupgrade.bin
  DEVICE_PACKAGES := \
	ath11k-firmware-ipq5018 \
	ath11k-firmware-qcn6122 \
	ipq-wifi-cmcc_rax3000q
endef
TARGET_DEVICES += cmcc_rax3000q

define Device/redmi_ax3000
  $(call Device/FitImage)
  $(call Device/UbiFit)
  SOC := ipq5000
  DEVICE_VENDOR := Redmi
  DEVICE_MODEL := AX3000
  DEVICE_ALT0_VENDOR := Xiaomi
  DEVICE_ALT0_MODEL := CR880x
  DEVICE_ALT0_VARIANT := (M81 version)
  DEVICE_ALT1_VENDOR := Xiaomi
  DEVICE_ALT1_MODEL := CR880x
  DEVICE_ALT1_VARIANT := (M79 version)
  BLOCKSIZE := 128k
  PAGESIZE := 2048
  DEVICE_DTS_CONFIG := config@mp02.1
  IMAGES := nand-factory.ubi
  DEVICE_PACKAGES := \
	ATH11K_MEM_PROFILE_256M \
	ath11k-firmware-ipq5018 \
	ath11k-firmware-qcn6122 \
	ipq-wifi-redmi_ax3000
endef
TARGET_DEVICES += redmi_ax3000

define Device/xiaomi_cr881x
  $(call Device/FitImage)
  $(call Device/UbiFit)
  SOC := ipq5018
  DEVICE_VENDOR := Xiaomi
  DEVICE_MODEL := CR881x
  BLOCKSIZE := 128k
  PAGESIZE := 2048
  DEVICE_DTS_CONFIG := config@mp03.3
  IMAGES := nand-factory.ubi
  DEVICE_PACKAGES := \
	ATH11K_MEM_PROFILE_256M \
	ath11k-firmware-ipq5018 \
	ath11k-firmware-qcn6122 \
	ipq-wifi-xiaomi_cr881x
endef
TARGET_DEVICES += xiaomi_cr881x

$(eval $(call BuildImage))
