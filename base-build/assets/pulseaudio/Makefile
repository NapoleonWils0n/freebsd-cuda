#===============================================================================
# FILE: Makefile
# Project: pulseaudio
#===============================================================================


#===============================================================================
# PHONY
#===============================================================================

.PHONY: all clean


#===============================================================================
# persistent storage
#===============================================================================

CONFIG_DIR          := $${HOME}/.config/pulse
DEFAULT_FILE        := $(CONFIG_DIR)/default.pa


#===============================================================================
# default target
#===============================================================================

all: config default


#===============================================================================
# pulse
#===============================================================================

config:
	mkdir -p $(CONFIG_DIR)


#===============================================================================
# pulseaudio - default.pa
#===============================================================================

default: config
	@printf "%s\n" \
	"#!/usr/local/bin/pulseaudio -nF" \
	"" \
	"# include default.pa and override" \
	".include /usr/local/etc/pulse/default.pa" \
	"" \
	"# pulseaudio.socket" \
	".ifexists module-esound-protocol-unix.so" \
	"load-module module-esound-protocol-unix" \
	".endif" \
	"load-module module-native-protocol-unix socket=/tmp/pulseaudio.socket" > $(DEFAULT_FILE)
	@echo "PulseAudio configuration written to $(DEFAULT_FILE)"


#===============================================================================
# Clean target to wipe the structure out if you need to reset
#===============================================================================

clean:
	rm -f $(DEFAULT_FILE)
	rmdir $(CONFIG_DIR)
