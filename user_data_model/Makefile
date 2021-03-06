##########################################################
# makefile for user-data-model
##########################################################


# source PolySync environment if not already done, assumes x86_64 if set here
# usually, the environment has these set
PSYNC_HOME ?= /usr/local/polysync
OSPL_HOME ?= $(PSYNC_HOME)/utils/x86_64.linux

# target
TARGET	:= bin/polysync-user-data-model-c

# sources
SRCS    :=  src/user_data_model.c

# object files, dep files
OBJS    := $(SRCS:.c=.o)
DEPS    := $(SRCS:.c=.dep)
XDEPS   := $(wildcard $(DEPS))

# get standard PolySync build resources
include $(PSYNC_HOME)/build_res.mk

# compiler
CC = gcc

# add our local pdm include before others
INCLUDE := -Ipdm $(INCLUDE)

# add node template library, must be first
LIBS := -L$(PSYNC_HOME)/lib -lpolysync_node $(LIBS)

# add data model
LIBS += -lpolysync_data_model

#
all: dirs build-pdm $(TARGET)

#
ifneq ($(XDEPS),)
include $(XDEPS)
endif

# directories
dirs::
	mkdir -p bin
	mkdir -p pdm

#
$(TARGET): $(OBJS)
	$(CC) $(LDFLAGS) $(INCLUDE) -o $@ $^ $(LIBS)

#
$(OBJS): %.o: %.c %.dep
	$(CC) $(CCFLAGS) $(INCLUDE) -o $@ -c $<

#
$(DEPS): %.dep: %.c Makefile
	$(CC) $(CCFLAGS) $(INCLUDE) -MM $< > $@

# add our example module to the complete data model
build-pdm:
	$(PSYNC_HOME)/bin/pdm-gen -c \
	$(PSYNC_HOME)/modules/dtc/dtc.idl \
	$(PSYNC_HOME)/modules/sensor/sensor.idl \
	$(PSYNC_HOME)/modules/navigation/navigation.idl \
	$(PSYNC_HOME)/modules/control/control.idl \
	idl/user_data_model.idl > pdm/version.txt

# show the results
	cat pdm/error.log

#
clean:
	-rm -f src/*.o
	-rm -f src/*.dep
	-rm -f $(TARGET)
	-rm -f bin/*
	-rm -rf pdm
	-rm -rf ospl-*.log
