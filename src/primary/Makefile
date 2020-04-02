# SPDX-License-Identifier: BSD-3-Clause
# Copyright(c) 2015-2016 Intel Corporation

ifeq ($(RTE_SDK),)
$(error "Please define RTE_SDK environment variable")
endif

# Default target, can be overriden by command line or environment
RTE_TARGET ?= x86_64-native-linuxapp-gcc

include $(RTE_SDK)/mk/rte.vars.mk

ifneq ($(CONFIG_RTE_EXEC_ENV),"linuxapp")
$(error This application can only operate in a linuxapp environment, \
please change the definition of the RTE_TARGET environment variable)
endif

# binary name
APP = spp_primary

# TODO: revise to not use functions in secondary's.
SPP_SEC_DIR = ../shared/secondary
SPP_FLOW_DIR = ./flow
SPP_FLOW_SRC = flow.c attr.c common.c
SPP_FLOW_PTN_DIR = $(SPP_FLOW_DIR)/pattern
SPP_FLOW_PTN_SRC = eth.c vlan.c
SPP_FLOW_ACT_DIR = $(SPP_FLOW_DIR)/action
SPP_FLOW_ACT_SRC = jump.c queue.c of_push_vlan.c of_set_vlan_vid.c
SPP_FLOW_ACT_SRC += of_set_vlan_pcp.c

# all source are stored in SRCS-y
SRCS-y := main.c init.c args.c
SRCS-y += ../shared/common.c ../shared/basic_forwarder.c ../shared/port_manager.c
SRCS-y += $(SPP_SEC_DIR)/add_port.c
SRCS-y += $(SPP_SEC_DIR)/utils.c
SRCS-y += $(addprefix $(SPP_FLOW_DIR)/,$(SPP_FLOW_SRC))
SRCS-y += $(addprefix $(SPP_FLOW_PTN_DIR)/,$(SPP_FLOW_PTN_SRC))
SRCS-y += $(addprefix $(SPP_FLOW_ACT_DIR)/,$(SPP_FLOW_ACT_SRC))

INC := $(wildcard *.h)

CFLAGS += -DALLOW_EXPERIMENTAL_API
CFLAGS += $(WERROR_FLAGS) -O3 -MMD
CFLAGS += -I$(SRCDIR)/../

ifeq ($(CONFIG_RTE_BUILD_SHARED_LIB),y)
LDLIBS += -lrte_pmd_ring
endif

# for newer gcc, e.g. 4.4, no-strict-aliasing may not be necessary
# and so the next line can be removed in those cases.
EXTRA_CFLAGS += -fno-strict-aliasing

SPP_DRIVERS_DIR = $(BASE_OUTPUT)/src/drivers
EXTRA_LDLIBS = -L$(SPP_DRIVERS_DIR)/pipe --whole-archive -lrte_pmd_spp_pipe --no-whole-archive

include $(RTE_SDK)/mk/rte.extapp.mk
