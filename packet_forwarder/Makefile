### Application-specific constants

APP_NAME := lora_pkt_fwd

### Environment constants

LGW_PATH ?= ../libloragw
LIB_PATH ?= ../libtools
ARCH ?=
CROSS_COMPILE ?=

OBJDIR = obj
INCLUDES = $(wildcard inc/*.h)

### External constant definitions
# must get library build option to know if mpsse must be linked or not

include $(LGW_PATH)/library.cfg
RELEASE_VERSION := `cat ../VERSION`

### Constant symbols

CC := $(CROSS_COMPILE)gcc
AR := $(CROSS_COMPILE)ar

CFLAGS := -O2 -Wall -Wextra -std=c99 -Iinc -I. -I../libtools/inc
VFLAG := -D VERSION_STRING="\"$(RELEASE_VERSION)\""

### Constants for Lora concentrator HAL library
# List the library sub-modules that are used by the application

LGW_INC =
ifneq ($(wildcard $(LGW_PATH)/inc/config.h),)
  # only for HAL version 1.3 and beyond
  LGW_INC += $(LGW_PATH)/inc/config.h
endif
LGW_INC += $(LGW_PATH)/inc/loragw_hal.h

### Linking options

LIBS := -lloragw -ltinymt32 -lparson -lbase64 -lrt -lpthread -lm

### General build targets

all: $(APP_NAME)

clean:
	rm -f $(OBJDIR)/*.o
	rm -f $(APP_NAME)

install:
	mkdir -p /etc/lora_pkt_fwd
	cp lora_pkt_fwd /usr/local/bin/
	cp reset_lgw.sh /usr/local/bin/
	chmod +x /usr/local/bin/reset_lgw.sh
	cp lora_pkt_fwd.service /etc/systemd/system/
	systemctl daemon-reload

install_conf:
	mkdir -p /etc/lora_pkt_fwd
	cp global_conf.json.sx1250.* /etc/lora_pkt_fwd/
	cp global_conf.json.sx1255.* /etc/lora_pkt_fwd/
	cp global_conf.json.sx1257.* /etc/lora_pkt_fwd/

### Sub-modules compilation

$(OBJDIR):
	mkdir -p $(OBJDIR)

$(OBJDIR)/%.o: src/%.c $(INCLUDES) | $(OBJDIR)
	$(CC) -c $(CFLAGS) -I$(LGW_PATH)/inc $< -o $@

### Main program compilation and assembly

$(OBJDIR)/$(APP_NAME).o: src/$(APP_NAME).c $(LGW_INC) $(INCLUDES) | $(OBJDIR)
	$(CC) -c $(CFLAGS) $(VFLAG) -I$(LGW_PATH)/inc $< -o $@

$(APP_NAME): $(OBJDIR)/$(APP_NAME).o $(LGW_PATH)/libloragw.a $(OBJDIR)/jitqueue.o
	$(CC) -L$(LGW_PATH) -L$(LIB_PATH) $< $(OBJDIR)/jitqueue.o -o $@ $(LIBS)

### EOF
