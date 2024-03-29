# Copyright (c) 2020 Huawei Device Co., Ltd.
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.


ifeq ($(LOCAL_HCS_ROOT),)
  LOCAL_HCS_ROOT := $(PRODUCT_PATH)
endif

SOURCE_ROOT:=$(abspath ../../../../../)

HC_GEN_DIR := $(abspath $(SOURCE_ROOT)/drivers/framework/tools/hc-gen)
HC_GEN := $(HC_GEN_DIR)/build/hc-gen
LOCAL_HCS_ROOT := $(abspath $(dir $(realpath $(lastword $(MAKEFILE_LIST)))))

HCS_DIR := $(LOCAL_HCS_ROOT)

$(info HCS_DIR = $(HCS_DIR))
HCB_FLAGS := -b -i -a

HCS_OBJ := hdf_hcs_hex.o
HCS_OBJ_SRC := $(subst .o,.c,$(notdir $(HCS_OBJ)))

CONFIG_GEN_HEX_SRC := $(addprefix $(LOCAL_HCS_ROOT)/, $(HCS_OBJ_SRC))
CONFIG_HCS_SRC := $(subst _hcs_hex.o,.hcs,$(addprefix $(HCS_DIR)/, $(HCS_OBJ)))

$(obj)/$(HCS_OBJ): $(CONFIG_GEN_HEX_SRC)
	$(Q)$(CC) $(c_flags) -c -o $@ $<
	$(Q)rm -f $<

$(CONFIG_GEN_HEX_SRC):  $(LOCAL_HCS_ROOT)/%_hcs_hex.c: $(HCS_DIR)/%.hcs | $(HC_GEN)
	$(Q)echo gen hdf built-in config
	$(Q)if [ ! -d $(dir $@) ]; then mkdir -p $(dir $@); fi
	$(Q)$(HC_GEN) $(HCB_FLAGS) -o $(subst _hex.c,,$(@)) $<

$(CONFIG_GEN_SRCS): $(CONFIG_OUT_DIR)%.c: $(HCS_DIR)/%.hcs | $(HC_GEN)
	$(Q)echo gen hdf driver config
	$(Q)if [ ! -d $(dir $@) ]; then mkdir -p $(dir $@); fi
	$(Q)$(HC_GEN) -t -o $@ $<

$(HC_GEN):
	$(HIDE)make -C $(HC_GEN_DIR)

obj-$(CONFIG_DRIVERS_HDF) += $(HCS_OBJ)

