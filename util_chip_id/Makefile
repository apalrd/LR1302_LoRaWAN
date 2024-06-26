### User defined build options

ARCH ?=
CROSS_COMPILE ?=
BUILD_MODE := release
OBJDIR = obj

### ----- AVOID MODIFICATIONS BELLOW ------ AVOID MODIFICATIONS BELLOW ----- ###

ifeq '$(BUILD_MODE)' 'alpha'
  $(warning /\/\/\/ Building in 'alpha' mode \/\/\/\)
  WARN_CFLAGS   :=
  OPT_CFLAGS    := -O0
  DEBUG_CFLAGS  := -g
  LDFLAGS       :=
else ifeq '$(BUILD_MODE)' 'debug'
  $(warning /\/\/\/  Building in 'debug' mode \/\/\/\)
  WARN_CFLAGS   := -Wall -Wextra
  OPT_CFLAGS    := -O2
  DEBUG_CFLAGS  := -g
  LDFLAGS       :=
else ifeq  '$(BUILD_MODE)' 'release'
  $(warning /\/\/\/  Building in 'release' mode \/\/\/\)
  WARN_CFLAGS   := -Wall -Wextra
  OPT_CFLAGS    := -O2 -ffunction-sections -fdata-sections
  DEBUG_CFLAGS  :=
  LDFLAGS       := -Wl,--gc-sections
else
  $(error BUILD_MODE must be set to either 'alpha', 'debug' or 'release')
endif

### Application-specific variables
APP_NAME := chip_id
APP_LIBS := -lloragw -lm -ltinymt32 -lrt

### Environment constants
LIB_PATH := ../libloragw

### Expand build options
CFLAGS := -std=c99 $(WARN_CFLAGS) $(OPT_CFLAGS) $(DEBUG_CFLAGS)
CC := $(CROSS_COMPILE)gcc
AR := $(CROSS_COMPILE)ar

### General build targets
all: $(APP_NAME)

clean:
	rm -f obj/*.o
	rm -f $(APP_NAME)

$(OBJDIR):
	mkdir -p $(OBJDIR)

### Compile main program
$(OBJDIR)/$(APP_NAME).o: src/$(APP_NAME).c | $(OBJDIR)
	$(CC) -c $< -o $@ $(CFLAGS) -Iinc -I../libloragw/inc

### Link everything together
$(APP_NAME): $(OBJDIR)/$(APP_NAME).o $(LIB_PATH)/libloragw.a
	$(CC) -L$(LIB_PATH) -L../libtools $^ -o $@ $(LDFLAGS) $(APP_LIBS)

### EOF
