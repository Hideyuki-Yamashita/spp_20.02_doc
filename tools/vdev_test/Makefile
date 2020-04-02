# SPDX-License-Identifier: BSD-3-Clause
# Copyright(c) 2020 Nippon Telegraph and Telephone Corporation

ifeq ($(RTE_SDK),)
$(error "Please define RTE_SDK environment variable")
endif

# Default target, can be overridden by command line or environment
RTE_TARGET ?= x86_64-native-linuxapp-gcc

include $(RTE_SDK)/mk/rte.vars.mk

# binary name
APP = vdev_test

# all source are stored in SRCS-y
SRCS-y := vdev_test.c

CFLAGS += -O3
CFLAGS += $(WERROR_FLAGS)

SPP_DRIVERS_DIR = $(BASE_OUTPUT)/src/drivers
EXTRA_LDLIBS = -L$(SPP_DRIVERS_DIR)/pipe --whole-archive -lrte_pmd_spp_pipe --no-whole-archive

include $(RTE_SDK)/mk/rte.extapp.mk
