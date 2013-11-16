# Copyright 2013 Dominic Spill
#
# This file is part of Project Daisho.
#
# This program is free software; you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation; either version 2, or (at your option)
# any later version.
#
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with this program; see the file COPYING.  If not, write to
# the Free Software Foundation, Inc., 51 Franklin Street,
# Boston, MA 02110-1301, USA.

PLATFORM = $(shell uname -m)
ifneq ($(PLATFORM), armv7l)
        CROSS_COMPILE ?= /opt/gcc-linaro-arm-linux-gnueabihf/bin/arm-linux-gnueabihf-
        CC = $(CROSS_COMPILE)gcc
        CXX = $(CROSS_COMPILE)g++
else
        CC = /usr/bin/gcc
        CXX = /usr/bin/g++
endif

INSTALL ?= /usr/bin/install
LDCONFIG ?= /sbin/ldconfig

OPTFLAGS   = -O3 -Wall
CFLAGS    += $(OPTFLAGS)

LIB_DIR ?= /usr/local/lib
INCLUDE_DIR ?= /usr/local/include
INSTALL_DIR ?= /usr/local/bin

LIB_NAME   = libusb-gadget.so
SONAME     = $(LIB_NAME).0
LIB_FILE   = $(SONAME).1

LIB_CFLAGS = $(CFLAGS) -shared -Wl,-soname,$(SONAME)

LIB_SOURCE_FILES = usb-gadget.c usbstring.c
LIB_OBJECT_FILES = $(LIB_SOURCE_FILES:%.c=%.o)
LIB_HEADER_FILES = usb-gadget.h

all: $(LIB_FILE)

$(LIB_FILE): $(LIB_SOURCE_FILES) $(LIB_HEADER_FILES)
	$(CC) $(CFLAGS) -fPIC -c $(LIB_SOURCE_FILES)
	$(CC) $(LIB_CFLAGS) -o $(LIB_FILE) $(LIB_OBJECT_FILES) $(LDFLAGS)

$(STATIC_LIB_FILE): $(LIB_FILE)
	$(AR) rcs $(STATIC_LIB_FILE) $(LIB_OBJECT_FILES)

install: $(LIB_FILE) $(TOOLS)
	$(INSTALL) -m 0644 $(LIB_HEADER_FILES) $(INCLUDE_DIR)
	$(INSTALL) -m 0644 $(LIB_FILE) $(LIB_DIR)
	ln -fs $(LIB_FILE) $(LIB_DIR)/$(LIB_NAME)
	ln -fs $(LIB_FILE) $(LIB_DIR)/$(SONAME)
	$(LDCONFIG)

clean:
	rm -f $(LIB_NAME).* $(LIB_OBJECT_FILES)

.PHONY: all clean install
